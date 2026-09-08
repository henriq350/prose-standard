# The evidence slice

## Scope

This document covers one vertical slice of the case-management domain: the code that lets an
investigator attach a file to a case as evidence, and lets someone with access to the case list and
download it again. It was written from the following files only, and every claim below traces to one
of them:

- `Domain/Cases/InqueritoPrevio/EvidenceItem.cs` — the aggregate
- `Domain/Cases/Events/EvidenceEvents.cs` — the three domain events
- `Application/Features/Cases/Inquiry/UploadEvidence/` — the write side (command + validator)
- `Application/Features/Cases/Inquiry/Reads/` — the read side (two queries, the hash-checking stream,
  the read-only error enum)
- `Application/Services/EvidenceService.cs`, `Application/Mappings/EvidenceMapper.cs`
- `Api/Controllers/EvidenceController.cs`, `EvidenceReadsController.cs`
- `Infrastructure.Persistence/Configurations/EvidenceItemConfiguration.cs`,
  `Infrastructure.Persistence/Repositories/EvidenceRepository.cs`
- the three test files covering the aggregate, the upload command, and the two read queries

Several types the slice depends on — `CaseCallerRole`, `CaseVisibility`, `ContentAddressedKey`,
`UploadLimits`, the `AggregateRoot` and `Event` base classes, the `Result<T>`/`ICommandHandler` CQRS
plumbing — are used by these files but are not themselves among them. Where this document describes
their behaviour, it is describing what the evidence-slice files show about how they are *called*, not
something read from their own source. That distinction is marked each time it matters.

## Vocabulary

The domain sits inside a Portuguese disciplinary procedure. Two terms recur and are not English:

- **Inquérito Prévio** — the case stage this all happens in. Read as "preliminary inquiry." The
  namespace `Domain.Cases.InqueritoPrevio` is named for it.
- **diligência** — a single investigative step or action taken within that inquiry (an interview, a
  document request, and so on). The code treats a diligência as its own entity with its own `Id`; this
  document does not cover how one is created, only that evidence must point at an existing one.

Both are kept untranslated from here on, the way the code keeps them: `DiligenciaId`,
`IDiligenciaRepository`, `DiligenciaNotFound`.

## What the aggregate stores

`EvidenceItem` (`Domain/Cases/InqueritoPrevio/EvidenceItem.cs`) is metadata only. Its fields:

| Field | Meaning |
|---|---|
| `CompanyId` | tenant |
| `CaseId` | the case this evidence belongs to |
| `DiligenciaId` | the diligência that produced it — see "The chain link" below |
| `Sha256` | lower-case hex SHA-256 of the uploaded bytes |
| `FileName` | as supplied by the uploader |
| `ContentType` | as supplied by the uploader |

There is no `Content` or byte-array property. The XML doc on the factory method is explicit about why:
`Upload` "hashes the content; does not store the bytes — the caller writes them to object storage under
the hash." The unit test for this method states the same guarantee directly: "No `Content` property to
assert against, by design: the aggregate is metadata only." The bytes live in object storage, addressed
by `{CompanyId}/{Sha256}` (built by `ContentAddressedKey.For`, a type used here but not read for this
document). The database row and the object it points at are two separate writes, described below.

All settable properties use `private init`, and the only way to construct an instance outside EF is the
static `Upload` factory. Nothing in this file set exposes a way to change `FileName`, `ContentType`,
`DiligenciaId`, or `Sha256` after construction — an `EvidenceItem`, once created, does not change.

## Uploading: how evidence enters custody

The route is `POST /companies/{companyId}/cases/{caseId}/inquiry-actions/{diligenciaId}/evidence`
(`EvidenceController.Upload`), which forwards to `UploadEvidenceCommand`. The handler runs its checks in
a fixed order, and the order is worth knowing because it determines which error a given bad request
gets:

1. **Field validation** (`UploadEvidenceValidator`) — every field is required: caller, company, case,
   diligência, file content, file name, content type. A blank `FileContentBase64` fails here, as a
   missing-field 400, before the handler's own decoding logic ever runs — confirmed by
   `Evidence_with_a_blank_body_is_a_missing_field_not_an_empty_upload`.
2. **Case exists** — otherwise 404 (`CasesErrors.CaseNotFound`).
3. **Caller has standing on the case** — role resolution happens through `CaseCallerRole.RolesOf`, a
   helper outside this file set. If it returns no roles at all, the response is 404, not 403. A
   test comment calls this out directly: a 403 "would confirm the case's existence to a caller who
   can't see it." If it returns roles but none is `Instructor` — for example a caller who is
   a `CaseManager`-role member of the case team — the response is 403
   (`CasesErrors.NotTheAssignedInstructor`). **Only the assigned instructor may upload.** This is a
   narrower check than the read side (below), and the two are asserted as staying distinct: "Two
   distinct refusal codes on purpose — asserted explicitly so a future collapse would fail here."
4. **The diligência exists and belongs to this case** — otherwise 404
   (`InquiryErrors.DiligenciaNotFound`). One error code covers two different failures — no such
   diligência, and a diligência that belongs to a different case — and the tests confirm both map to it
   rather than being distinguished. The code comment names this "no orphan uploads (SYS-REQ-402)."
5. **Encoded size bound** — the base64 string's length is checked against a configured maximum
   (`FileStorageSettings.MaxUploadBytes`, via a helper called `UploadLimits.ExceedsMax`) *before*
   decoding, so an oversized payload is rejected (422, `EvidenceTooLarge`) without ever allocating the
   decoded byte array. The exact arithmetic inside `UploadLimits` is not in this file set; what the test
   `An_oversized_evidence_file_is_refused_before_it_is_decoded` confirms is that the bound is judged on
   the *encoded* string, and that its threshold tracks the roughly 4/3 base64 expansion factor.
6. **Decode** — `Convert.FromBase64String`. A string that is not valid base64, or that decodes to zero
   bytes, is 422 (`InvalidEvidenceContent`) — a `Result`, not a thrown `FormatException`.

Only after all of that does the handler call `EvidenceItem.Upload`, which computes the hash, then two
writes happen in this order:

1. `IFileStore.PutAsync` — the bytes go to object storage under the content-addressed key, first.
2. `IEvidenceRepository.AddAsync` followed by `Commit` — the row is written second.

The handler comment states the reasoning directly: "a crash between the two orphans the object rather
than leaving a row pointing at nothing." A test exercises exactly this: a store write that succeeds
followed by a commit that fails still leaves the object written (`PutAsync` verified once) and returns
500 (`ErrorSaving`) — the object is an orphan, not the row. A store write that throws is not caught by
the handler; it propagates, and the test for that case asserts the repository's `AddAsync` was never
called. **This document did not find any cleanup path for an orphaned object** — nothing in the given
files reclaims or garbage-collects one; whether that exists elsewhere is not answerable from this file
set.

One asymmetry worth flagging against the read side: upload decodes the entire body into an in-memory
`byte[]` before writing it (after the size check above passes). The download path, covered next,
streams. Upload does not.

## Identity: the hash

`Sha256` is computed once, inside `EvidenceItem.Upload`, from `SHA256.HashData(content)`, and is never
reassigned — the aggregate's own doc comment names this "hash set once, never re-set
(`PROOF-evidence-002`)." `PROOF-evidence-002` is a bare identifier in the code; this document did not
read whatever artifact it refers to, and cannot say more about it than the guarantee the code itself
enforces: no setter exists, and the constructor path runs once.

Hashing is deterministic and content-sensitive: `The_hash_is_deterministic_over_content` asserts equal
bytes produce equal hashes, and that appending a single byte changes the hash. The domain comment adds
that the hash is also the tail of the storage key — `{CompanyId}/{Sha256}` — so it does double duty as
both an identity check and an address.

## The chain link: diligência

`DiligenciaId` is required at every layer that could otherwise let it be skipped: the validator
(`NotEmpty`), the handler (existence + same-case check, above), and the EF configuration, which both
requires the column and indexes it. Nothing in this file set describes what happens to an
`EvidenceItem` if its diligência is later deleted or edited elsewhere — that is outside the files read
for this document.

## Reading: listing and downloading

Both read paths sit behind case *visibility*, resolved by `CaseVisibility.ResolveVisibleCaseAsync` (a
helper outside this file set). This check is broader than the upload path's instructor-only check — the
read tests arrange the caller as a plain `CaseManager`-role team member, not an instructor, and that is
sufficient. A caller with no standing at all — no membership, not on the case team — gets 404, the same
"don't confirm existence" reasoning as upload's outsider case.

**Listing** (`GetEvidenceQuery`, `GET /companies/{companyId}/cases/{caseId}/evidence`) returns metadata
only: id, diligência id, hash, file name, content type, and `CreatedAt` (a property this document
observed being used — for sorting in the repository and returned in the list DTO — but did not find
defined; it is presumably inherited from the shared `AggregateRoot` base, which is outside this file
set). The list handler never opens object storage; a test pins this down explicitly as a guard against
regression, calling it "structural since #191" — the DB row has held no bytes since that change — "but
pinned: a future 'convenience' that folded content into the list would reintroduce exactly the memory
profile that move removed."

**Downloading** (`DownloadEvidenceQuery`,
`GET /companies/{companyId}/cases/{caseId}/evidence/{evidenceId}/content`) is proxied through the
application rather than served as a signed URL to the object store directly. The query's own doc comment
gives the reason: "the application has to stay between caller and bytes to record the access." What it
returns is a `FileDownload` — a stream, a content type, a file name — not JSON.

Two details of the download path matter enough to call out on their own:

- **The stored content type is never served back.** `DownloadEvidenceQueryHandler.SafeContentType` is a
  constant, `"application/octet-stream"`, used regardless of what was recorded at upload. The reason
  given in the code: `ContentType` "is caller-supplied and never validated, so echoing it back is the
  stored-XSS path." A test uploads with `contentType: "text/html"` and asserts the response is still
  `application/octet-stream`. Note that this only protects the *download* response header — the
  `ContentType` field is still returned as plain data in the list DTO (`EvidenceListItemDto`); whether
  that is safe depends on how a caller of the list endpoint uses that field, and this file set does not
  cover that caller.
- **A missing object is a thrown exception, not a modelled 404.** If `IFileStore.OpenReadAsync` returns
  null for a row that exists, the handler throws `InvalidOperationException`. The comment explains why
  this is not treated as an ordinary not-found: "the upload writes the object before committing the row,
  so this is a fault, not a modelled 404." In other words, the write ordering above is also the
  justification for treating this as a bug/incident rather than an expected outcome the caller should
  handle.

## Integrity check: detection, not prevention

`HashVerifyingStream` wraps the object-store stream and recomputes SHA-256 while the bytes pass through
it, comparing the result to the recorded `Sha256` once the stream is fully drained. The class comment
names the trade-off: this happens "without buffering the whole file — detection, not prevention." The
mismatch, if any, is not caught before the caller receives the bytes; the corrupted or swapped bytes are
already on the wire by the time it is known.

Two behaviours of this class are edge cases an engineer changing it should know:

- **The completion callback (`onDrained`) only fires on the async read path.** The class's own remark
  says so directly, and gives the reason: the callback does I/O (raising a domain event through the
  bus), and the synchronous `Read(byte[], int, int)` "would have to block a thread on it." The practical
  consequence: a consumer that reads this stream synchronously will never trigger the integrity check or
  the event it raises. Nothing in this file set describes which callers use the sync path versus the
  async one; this document can only report the asymmetry, not its consequences downstream.
- **Reading only part of the file does not trigger the check.** The hash is only finalised at end of
  stream. A caller who disconnects mid-download leaves no completed check and raises no
  `EvidenceIntegrityMismatched` for that read — confirmed by
  `An_abandoned_download_still_records_the_access`, which shows the *access* event still fires (see
  next section) while the file is never drained.

When a mismatch is detected, the handler raises `EvidenceIntegrityMismatched` using
`CancellationToken.None`, deliberately not the request's own token — the comment explains that by the
time end-of-stream is reached, "the caller has drained the response, so a disconnect at EOF must not
lose the entry." If raising that event itself throws, the handler catches the exception and logs it
rather than letting it propagate, because "the 200 and most of the body are already on the wire —
propagating would truncate the download without preventing the access it failed to record." One
consequence follows directly from this: a failure inside the event-raising path can leave a real
integrity mismatch **unrecorded**, silently. That is a stated trade-off in the code, not a gap this
document is pointing out unprompted.

## The custody trail: three events

All three events (`Domain/Cases/Events/EvidenceEvents.cs`) set `AggregateId` to the **case's** id, not
the evidence item's own id. The file comment on `EvidenceUploaded` states the reason: "so the ledger
records custody on the case stream, not a stream of its own." The same pattern holds for the other two.

- **`EvidenceUploaded`** — raised inside `EvidenceItem.Upload`, carries `DiligenciaId` and `Sha256`.
  Domain events raised this way go through the aggregate's own event list and are presumably dispatched
  as part of the normal unit-of-work commit; this file set does not show that dispatch mechanism itself.
- **`EvidenceAccessed`** — raised by the download handler as soon as the object stream is opened, not
  when it finishes draining. The comment is explicit: "a caller who reads part of the file and
  disconnects has still seen evidence, and an access that was never recorded cannot be recovered later."
  It is raised through `bus.RaiseEvent` rather than the command's `UnitOfWork.CommitAsync` — the event
  class comment says this is "so it goes through `RaiseEvent` rather than `UnitOfWork.CommitAsync` — it
  persists because `AuditLedger.AppendAsync` opens its own transaction." (`AuditLedger` is not in this
  file set; this document is reporting the comment's claim, not something it verified.)
- **`EvidenceIntegrityMismatched`** — raised only from inside the streaming hash check, described above,
  and only along the async path. Carries both the expected and actual hash.

Downloading is, in the vocabulary of the code's own comment, "a READ that writes" — a query that has a
side effect (the custody event) that an engineer used to CQRS naming conventions might not expect from a
query.

## Persistence

`EvidenceItemConfiguration` requires and indexes `CompanyId`, `CaseId`, and `DiligenciaId` individually
— three separate indexes, not a composite one, as read from the file. `Sha256`, `FileName`, and
`ContentType` are required but not indexed. There is no bytes column; the configuration's own comment
says so and points to the object-storage key convention. A comment also states that a fail-closed
tenant-scoping query filter is added elsewhere, in `ApplicationDbContext`, mirroring `Case` and
`Deliverable` — that file is outside this document's set, so this is the configuration comment's claim,
not something checked directly here.

Optimistic concurrency uses a shadow property named `Version`, mapped as a Postgres `xmin`-backed row
version (`IsRowVersion()`), with a comment warning it must not literally be named `xmin`. An engineer
adding a migration or a new configuration nearby should keep that naming constraint in mind; the file
does not explain the underlying reason beyond "must NOT be named 'xmin'," so treat it as a stated
constraint rather than an explained one.

`EvidenceRepository.GetByCaseAsync` returns newest-first (`OrderByDescending(CreatedAt)`), mirroring
`DeliverableRepository`'s method of the same name, per its own comment.

## Boundaries — what this document did not find

Stated plainly, because the standard this document follows asks for that rather than silence:

- **No update or delete path.** Nothing in the read file set adds, changes, or removes an `EvidenceItem`
  once uploaded, beyond the one-shot `Upload` factory. This document cannot say whether such a path
  exists elsewhere in the codebase or was never built; it was simply not among the files read.
- **No cleanup for an orphaned object.** Covered above — a failed commit after a successful object write
  leaves the object behind, and no reclaiming mechanism is visible in this file set.
- **The sync read path of `HashVerifyingStream` never checks integrity or records the completion event.**
  Stated in the code as a deliberate consequence of not wanting to block a thread on I/O; still worth
  flagging as a real behavioural difference between the two read APIs on the same class.
- **A failure while raising `EvidenceIntegrityMismatched` is swallowed (logged, not thrown).** A real
  tamper can go unrecorded if that particular write fails at exactly that moment.
- **The identifiers `#51`, `#205`, `#191`, `SYS-REQ-402`, `SYS-REQ-404`, `PROOF-evidence-001`, and
  `PROOF-evidence-002`** appear throughout these files as traceability tags to tickets or requirement
  documents. This document did not read whatever they point to (the task that produced it explicitly
  excluded `docs/`, `BEHAVIOUR.*.md`, PRDs, and PR descriptions) and reports them only as labels the code
  itself uses, not as verified requirements.
- **The exact bound math inside `UploadLimits.ExceedsMax`, and the role-resolution logic inside
  `CaseCallerRole.RolesOf` and `CaseVisibility.ResolveVisibleCaseAsync`,** are outside the file set. This
  document describes their observed *effects* — which requests they let through in the tests — not their
  implementation.

## Where the tests live, and how to read them as spec

The three test files given for this slice double as a fairly complete behavioural spec, each fact
tagged with a `[Trait("bhv", "...")]` identifier (`BHV-evidence-...` for the domain and upload side,
`BHV-evidenceread-...` for listing and downloading). Where this document states a guarantee — a status
code, an ordering, an event field — it is traceable to one of those tests by name. An engineer changing
this slice's behaviour should expect to touch the matching trait, not just the code near it.
