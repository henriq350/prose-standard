# Test 002 — base standard vs. nothing, with the imitation channel closed

**Why a second test.** In test 001 both agents were told to read `BEHAVIOUR.PortalDisclosure.md` and
PR #218 — prose written under this standard. The control could therefore imitate the target register,
which is the likeliest explanation for the near-parity. That was not a control.

**The fix.** Both agents read **code only**, on a slice neither the standard's examples nor this
session's prose touch. Reading behaviour specs, design notes, PRDs and PR bodies is explicitly
forbidden. The treatment's only prose input is `base.md` itself.

**Slice.** Evidence — upload with SHA-256 custody, and the read side with authorised download that
records access. Comparable layering to test 001 (aggregate → command → query → streaming read), and it
carries real claim-status content: the hash detects tampering rather than preventing the serve, and
access is recorded at authorisation rather than at stream completion.

**Same as before:** same model, same task text, neither agent told it is a comparison.

**Committed before reading the outputs — same five checks as test 001:**

1. Concepts used before they are introduced.
2. Distinct names given to each core thing.
3. Sentences opening on a bare pronoun, as a share of all sentences. *(Test 001: control 14.1%,
   treatment 8.6% — the only measurable separation. This is the one to watch.)*
4. Is *decided* distinguished from *deferred* from *open*? Specifically: is "detects tampering, does not
   prevent the serve" stated as the deliberate choice it was, or glossed as a limitation?
5. Can I answer from the text alone: what is stored, what is verified and when, what is recorded and at
   which moment, and what the hash does **not** guarantee?

**What would count against the standard:** longer without scoring better, or reading as rule-following
rather than explanation.

**Still true:** n=1 per condition, one model, one task. Detects a large difference or none.
