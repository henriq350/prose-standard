# Test 001 — base standard vs. nothing

**Task.** A design note explaining the portal-disclosure model to a backend engineer who has just
joined: what it stores, what it deliberately does not do, and what is still open.

**Why this task.** The feature was built this week, so no polished prose exists to copy. It is
conceptually layered (visibility window → recipient role → download grant → per-row vs cross-row), and
it carries real claim-status content: things decided, things deferred to #229, and one question left
open in PRD-012 §8. Introducing concepts in order and marking their status is exactly what the standard
claims to help with.

**Conditions.** Same model (Sonnet), same brief, same repo access. Treatment additionally reads and
follows `base.md`. Neither is told it is a comparison.

**Committed before reading the outputs — what I will check:**

1. **Forward references.** Count concepts used before they are introduced. This is the failure that
   prompted the whole exercise.
2. **Terminological drift.** For each of the core things (the aggregate, the recipient field, the
   download grant, the window), count how many distinct names each is given.
3. **Pronoun resolution.** Count pronouns whose referent is not the subject of the preceding sentence.
4. **Claim status.** Is *decided* distinguished from *deferred* from *open*? Is the cross-row question
   marked as undecided, or silently resolved?
5. **The reader test.** Can I answer, from the text alone: what is persisted, what is enforced, what is
   not enforced and by whom instead, and what must not be assumed settled?

**What would count against the standard:** the treatment document being longer without scoring better
on 1–5, or reading as a rule-following exercise rather than an explanation.

**Known limits.** One task, one model, n=1 per condition. This can show a large difference or nothing.
It cannot show a small one, and it is not evidence about human readers.
