---
name: prose
description: Write or revise prose that another person will read — design notes, PR descriptions, commit messages, issues, specifications, documents for clients or counsel. Use when producing any written artifact, or when asked to improve one that exists.
---

# Prose

Read `base.md` before writing. It is the standard: four facets — reference, order, load, status — with
worked examples. It is about 1 500 words and is meant to be read whole, once, before you start.

Then, depending on what you are doing:

| Situation | Read |
| :--- | :--- |
| Writing something new | `base.md` |
| The output is a commit message, PR description, issue, spec, design note, or a document for someone outside the team | `base.md` + the matching section of `outputs/README.md` |
| Revising a draft that already exists | `base.md` + `editing.md` |
| The document has terms that could be named more than one way | start a `style-sheet.md` beside it |

Paths are relative to this repository. If it is installed elsewhere, they are relative to the skill
directory.

## The short version, if you read nothing else

- **Reference.** One name per thing, repeated. A pronoun only when the sentence is already about the
  thing it refers to. Definitions inside the clause, at first use. An identifier is a precise
  reference — use it plainly with a reader who shares it, gloss it once where they do not.
- **Order.** The front of a sentence says whose story this is; the point of closure carries the
  emphasis. Old information that links backward goes first; new information you want stressed goes
  last.
- **Load.** The reader should never have to hold something that is not in front of them.
- **Status.** Distinguish decided from defaulted from judged. Mark inference as inference. Say what you
  did not do.

## Two things this skill does not cover

**Your own replies to the user.** Those are turns, not artifacts — different constraints, and a
different document: `.claude/output-styles/agent-responses.md`.

**Consistency.** `base.md` can ask for one name per thing; only a style sheet kept beside the document
can deliver it.

## Honesty about what this is

`experiments/results.md` records two controlled comparisons. Neither showed a measurable improvement,
and the one effect that appeared under a confound reversed without it. A human reader preferred the
standard-written document on the clean test, unblinded. Use it because its decisions are recorded and
defensible, not because it is proven.
