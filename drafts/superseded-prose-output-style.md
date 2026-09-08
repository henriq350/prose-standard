---
name: Prose
description: Three tests for prose a person reads — references resolve, claims carry their status, statements are checkable — plus a deletion pass.
---

# Prose

Applies to prose a person reads: PR bodies, commit messages, issue text, design notes, PRDs, specs,
client documents. Not to code comments — `.claude/rules/comments.md` is stricter and wins. Holds in
any output language.

Three tests, then a deletion pass. **The reader is a parameter, not a special case:** the tests are
the same for a colleague, a lawyer and a client, and only what counts as *resolved* or *checkable*
changes with what they already know.

Worked examples and provenance: `docs/content/design-notes/prose-conventions.md`.

## 1. Every reference resolves on the page

A reader must never have to hold, recall, or go looking for what a word points at.

- **One name per thing, repeated.** *Case manager* stays *case manager* — not "the manager", "the
  handler", "that role". Variation for elegance is the main cause of a reader stopping to ask whether
  two names mean the same thing.
- **Repeat the noun rather than reaching for a pronoun** across sentences.
- **Define in the clause, at first use** — "the **decider** — who approves the final decision — was
  not included". No glossary detour, no forward reference.
- **Carry the exact name.** Identifiers, paths, error codes and signatures are the precise handle:
  unambiguous and greppable. Use them plainly where the reader shares them; where they do not, gloss
  the term once at first crossing and say why they can ignore it. Never dump it raw, never silently
  drop it.

## 2. Every claim carries its status

The reader has to know how firmly a thing is held, or they cannot tell what is safe to challenge.

- **Grade reasons:** *decided* ("deliberately, because…"), *defaulted* ("for no reason beyond…"),
  *judged* ("we took this to be…"). Flattening the three into "we excluded X" is the most common way
  documentation loses its usefulness.
- **Separate what you observed from what you infer.** Report the observation and its source; mark a
  causal or mechanistic claim as inference. Do not promote an explanation to a principle because it
  is tidy.
- **Say what you did not do** — not decided, not covered, not enforced. Silence reads as
  completeness.
- **Source what is sourceable.** Where the repo holds the authority — a statute file, a spec, a PRD —
  quote and cite it rather than recalling it.
- **Give the weakest part the plainest sentence.** State errors and limits as cause then fix, flatly.
  Burying them costs more than admitting them.

## 3. Every statement is checkable

Prefer the form the reader can verify without asking you.

- **A worked instance beats an abstract statement.** Dates and quantities they can check themselves.
- **Quantify instead of qualifying.** Give the unit, and a range with its condition — not "some
  work".
- **Show failure in both directions**, in consequences the reader recognises: too permissive and too
  restrictive.
- **Lead with what they must do or decide;** reasoning follows. When offering options, label the
  current behaviour as current.
- **Bold the operative fact only.** Skimming the bold should still give the answer.

## The deletion pass

Nothing above is enforced by tooling, so run this before sending. Delete:

1. An opening sentence announcing what you are about to do.
2. A closing sentence recapping, or offering further help.
3. "Note that", "it is worth noting", "as mentioned above".
4. Any sentence explaining the document's purpose to its reader.
5. Hedging adverbs carrying no information — "perhaps", "possibly", "somewhat". Keep a hedge marking
   real uncertainty; deleting that one manufactures confidence.
6. Idioms and figurative phrases.
7. Marketing register in internal text: "powerful", "seamless", "robust".
8. Any restatement of the diff in a commit message.
9. Any claim you could have sourced and did not.
