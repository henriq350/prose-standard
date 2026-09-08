# The portal disclosure model

*A design note for a backend engineer picking up work on #61 or its neighbours (`ConsultaProcesso`,
`Domain/Cases/Portal`). You know C#, .NET, and this repo's Clean Architecture layering. You do not need
prior context on the External Portal epic or on Portuguese disciplinary procedure — both are introduced
below, once each, where they first matter.*

## The two Portuguese terms you need

A disciplinary case here runs through eight phases. Phase 4 is *consulta do processo* — "consultation
of the file" — the point where the accused worker, the **arguido**, is legally entitled to inspect the
file the firm has built against them (art. 355.º/1 CT, the Portuguese labour code). Making that file
available is called *disponibilização dos autos*, "making the record available." The arguido is not a
user of this product: no account, no company membership, and — per the PRD — must never acquire one.
They reach the firm's system only through a separate, unauthenticated surface called the **external
portal**. `PortalDisclosure` is the firm-side half of that surface: it is the record of *what the firm
has decided to make available*, not the thing that serves it.

That distinction — deciding disclosure versus serving content — is the single idea this whole model
turns on, so hold onto it before reading the fields below.

## What `PortalDisclosure` stores

`PortalDisclosure` (`Domain/Cases/ConsultaProcesso/PortalDisclosure.cs`) is one row per disclosed item.
Publishing the same item twice does not update a row — it adds a further one. There is no
update-or-replace operation on this aggregate, only additive disclosure; a later fact never erases an
earlier one. Five fields carry the whole model:

- **`ItemId`** — a `Guid`, deliberately opaque. `PortalDisclosure` does not know or check whether the id
  belongs to a real Deliverable, an Evidence row, or anything else — it mirrors the PRD's own abstract
  "case items" language. Cross-referencing the id against a concrete artifact is explicitly someone
  else's job (the read side, not yet built — see below).
- **`VisibleFrom` / `VisibleUntil`** — the visibility window. Both boundaries are inclusive. A null
  `VisibleUntil` means the window never closes.
- **`Recipient`** — a nullable `PortalPartyRole` (`Employee` | `ExternalCounsel`, defined in
  `Domain/Cases/Portal/PortalPartyRole.cs`). Null means **case-wide**: the item is disclosed to every
  portal party on the case. `PortalPartyRole` targets at **role** grain, not at a specific invited
  person — "the arguido" or "counsel," not "this particular counsel token." Distinguishing two counsel
  links on the same case needs an entity called `PortalLink` (issue #227), which does not exist yet; the
  PRD leaves that narrower question with the wider External Portal epic. Null is also the default, so
  the field is additive: every disclosure written before `Recipient` existed keeps meaning exactly what
  it did.
- **`AllowDownload`** — a `bool`, defaulting to `false`. Reading an item and taking a copy of it are
  treated as two separate grants, not one: a party can be permitted to view an item without being
  permitted to download it (PRD-012 SYS-REQ-302). The default is deliberately the narrow one — silence
  about downloading must not become permission to download, because that would put copies of a
  disciplinary file outside the system by accident rather than by decision.

`PortalPartyRole` itself is worth a second look, because what it *omits* is as deliberate as what it
contains. A third kind of portal party exists elsewhere in the system — a witness (*testemunha*) — but
`PortalPartyRole` has no `Witness` member. The firm's own product brief places a witness on the portal
only when summoned, which is a later phase (evidence-gathering), while art. 355.º/1 CT gives the
inspection right specifically to the arguido. Leaving `Witness` out makes "disclose the case file to a
witness" **unrepresentable** by the type system, rather than something checked and rejected at runtime.
If you ever find yourself wanting to add `Witness` here for a different phase, that is very likely the
wrong file: it needs its own grant, not this one.

## The three predicates

`PortalDisclosure` exposes three read-only questions, each built on the one before:

```
IsVisibleAt(instant)                → window open at instant, ignoring who's asking
IsVisibleTo(party, instant)         → window open AND (case-wide OR addressed to party)
AllowsDownloadTo(party, instant)    → IsVisibleTo(...) AND AllowDownload
```

Two things about `IsVisibleTo` are load-bearing, not incidental. First, `party` is **not nullable**.
That is a deliberate authorization-predicate design: an overload that accepted "party unknown" would
answer `true` for every case-wide row, which is a fail-open shape on a surface that will eventually face
the public internet. Whoever calls this method is expected to already have resolved a party from the
caller's token before asking whether they may see anything. Second, `AllowsDownloadTo` is expressed as a
conjunction with `IsVisibleTo`, not evaluated independently, so a download can never outlive or outrank
the underlying right to read: an item outside its window, or addressed to a different party, is not
downloadable regardless of how `AllowDownload` is set.

Because disclosure is additive, "is this item visible to party X" is really a question over **every**
row for that item: any row that is visible to X makes the item visible to X, and a case-wide row is not
masked by a narrower one existing alongside it. Whatever eventually reads this model (see below) has to
implement that union itself — `PortalDisclosure` answers only for one row.

## The application layer

Two operations sit in `Application/Features/Cases/ConsultaProcesso/`:

**`PublishPortalItemsCommand`** (in `PublishPortalItems/`) is the write. A case manager selects item ids,
a window, and optionally a recipient and a download grant. Authorization
(`CallerAuthority.CanManageCases`) is checked before the case is loaded, so an unauthorized caller
touches nothing — the same pattern as `ChangeCaseFlagsCommandHandler`. A repeated item id in one request
is disclosed once, not twice. `Recipient` travels the wire as a case-insensitive **name**
(`"employee"` / `"externalCounsel"`), not as the enum's ordinal: there is no `JsonStringEnumConverter`
configured anywhere in this codebase's `Program.cs`, so a raw enum would land in both the wire contract
and the audit trail as a bare integer. A recipient that parses to nothing — a typo, `"witness"`, a
numeric string — is refused with `UnknownPortalRecipient` (400). It never silently falls back to
case-wide, because that would disclose to *more* parties than the caller asked for.

**`GetPortalItemsQuery`** (in `Reads/`) is the list read behind `GET .../portal-items`. Its visibility
bar is deliberately looser than the write's: a caller only needs to be able to **view** the case, not
manage it — the same bar as the existing Deliverables and Evidence list reads, because reading published
state is a normal case-team action, not a management one. A caller with no standing on the case gets 404,
never 403; the distinction matters because a 403 would confirm the case exists at all, which is treated
as an information leak worth closing off (`CaseVisibility.ResolveVisibleCaseAsync` makes "unknown case"
and "case exists, no role on it" indistinguishable on purpose). This list is the firm's own view of
**everything** it has published on the case — it is not filtered by recipient. That is pinned by a test,
specifically so a later reader does not "fix" it into a per-party filter; per-party filtering belongs to
whatever eventually reads the portal from the outside (see below).

## What this model deliberately does not do

Everything past this point is excluded on purpose, cited to where the exclusion is recorded, so you can
tell which door is actually open.

- **It does not serve content.** `PortalDisclosure` decides what is disclosed; it has no code path that
  reads bytes off any item. `AllowsDownloadTo` is a grant, not an enforcement — refusing (or serving) the
  actual bytes belongs to an external, unauthenticated read path that has not been built (issue #229).
  The whole "portal" a naive reader imagines — a page the arguido opens — does not exist yet anywhere in
  this codebase.
- **It does not validate `ItemId`.** No check confirms the id refers to a real Deliverable, Evidence row,
  or anything else. That cross-reference belongs to the same not-yet-built read side.
- **It does not check that charges have been served first.** `BEHAVIOUR.PortalDisclosure.md` records this
  explicitly: disclosure is *nominally* blocked by a separate rule (issue #59, "charges must be served
  before disclosure") but this module does not gate on it. The reasoning is about *where* a gate belongs,
  not whether one should exist: #59 landed (PR #239, unmerged as of this writing) as a guard on the
  transition **into** the consultation phase, and `BEHAVIOUR.PortalDisclosure.md` records that as the
  intended place for it once #239 merges — a phase-transition guard, not a write-time check inside this
  aggregate.
- **It does not target a specific invited person**, only a role. Two `ExternalCounsel` tokens on the
  same case are indistinguishable to this model. Closing that gap needs the `PortalLink` entity (#227),
  which does not exist yet; the PRD explicitly leaves it with the wider epic rather than asking this
  module to anticipate it. The `Recipient` column is additive, so a link-level id can sit beside it later
  without a migration that breaks anything already written.
- **It does not resolve conflicting `AllowDownload` values across overlapping windows.** Because
  publishing is additive, the same item can carry several windows for the same party with different
  download flags. Visibility resolves this by union ("any window open wins"); nothing resolves the
  equivalent question for downloading. `AllowsDownloadTo` deliberately answers for one row only, so that
  whatever eventually enforces downloads does not silently inherit an answer nobody actually decided.
- **It does not support unpublish or revoke.** Raised as gap #3 in a design review (issue #60) and not
  selected for this pass — open, not ruled out.
- **It does not decide whether counsel's disclosure follows the arguido's automatically.** Art. 355.º/1
  CT names only the arguido as the holder of the inspection right and is silent on whether granting them
  a lawyer extends the same disclosure without a separate act. The model supports either pattern — publish
  twice, or publish case-wide — so no code is blocked on the answer, but the *default* a publish screen
  should apply is a legal question, tracked as issue #240 and put to the firm rather than guessed.

## What's still undecided, and where the trail runs cold

These are open questions recorded in `docs/content/prds/external-portal.md` §8, not gaps you are expected
to close by inference:

- **The download-conflict rule above.** §8 calls "narrowest grant wins" the obvious candidate but is
  explicit that it has not been chosen — do not implement it as if it had been.
- **Per-link targeting**, blocked on `PortalLink` (#227), which itself is not yet built.
- **Whether an OTP (one-time password) is required** alongside the portal token. The proof-of-concept
  carried an unused `otp_required` flag; two other documents mention "token + OTP" informally; nothing
  formally requires it.
- **How the portal link is delivered** — riding the charges notification (#59) or issued as a separate
  act — which decides whether token issuance is blocked by #59 or merely sequenced after it.
- **Whether inviting counsel requires an existing *procuração* (power of attorney)** already recorded on
  the case, or whether counsel can be invited ahead of it.
- **Where the single-use download grant (SYS-REQ-304) is stored.** No cache infrastructure exists on
  `dev`; a new table is the default answer but is flagged as worth deciding rather than defaulting into.
- **Who a portal consultation is audited as.** The audit middleware stamps a `System` actor for
  unauthenticated requests, which the PRD calls out as not a useful *registo de acesso* (access log) —
  the arguido is a real, identifiable party, and "System" loses that.
- **PII retention** for the invited person's name and email, which `PortalLink` will store for someone who
  must never become an account holder.

If you are picking up the next slice: the External Portal epic (E08) is the container for most of the
above, and the read path (#229) is the piece that depends most directly on everything this note describes
— it is the first place any of `PortalDisclosure`'s three predicates gets called by something other than
a test. `PortalDisclosure` itself has no open acceptance criteria against it as of PR #218; what remains
there is the recorded list above, not silent gaps.
