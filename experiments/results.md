# Test results

## Test 001 — portal disclosure (imitation channel OPEN)

| | Control | With standard |
|---|---|---|
| Words | 1 982 | 2 020 |
| Sentences opening on a bare pronoun | 12/83 — **14.5%** | 7/80 — **8.8%** |
| Terminological drift | none | none |
| Forward references | none found | none found |
| Open cross-row question resolved silently? | no | no |

Invalid as a test. Both agents were told to read `BEHAVIOUR.PortalDisclosure.md` and PR #218 — prose
written under this standard hours earlier — so the control could imitate the target register from its
sources.

## Test 002 — evidence slice (code only, no prose inputs)

| | Control | With standard |
|---|---|---|
| Words | 3 350 | **2 907** |
| Sentences | 133 | 92 |
| Sentences opening on a bare pronoun | 9/133 — **6.8%** | 8/92 — **8.7%** |
| "Detection, not prevention" framed as deliberate? | yes | yes |
| Honest about what the sources could not answer? | yes | yes |

## What the two tests together show

**The one measurable effect from 001 did not replicate, and reversed.** Bare-pronoun openings were the
only separation in 001, and it was the rule with a formal source behind it. On a clean slice the control
scored *better* (6.8% vs 8.7%). The 001 gap is best explained by the control imitating prose that had
been written under the standard — the confound, not the intervention.

**No criterion separated the two documents in 002.** Both framed the detection-not-prevention trade-off
as the deliberate choice it is, and both were explicit about which questions their sources could not
answer. Neither invented a rationale it could not see.

**The only stable difference is length.** The treatment was 13% shorter on the same task with the same
material, and used 92 sentences against 133.

**One number worth noting, on two data points, so barely a claim:** the treatment's pronoun rate was
almost identical across two very different tasks (8.8%, 8.7%), while the control's swung from 14.5% to
6.8%. Consistency is what a standard would be expected to produce. Two points is not evidence of it.

## Where these tests are still wrong

- **I contaminated criterion 4 in test 002.** Both agents were told "if something is not answerable from
  the sources, say so in the document rather than guessing." That is itself an instruction to mark claim
  status, given to the control as well. The criterion could not separate them because I had already
  supplied the behaviour to both.
- **n=1 per condition, one model, one author of both briefs.**
- **The evaluation is mine**, on a standard I wrote, using criteria I chose. Blind scoring by someone
  else would be worth more than another run.

## Conclusion

These tests do not show that the standard improves agent-written documentation. One apparent effect
appeared under a confound and disappeared without it.

What the standard demonstrably does is narrower: it makes the output shorter for the same content, and
it records decisions — which passive to prefer and why, what a style sheet is for, which claims are
craft and which are measured — that would otherwise be re-argued or silently re-decided each time. That
is a real function, and it is not the function these tests were built to measure.
