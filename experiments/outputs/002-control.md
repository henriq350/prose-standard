# The evidence slice

Audience: a backend engineer joining the team who knows C#, .NET, and this repo's Clean
Architecture layering, but has never touched this feature and has no background in Portuguese
disciplinary procedure.

This document is built only from the source files listed at the end, plus the three test
files. Where those files don't answer a question, this document says so instead of guessing.

## 1. What the slice is, in one paragraph

A `Case` can be in a "Preliminary Inquiry" stage (the code and tests call this
`CaseStatus.PreliminaryInquiry` / "Inquérito Prévio"; the domain folder is literally named
`InqueritoPrevio`). During that stage, an `Diligencia` ("diligência" — an investigative act
recorded against the case; its own type isn't in the files read for this document, only its
`Id` and `CaseId`) can produce a file that needs to be kept as evidence. The evidence slice is
the write path that uploads such a file, hashes it, and stores it, plus a read path that lists
a case's evidence and streams one item's bytes back out. Every upload and every download is
recorded as an event, so the slice functions as a chain-of-custody log, not just file storage.

## 2. The domain model: `EvidenceItem`

`backend/MyLegalTeam.Domain/Cases/InqueritoPrevio/EvidenceItem.cs`

```csharp
public sealed class EvidenceItem : AggregateRoot
{
    public Guid CompanyId { get; private init; }
    public Guid CaseId { get; private init; }
    public Guid DiligenciaId { get; private init; }
    public string Sha256 { get; private init; } = string.Empty;
    public string FileName { get; private init; } = string.Empty;
    public string ContentType { get; private init; } = string.Empty;
}
```

Points worth internalizing:

- **Every property is `private init`.** There is no way to mutate an `EvidenceItem` after
  construction anywhere in this file. The only way to create one is the static factory
  `Upload(...)`.
- **There is no `Content`/bytes property.** This is deliberate — the aggregate is metadata
  only. `Upload(...)` takes a `byte[] content` parameter, hashes it, and returns; it is the
  *caller's* job (the command handler, see §5) to actually persist those bytes somewhere. The
  domain test says this outright: "No Content property to assert against, by design: the
  aggregate is metadata only."
- **`Sha256` is computed once, by the domain, from the actual bytes passed in** — not supplied
  by the caller: `Convert.ToHexString(SHA256.HashData(content)).ToLowerInvariant()`. It is
  lower-case hex. Nothing in the read files re-sets it after construction (the class doc
  comment calls this out explicitly: "hash set once, never re-set").
- **`DiligenciaId` is required and is treated as a chain link, not a loose reference.** The
  class doc comment says "Requires a diligência link ... no orphan uploads"; the handler (§5)
  enforces that the referenced diligência exists *and* belongs to the same case before it will
  build an `EvidenceItem`.
- **The class comment carries two tags**, `PROOF-evidence-001` (the diligência link) and
  `PROOF-evidence-002` (hash set once). These look like pointers into some external
  requirements/proof-tracking system. Nothing in the read files explains what that system is —
  treat them as traceability breadcrumbs, not as something this document can expand on.
- Raising the domain event happens inside `Upload`, via `evidence.RaiseDomainEvent(new
  EvidenceUploaded(...))` — same pattern as any other aggregate root in this codebase.

`EvidenceItemTests.cs` pins two behaviors: the hash is deterministic (equal bytes → equal
hash; a one-byte change flips it), and `Upload` populates `CaseId`/`DiligenciaId`/`FileName`
correctly while raising exactly one `EvidenceUploaded` event.

## 3. The three domain events

`backend/MyLegalTeam.Domain/Cases/Events/EvidenceEvents.cs`

| Event | Raised where | `AggregateId` | Carries |
|---|---|---|---|
| `EvidenceUploaded` | `EvidenceItem.Upload` | the **case** id | `EvidenceId`, `DiligenciaId`, `Sha256` |
| `EvidenceAccessed` | the download handler, on every successful download attempt | the **case** id | `EvidenceId`, `Sha256` |
| `EvidenceIntegrityMismatched` | the download handler, only if the served bytes don't hash to the recorded value | the **case** id | `EvidenceId`, `ExpectedSha256`, `ActualSha256` |

The detail most likely to surprise a newcomer: **all three events use the case's id as
`AggregateId`, not the evidence item's own id.** The class comment on `EvidenceUploaded`
spells this out: "Carries the case id as AggregateId so the ledger records custody on the case
stream, not a stream of its own." There is no independent event stream per `EvidenceItem`;
custody facts about evidence live inside the case's own event history. `EvidenceReadsTests.cs`
asserts this directly for both `EvidenceAccessed` and `EvidenceIntegrityMismatched`.

`EvidenceAccessed`'s doc comment adds a subtlety about *how* it gets persisted: "Raised from a
read, so it goes through `RaiseEvent` rather than `UnitOfWork.CommitAsync` — it persists
because `AuditLedger.AppendAsync` opens its own transaction." `AuditLedger` is named but its
source file is not among those read for this document, so exactly how/where these events end
up durable is out of scope here — treat that mechanism as a black box that this slice calls
into via `IMediatorHandler.RaiseEvent`.

`EvidenceIntegrityMismatched`'s doc comment: "Detection, not prevention — hashed while
streaming, so the bytes are already on the wire." This is the single most important guarantee
(or rather, non-guarantee) to understand about the download path — see §6.2.

## 4. Storage split: DB row vs. object storage

`backend/MyLegalTeam.Infrastructure.Persistence/Configurations/EvidenceItemConfiguration.cs`

The EF configuration confirms what the domain model implies: the row stores only
`CompanyId`, `CaseId`, `DiligenciaId`, `Sha256`, `FileName`, `ContentType` (all `IsRequired()`),
plus whatever `AggregateRoot` contributes (`Id`, and a `CreatedAt` used by the mapper and
repository ordering). A comment on the configuration says explicitly: "Bytes live in object
storage under `{CompanyId}/{Sha256}` ... not as a column here."

Indexes: `CompanyId`, `CaseId`, and `DiligenciaId` each get a `HasIndex`. There is **no unique
index or constraint on `Sha256`** in this file — nothing here prevents two different rows
(e.g. two uploads of byte-identical content, whether for the same or different diligências)
from sharing a hash. Because the object-storage key is derived from `{CompanyId, Sha256}`
(see below), such rows would point at the same stored object. Whether the system treats that
as fine, intentional dedup, or an unhandled case is not something the read files answer.

Optimistic concurrency uses a shadow `uint` property named `"Version"`, mapped with
`IsRowVersion()` onto Postgres's `xmin` system column — mirroring `Case`/`Deliverable`
elsewhere in the codebase, per the inline comment. The comment also warns the property must
**not** be named `xmin` directly.

The actual object-storage key is built by `ContentAddressedKey.For(companyId, sha256).Value`.
That helper's source is not among the files read for this document; treat it as a black box.
What the tests do prove is that **both the upload path and the download path call the exact
same builder** — `UploadEvidenceCommandTests` asserts the file store is written to at
`ContentAddressedKey.For(CompanyId, added.Sha256).Value`, and `EvidenceReadsTests` seeds the
mock file store at `ContentAddressedKey.For(evidence.CompanyId, evidence.Sha256).Value`. A test
comment underlines why this matters: "Asserted against production's own key builder, so this
breaks if the tenant-prefixed key convention ever changes" — i.e., don't hand-roll the key
anywhere; always go through `ContentAddressedKey`.

## 5. The write side: uploading evidence

Route: `POST /companies/{companyId}/cases/{caseId}/inquiry-actions/{diligenciaId}/evidence`
(`EvidenceController`, `[Authorize]`). Note the diligência is part of the URL — evidence is
addressed by which diligência produced it.

Flow, in the order the handler (`UploadEvidenceCommandHandler.Handle`) actually runs it:

1. **Validate the command** (`UploadEvidenceValidator`, FluentValidation): every field is
   required — `CallerAccountId`, `CompanyId`, `CaseId`, `DiligenciaId` (non-empty GUIDs),
   `FileContentBase64`, `FileName`, `ContentType` (non-empty strings). A blank body fails here,
   as a 400 on field `fileContentBase64`, before anything tries to decode it — proven by
   `Evidence_with_a_blank_body_is_a_missing_field_not_an_empty_upload`.
2. Set tenant context to `CompanyId`.
3. Load the caller `Account` and `Membership`, resolve `CallerAuthority.Resolve(caller,
   membership)`. (`CallerAuthority` is not among the read files — treated as a black box that
   produces something `CaseCallerRole.RolesOf` consumes.)
4. Load the `Case`. Missing → **404** `CasesErrors.CaseNotFound`.
5. Look up whether the caller is on the case team.
6. Resolve `CaseCallerRole.RolesOf(@case, callerAccountId, authority, callerIsOnCaseTeam)` into
   a set of roles. **Empty set → 404** `CasesErrors.CaseNotFound` — not 403. The reasoning,
   stated in the test comments: "A 403 would confirm the case's existence to a caller who
   can't see it."
7. **If the resolved roles don't include `CaseRole.Instructor` → 403**
   `CasesErrors.NotTheAssignedInstructor`. So there are two distinct refusal codes by design:
   total outsiders get 404, case participants who aren't the assigned instructor get 403. The
   test file even flags this on purpose: "Two distinct refusal codes on purpose — asserted
   explicitly so a future collapse would fail here."
8. Load the `Diligencia` by id. **Missing, or `diligencia.CaseId != request.CaseId` → 404**
   `InquiryErrors.DiligenciaNotFound`. This is the "no orphan uploads" rule (comment tags it
   `SYS-REQ-402`). Note this check runs *after* the instructor check, so error precedence for a
   non-instructor pointing at a bad diligência is 403, not 404-for-the-diligência.
9. **Size bound, checked before decoding:** `UploadLimits.ExceedsMax(FileContentBase64,
   maxUploadBytes)` — evaluated against the *base64-encoded* string length, specifically so an
   oversized payload never gets decoded into a byte array first. Exceeded → **422**
   `InquiryErrors.EvidenceTooLarge`. `UploadLimits` itself isn't in the read files; the limit
   value comes from `FileStorageSettings.MaxUploadBytes`, injected via configuration (a test
   comment notes "Limit is configuration now; tests state the value directly rather than a
   removed constant").
10. **Decode:** `Convert.FromBase64String`. A `FormatException`, or a decoded array of length
    0, both count as invalid → **422** `InquiryErrors.InvalidEvidenceContent`.
11. `EvidenceItem.Upload(...)` — computes the hash and raises `EvidenceUploaded` (in memory,
    not yet dispatched).
12. **Object storage is written before the database commit**, deliberately:
    ```csharp
    var key = ContentAddressedKey.For(evidence.CompanyId, evidence.Sha256).Value;
    await _fileStore.PutAsync(key, bytes, evidence.ContentType, cancellationToken);
    await _evidenceRepository.AddAsync(evidence, cancellationToken);
    if (!await Commit(cancellationToken)) { ... }
    ```
    The comment: "Written before the row commits: a crash between the two orphans the object
    rather than leaving a row pointing at nothing." Two tests pin the two failure modes this
    buys:
    - If `Commit` fails after a successful `PutAsync`, the object is still in storage
      (orphaned) and the handler returns **500** `CasesErrors.ErrorSaving` — it does **not**
      try to undo the storage write.
    - If `PutAsync` itself throws, the exception **propagates out of the handler** (it is not
      turned into a `Result`), and `AddAsync` is never called — nothing lands in the
      repository. A storage failure is a hard failure, not a modelled error case.
13. On success: `UploadEvidenceResult(evidence.Id, evidence.Sha256)`.

Everything downstream of a failed check calls `VerifyNothingStored()` in the tests — neither
`AddAsync` nor `PutAsync` is invoked for any rejected upload except where noted above.

## 6. The read side

Route base: `GET /companies/{companyId}/cases/{caseId}/evidence` (`EvidenceReadsController`)
— **case-scoped**, unlike the diligência-scoped upload route.

### 6.1 Listing (`GetEvidenceQuery` / `GetEvidenceQueryHandler`)

- Visibility check is `CaseVisibility.ResolveVisibleCaseAsync(...)` — a different, broader
  check than "is the assigned instructor." The class doc comment says only: "Visibility: the
  caller may view the case." `CaseVisibility` is not among the read files, so the exact rule
  (who counts as able to view a case) is out of scope here. Not visible → **404**
  `CasesErrors.CaseNotFound`.
- Returns `evidenceRepository.GetByCaseAsync(caseId)`, which the repository orders newest
  first (`OrderByDescending(e => e.CreatedAt)`), then maps each item through
  `EvidenceMapper.ToListItemDto` into an `EvidenceListItemDto(Id, DiligenciaId, Sha256,
  FileName, ContentType, CreatedAt)`.
- **Listing never touches object storage.** `EvidenceReadsTests.Listing_never_reads_the_stored_bytes`
  asserts `fileStore.OpenReadAsync` is called zero times. The repository comment explains why
  this is structural, not incidental: "the bytes are in object storage since #191, so the row
  is already metadata-only."
- Note: the list DTO exposes the raw, **caller-supplied, never-validated** `ContentType`
  string as-is. Contrast with download, next.

### 6.2 Downloading (`DownloadEvidenceQuery` / `DownloadEvidenceQueryHandler`)

The file-level comment states the design intent up front: "Proxied rather than a signed URL on
purpose: the application has to stay between caller and bytes to record the access." There is
no direct/pre-signed link to object storage anywhere in this slice — every download goes
through the API.

Handler flow:

1. Same `CaseVisibility.ResolveVisibleCaseAsync` check as listing → 404 if not visible.
2. Load the evidence row by id; **missing, or belonging to a different case → 404**
   `EvidenceReadErrors.EvidenceNotFound`.
3. Build the storage key with `ContentAddressedKey.For(evidence.CompanyId, evidence.Sha256)`
   and open it via `fileStore.OpenReadAsync`. **If the store returns `null`, the handler
   throws `InvalidOperationException`** — this is *not* modelled as a `Result` error. The
   reasoning: "The upload writes the object before committing the row, so this is a fault, not
   a modelled 404." In other words, a missing object at this point means something is broken
   in infrastructure/data integrity, not a normal user-facing "not found." Test:
   `A_row_whose_object_is_missing_fails_loudly`.
4. **`EvidenceAccessed` is raised immediately here** — before the caller has read any bytes,
   and specifically *not* deferred until the stream is drained. Comment: "a caller who reads
   part of the file and disconnects has still seen evidence, and an access that was never
   recorded cannot be recovered later." Test `An_abandoned_download_still_records_the_access`
   proves the event is present even when the returned stream is never drained by the caller of
   the handler.
5. The stored stream is wrapped in `HashVerifyingStream`, which hashes bytes as they are read
   (so it never buffers the whole file) and, only once the stream is drained to end-of-stream
   **via the async read path**, compares the computed digest to the recorded `Sha256`:
   - Match: nothing further happens.
   - Mismatch: raises `EvidenceIntegrityMismatched(caseId, evidenceId, expected=evidence.Sha256,
     actual=computedDigest)`, deliberately with `CancellationToken.None` — "the caller has
     drained the response, so a disconnect at EOF must not lose the entry."
   - If *raising that event* itself throws, the exception is caught and only logged, never
     propagated — "the 200 and most of the body are already on the wire — propagating would
     truncate the download without preventing the access it failed to record."
   - **`HashVerifyingStream.Read` (the synchronous overload) does not fire the completion
     callback at all** — only `ReadAsync` does. Its own doc comment says so explicitly. A
     caller that drains this stream synchronously will never trigger the hash-verification
     step or its resulting event.
6. Returns `FileDownload(stream, SafeContentType, evidence.FileName)`, where
   `SafeContentType` is the **hardcoded constant** `"application/octet-stream"` —
   *regardless* of what `ContentType` is stored on the row. Comment: "Served regardless of the
   stored type: that value is caller-supplied and never validated, so echoing it back is the
   stored-XSS path." Test `A_hostile_stored_content_type_is_not_echoed_back` uploads with
   `ContentType = "text/html"` and confirms the download response's content type is the safe
   constant, not `"text/html"`.

**The central non-obvious guarantee here:** the integrity check is *detection*, not
*prevention*. If the stored object has been swapped/corrupted, the download still succeeds and
serves the (wrong) bytes to the caller — the mismatch is only discovered and recorded as an
event once the caller (or the framework, on their behalf) has read all the way to end of
stream via the async path. Test `A_swapped_object_is_detected_and_recorded_as_a_tamper_fact`
shows the caller still gets the swapped bytes; the corruption is only visible afterward, as an
`EvidenceIntegrityMismatched` fact on the case's event stream.

## 7. Application layer glue

- `EvidenceService` (`Application/Services/EvidenceService.cs`) is the thin `IEvidenceService`
  implementation both controllers depend on. It just forwards to the mediator
  (`bus.SendCommand(...)`) for `Upload`, `List`, and `Download`, and for `List` additionally
  maps the `Result<List<EvidenceItem>>` into `Result<EvidenceListResult>` via
  `EvidenceMapper.ToEvidenceListResult`.
- `EvidenceMapper` (`Application/Mappings/EvidenceMapper.cs`) is the only place `EvidenceItem`
  is turned into wire DTOs (`EvidenceListItemDto`, `EvidenceListResult`). Its own doc comment
  notes it's split out from `CaseMapper`, "like `DeliverableMapper`", because evidence is its
  own aggregate.
- The exact shapes of `UploadEvidenceRequest`, `EvidenceListItemDto`, `EvidenceListResult`, and
  `FileDownload` live under `ViewModels.Cases` / storage namespaces, which weren't in the read
  set beyond their field names as used here. From usage: `UploadEvidenceRequest` has
  `FileContentBase64`, `FileName`, `ContentType`; `FileDownload` has `Stream`, `ContentType`,
  `FileName` (constructor-positional, from `new FileDownload(stream, SafeContentType,
  evidence.FileName)` and test access to `.Stream` / `.ContentType`).

## 8. API surface

| Route | Method | Handler | Notes |
|---|---|---|---|
| `POST /companies/{companyId}/cases/{caseId}/inquiry-actions/{diligenciaId}/evidence` | `EvidenceController.Upload` | `UploadEvidenceCommand` | 201 on success; 400/403/404/422 documented on the action via `ProducesResponseType` |
| `GET /companies/{companyId}/cases/{caseId}/evidence` | `EvidenceReadsController.List` | `GetEvidenceQuery` | metadata only |
| `GET /companies/{companyId}/cases/{caseId}/evidence/{evidenceId}/content` | `EvidenceReadsController.Download` | `DownloadEvidenceQuery` | streams bytes, records access |

Both controllers are `[Authorize]`; `CallerAccountId` is read from the base `ApiController`
(not in the read set — presumably derived from the authenticated principal, but that's outside
this document's source files).

## 9. What this slice guarantees

Backed directly by code and/or the three test files:

- The recorded `Sha256` is always the hash of the bytes the domain actually received at
  upload time, computed server-side, and is never changed afterward.
- An `EvidenceItem` cannot exist without a `DiligenciaId` pointing at a diligência that exists
  and belongs to the same case (checked at upload time).
- Only the account holding `CaseRole.Instructor` for the case may upload evidence for it; other
  case participants get 403, non-participants get 404.
- On upload, object-storage bytes are written before the DB row commits, so the only possible
  inconsistency after a failure is an **orphaned object with no row** — never a row with no
  backing object (assuming `PutAsync` itself didn't fail, in which case nothing is persisted
  at all).
- Listing evidence for a case never reads or transfers file bytes.
- Every download is proxied through the API; there is no direct/signed link to the object
  store. Reaching the point where the stored object is opened always raises
  `EvidenceAccessed`, regardless of whether the caller actually reads any bytes afterward.
- A mismatch between the recorded hash and the bytes actually served is detected and recorded
  as `EvidenceIntegrityMismatched` — but only when the response stream is drained through the
  async read path to end of stream.
- The HTTP content type on a download response is always a fixed, safe value
  (`application/octet-stream`), never the caller-supplied value stored at upload time.
- Upload/access/mismatch events are all recorded on the **case's** event stream (`AggregateId`
  = case id), not on a stream of their own.

## 10. What this slice does not guarantee (or leaves to other code)

- **It does not prevent tampered or corrupted content from being served.** The hash check is
  after-the-fact detection; the caller receives the bytes regardless.
- **Draining the download stream synchronously (`Read`, not `ReadAsync`) skips hash
  verification entirely** — the completion callback, and therefore
  `EvidenceIntegrityMismatched`, never fires on that path.
- **The stored `ContentType` is never validated at upload** (the validator only checks
  non-empty) and is returned verbatim by the *list* endpoint, even though the *download*
  endpoint deliberately ignores it in favor of a safe constant. A consumer of the list endpoint
  should not treat `ContentType` as trustworthy.
- **`Sha256` has no uniqueness constraint** in the EF configuration read here. Nothing in the
  read files says whether duplicate content (same hash) across rows is expected, deduplicated,
  or simply allowed to coexist pointing at the same storage key.
- **Case visibility rules for List/Download** (`CaseVisibility.ResolveVisibleCaseAsync`) and
  **instructor/authority resolution for Upload** (`CallerAuthority.Resolve`,
  `CaseCallerRole.RolesOf`) are implemented elsewhere and were not part of the read set. This
  document only describes their observed effect at the evidence slice's boundary (which error
  and status code result), not their internal rules.
- **`IFileStore`, `ContentAddressedKey`, and `UploadLimits`** are all referenced but not
  defined in the read files. This document treats them as black boxes with the behavior
  implied by their call sites and by the test assertions against them.
- **Where `EvidenceUploaded`/`EvidenceAccessed`/`EvidenceIntegrityMismatched` end up, and how
  `AuditLedger` (named in one doc comment) persists or exposes them**, is not covered by the
  read files.
- **No update or delete operation exists anywhere in the read files.** Nothing observed here
  contradicts "evidence, once uploaded, is immutable and permanent," but this document can only
  say that no such operation was found in the read set — not that one is impossible elsewhere
  in the codebase.
- Several identifiers in the code comments (`PROOF-evidence-001`, `PROOF-evidence-002`,
  `SYS-REQ-402`, `SYS-REQ-404`, `#51`, `#191`, `#205`, and the `BHV-evidence-*` /
  `BHV-evidenceread-*` trait tags on tests) look like references into an external
  requirements/behavior-tracking system. This document does not resolve what that system says;
  it only reports the code's own comments and test assertions.

## 11. Two vocabulary notes

- **"Inquérito Prévio" / "Preliminary Inquiry"** — the case status this evidence type belongs
  to. Known only as a stage name (`CaseStatus.PreliminaryInquiry`, used in the tests) plus the
  domain folder name `InqueritoPrevio`. Nothing about the underlying disciplinary procedure is
  covered here.
- **"Diligência"** — the producing action an evidence item links to via `DiligenciaId`. The
  `Diligencia` type itself, and its subtypes (the tests use a `DocAnalysis.Record(...)`
  factory, implying at least one concrete kind exists), are outside the read set. Treat it here
  only as "an id that must exist and must belong to the same case."

## 12. Files this document is built from

- `backend/MyLegalTeam.Domain/Cases/InqueritoPrevio/EvidenceItem.cs`
- `backend/MyLegalTeam.Domain/Cases/Events/EvidenceEvents.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/UploadEvidence/UploadEvidenceCommand.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/UploadEvidence/UploadEvidenceValidator.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/Reads/GetEvidenceQuery.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/Reads/DownloadEvidenceQuery.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/Reads/HashVerifyingStream.cs`
- `backend/MyLegalTeam.Application/Features/Cases/Inquiry/Reads/EvidenceReadErrors.cs`
- `backend/MyLegalTeam.Application/Services/EvidenceService.cs`
- `backend/MyLegalTeam.Application/Mappings/EvidenceMapper.cs`
- `backend/MyLegalTeam.Api/Controllers/EvidenceController.cs`
- `backend/MyLegalTeam.Api/Controllers/EvidenceReadsController.cs`
- `backend/MyLegalTeam.Infrastructure.Persistence/Configurations/EvidenceItemConfiguration.cs`
- `backend/MyLegalTeam.Infrastructure.Persistence/Repositories/EvidenceRepository.cs`
- `backend/MyLegalTeam.Tests/Domain/EvidenceItemTests.cs`
- `backend/MyLegalTeam.Tests/Commands/Cases/UploadEvidenceCommandTests.cs`
- `backend/MyLegalTeam.Tests/Commands/Cases/EvidenceReadsTests.cs`
