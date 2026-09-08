# The portal disclosure model

## Who this is for

You know C#, .NET, and this repo's Clean Architecture conventions. You do not know the External
Portal epic, or Portuguese disciplinary procedure. This note gives you both, at the depth you need to
pick up the next slice of `PortalDisclosure` work without asking around.

## The legal backdrop, in one paragraph

A Portuguese disciplinary process against an employee (the *arguido*) is only lawful if that person
can do two things: inspect the case file held against them (*consulta do processo*, phase 4 of 8 —
PRD-007), and respond to the charges in writing (*resposta do arguido*, phase 5 of 8 — PRD-008). Both
rights belong to someone who is not a user of this product. They have no account, no company
membership, and must never get one. Their counsel, if they retain one, is in the same position. The
capability that lets these two accountless people reach a case at all is called the External Portal
epic, internally "E08". It has been referenced by six-plus issues since the rebuild began, but as of
this writing E08 itself has not been cut into a concrete piece of work — only pieces that feed it have
landed. `PortalDisclosure` is one of those pieces.

## What problem `PortalDisclosure` solves

Before the arguido or their counsel can read anything, the firm has to decide, on its own side, which
case items to show them and for how long. `PortalDisclosure` is that firm-side decision, and nothing
more. Concretely, it answers three questions per item: is it visible, to whom, and until when.

It does **not** answer "how does the arguido actually read it" — that is a separate, not-yet-built
read path (issue #229), reached through a portal token this module knows nothing about. Keep that
split in your head throughout this note: `PortalDisclosure` is the firm's ledger of intent; #229 (or
whatever eventually implements it) is the door the arguido walks through. Confusing the two is the
easiest way to over-scope a future PR here.

## The aggregate: what one row means

`PortalDisclosure` (`backend/MyLegalTeam.Domain/Cases/ConsultaProcesso/PortalDisclosure.cs`) is a
small aggregate root. One row means: "this item, on this case, is visible from this instant, until
this instant (or forever), to this audience, with this download permission." Its fields:

- **`ItemId`** — deliberately untyped and unchecked. A "case item" can be a deliverable, an evidence
  row, or something else entirely; this module does not look it up or validate it against a real
  entity. Confirming the id actually refers to something real is the reading side's job, not this
  write's.
- **`VisibleFrom` / `VisibleUntil`** — the window. Both boundaries are inclusive. A null
  `VisibleUntil` means the window never closes on its own.
- **`Recipient`** — a nullable `PortalPartyRole` (`Employee` or `ExternalCounsel`, defined in
  `Domain/Cases/Portal/PortalPartyRole.cs`). Null means case-wide: every portal party on the case can
  see the item. A value narrows the disclosure to just that role. This is *role* grain, not
  *person* grain — publishing "to `ExternalCounsel`" reaches every counsel token on the case, because
  there is currently no entity representing an individual portal link. That entity (`PortalLink`, from
  issue #227) does not exist yet; when it does, per-link targeting can sit beside `Recipient` without
  breaking anything, because `Recipient` being nullable and defaulting to null makes every disclosure
  written today additive-compatible with that future.
- **`AllowDownload`** — a boolean, defaulting to false. This is a second, independent grant on top of
  visibility: a party can be allowed to *read* an item without being allowed to take a copy of it.
  Downloading is never implied by visibility; it has to be asked for explicitly.

Two behavioural points worth internalizing, because both are easy to get backwards:

1. **Publishing is additive, never a replace.** Publishing the same item again creates a further row.
   There is no update or unpublish operation on this aggregate. If an item ends up with two overlapping
   windows, both exist simultaneously, and the union of them governs visibility (see below).
2. **The party argument to the visibility check is never nullable.** `IsVisibleTo(party, instant)`
   requires a real party, on purpose. An overload that accepted "party unknown" would return `true` for
   every case-wide row — a fail-open shape that would be dangerous on an internet-facing read. Whatever
   eventually reads this state from outside always resolves a party from its token before asking, so it
   always has one to pass. Do not add a convenience overload that relaxes this.

The three query methods build on each other in one direction only:

```
IsVisibleAt(instant)                     — window only
IsVisibleTo(party, instant)              — window AND (case-wide OR recipient == party)
AllowsDownloadTo(party, instant)         — IsVisibleTo(...) AND AllowDownload
```

A download can never outlive or outrank the right to read: whatever the `AllowDownload` flag says, an
item out of window or addressed to a different party is not downloadable.

Because publishing is additive, "is this item visible to party X" really means "is there *any* row for
this item visible to X" — a case-wide row is not masked by a narrower row existing alongside it. That
union rule is something a future reader has to implement; `PortalDisclosure` only answers for one row
at a time.

## The two application-layer operations

Both live under
`backend/MyLegalTeam.Application/Features/Cases/ConsultaProcesso/`.

**`PublishPortalItemsCommand`** (in `PublishPortalItems/`) is the write. A caller who can manage the
case (`CallerAuthority.CanManageCases`, the same check `ChangeCaseFlagsCommandHandler` uses) submits a
set of item ids, a window, and optionally a recipient and a download flag. Authorization is checked
*before* the case is loaded, so an unauthorised caller touches nothing. A few details that matter if
you touch this code:

- Item ids are de-duplicated before any row is written — a repeated id in one request produces one
  disclosure, not two.
- The recipient rides the wire as a case-insensitive **name** (`"employee"`, `"externalCounsel"`),
  never as the enum's numeric value — there is no `JsonStringEnumConverter` configured anywhere in this
  service, so a bare enum would serialize as an integer and become unreadable in the audit trail. A
  malformed or unrecognized recipient is refused outright (`UnknownPortalRecipient`, a 400). It is
  never treated as "absent" and silently widened to case-wide — a typo must not accidentally publish to
  more people than the caller intended.
- `"witness"` is one of the values explicitly tested as rejected, and this is deliberate at the type
  level, not just at validation. A witness is a portal party in general (issue #20), but is not a party
  to *this* phase — the firm's internal brief only puts witnesses on the portal for the evidence phase,
  while the underlying labour-law article (art. 355.º/1 CT) gives the right to consult the file to the
  arguido specifically. `PortalPartyRole` simply has no `Witness` member, so "disclose to a witness" is
  unrepresentable rather than merely rejected at runtime.

**`GetPortalItemsQuery`** (in `Reads/`) is the list read. Its visibility bar is deliberately different
and broader than the publish bar above: any caller who may *view* the case can list what is published,
not just case managers. This mirrors how the Deliverables and Evidence reads work. A caller with no
standing on the case gets a 404, never a 403 — `CaseVisibility.ResolveVisibleCaseAsync` makes "this
case doesn't exist" and "this case exists but you have no role on it" indistinguishable on purpose, so
the endpoint is not an oracle for which case ids exist. Note also: this internal list is **not**
filtered by recipient. It is the firm's own view of everything it has published to anyone, and that
is pinned by a test — a future PR should not "fix" this into a per-party filter. Per-party filtering
belongs to whatever eventually reads this from the portal side.

## What this model deliberately does not do

Keep this list in mind before proposing to extend `PortalDisclosure` itself — several of the obvious
extensions belong somewhere else on purpose:

- **It does not serve content.** No path here streams bytes to anyone. `AllowDownload` is a grant
  recorded for later use; enforcing it (actually refusing bytes to a party who isn't allowed them) is
  the job of the not-yet-built external read path (#229).
- **It does not validate `ItemId` against a real artifact.** Cross-referencing an id to a real
  Deliverable or Evidence row is left to the reading side.
- **It does not gate on whether charges have been served.** There is a real product rule that
  disclosure should be blocked until charges are served (issue #59), but this module deliberately does
  not enforce it here. As of this writing, #59 is implemented (a `chargesNotDelivered` guard on a
  different transition, in an unmerged PR) as a gate on entering the consultation phase, not as a check
  inside publishing. Once that lands, do not duplicate the gate here — it belongs on the phase
  transition, one layer up.
- **It has no update or unpublish operation.** Revoking a disclosure once it is published is an
  explicitly undecided gap (tracked, not built).
- **It is not the access log.** Recording that a party actually *consulted* an item — the *registo de
  acesso* that becomes part of the court-defensible dossier — is a separate concern (issue #112), built
  on top of this state, not inside it.
- **It does not implement per-link targeting.** `Recipient` is a role, not an individual invitation.
  Distinguishing two different `ExternalCounsel` tokens on the same case needs the `PortalLink` entity
  (#227), which does not exist yet.

## What is still open

These are genuine open questions, not just unbuilt features — do not resolve them incidentally while
working on something else nearby:

- **Conflicting download grants across overlapping windows.** Because publishing is additive, one item
  can end up with two windows for the same party where one allows download and the other doesn't. There
  is no rule yet for which one governs. "Narrowest wins" is the candidate mentioned in the PRD, but it
  has not been chosen. `AllowsDownloadTo` intentionally answers for one row only, so nothing in this
  module implies an answer — a future reader must not assume `PortalDisclosure` has already decided
  this for it.
- **Does counsel's disclosure follow the arguido's automatically?** The relevant labour-law article
  names only the worker as the rights-holder and is silent on whether appointing a lawyer extends the
  same disclosure automatically, or whether the firm must grant it as a separate act. The model
  supports either today (publish twice, or publish case-wide) — this is a legal/product default
  question, not a code gap, and it is tracked for the firm to answer rather than guessed at.
- **Route shape and the #112 interlock.** Two different issues have proposed different route prefixes
  for the eventual access-log write, and #112 (the access log) and the portal read path each need
  something the other one owns — that circular dependency has not been untangled yet.
- **OTP, token delivery, and counsel provenance.** Whether a second factor is required beyond the
  opaque portal token, whether the token rides on an existing notification or is issued separately, and
  whether an `ExternalCounsel` link requires proof of representation to already exist on the case — all
  open, all outside this module's scope, but relevant if your next task touches how tokens get created.

## Where the next slice of work probably is

If you are picking this up cold, the two most concretely scoped continuations are:

- **Certidões (issue #62)** — certified copies, explicitly described as building on top of this
  disclosure state.
- **A "shareable case items" read** — enumerating what a case actually has available to disclose
  (deliverables, evidence, etc.), which was blocked before but no longer is now that Evidence exists on
  `dev`. This feeds the publish picker's `itemIds[]` input, which currently has no source.

Anything touching the external, unauthenticated read side (#229) is a much larger, still-undesigned
piece of work — read PRD-012 §6.3 and §8 in full before scoping anything there, since several of its
requirements depend on decisions this note lists as still open.
