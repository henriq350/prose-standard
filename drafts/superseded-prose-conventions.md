---
id: note-prose-conventions
title: "Prose conventions — evidence, examples, and what is only inferred"
slug: /design-notes/prose-conventions
sidebar_position: 9
---

# Prose conventions

Reference for `.claude/output-styles/prose.md`. That file states three tests and a deletion pass; this
one holds the worked examples, records where they came from, and marks which parts are untested.

## Provenance

**What happened.** A pt-PT document was produced for the law firm setting out six open legal questions
(`/home/miew/briefings/questoes-juridicas/`). Its requester judged it unusually readable, against a
stated frustration with documentation that refers to concepts it never introduces. The rules were
extracted afterwards by reading that document for recurring sentence-level patterns.

**What that is evidence of.** One document, one reader, one favourable judgement. The patterns are
demonstrably *present* in a text that was received well. Whether they *caused* it is not established:
no comparison against a control, and the document also had a capable model on a single-purpose task, a
verbatim statutory source to check against, and a brief that banned a specific vocabulary.

## The constraint the three tests share

Stated without any claim about cognition: *a unit should be understandable without holding state that
is not in front of you.*

That is not specific to prose, which is the reason to expect the tests to generalise. The
correspondence with functional-programming disciplines is structural:

| Discipline | What it removes from working memory | Test |
| :--- | :--- | :--- |
| Referential transparency | The execution history behind a value | 1 — references resolve |
| Immutability | "Did something change this behind me?" | 1 — one name per thing |
| Local reasoning | Needing to read elsewhere to understand here | 1 — define in the clause |
| Explicit effects (`Result`/`Option`) | Invisible failure paths | 2 — claims carry their status |
| Exhaustiveness checking | "Which cases were left out?" | 3 — failure in both directions |

The backend already commits to the fourth row: handlers return `Result<T>` rather than throwing, so a
failure path is visible at the call site instead of implied. The writing convention and the code
convention answer the same complaint.

**The asymmetry is the operative part.** In code these properties are enforced — the compiler rejects
a non-exhaustive match, the type system makes a hidden effect impossible to ignore. In prose nothing
is enforced. No tool objects when a pronoun's antecedent is three sentences back, or when a term
quietly changes meaning. That is the argument for the deletion pass: a manual substitute for a
compiler, and the only part of the rule list that can be run rather than intended.

**Caveat.** This is an analogy between two design disciplines, not evidence about readers. It explains
why the tests cohere and gives a reason to expect them to transfer; it does not show that they improve
comprehension. Nobody has run that test.

An earlier draft made a stronger claim — that the rules work *because* working memory is small,
borrowed from [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) (MIT), which shapes
assistant turns for a reader with ADHD. That is plausible and unmeasured, so it is recorded here as a
hypothesis rather than stated as mechanism. The **deletion pass** is adapted from the same source; its
merit does not depend on the hypothesis, because a checklist is verifiable against a draft and an
instruction to write clearly is not.

## Examples, by test

Quotations are from the document named above and from PR #218 in this repository.

### Test 1 — every reference resolves on the page

Both halves of a behaviour sentence are required: the consequence, and something the reader can grep.

> `AllowsDownloadTo(party, instant)` is `IsVisibleTo(...) && AllowDownload` — a download can never
> outlive or outrank the right to read that it rests on.

> An unrecognised recipient is a 400 (`UnknownPortalRecipient`), never a silent fallback to
> case-wide.

Order is a judgement about which half carries more information; in the first, the signature is the
clearest statement of the rule, so it leads. The two failures:

- *Code with no consequence* — "the guard returns 409". Traceable, silent about what it means.
- *Consequence with no anchor* — "the system stops the case from advancing". Readable, unverifiable,
  unfindable. With a reader who shares the vocabulary this is the worse one: the information was free
  to keep.

Where the reader does not share the vocabulary, the term is glossed once and then dismissed:

> (besides system administration, which exists for maintenance and is not a participant in the
> procedure)

That stands in for `super_admin` — neither dumped raw nor silently omitted. Omission is how a reader
later discovers a whole role was hidden from them.

Reference stability across the same document: *gestor do processo*, *revisor jurídico*, *instrutor
designado* and *mandatário do arguido* appear in identical form every time. And the noun is
re-established before a pronoun is used:

> "Este registo não é uma formalidade administrativa: é dele que o sistema retira a data…"

### Test 2 — every claim carries its status

Three exclusions from one section, each with a different status:

> "Foi deixado de fora **deliberadamente**, por ser a autoridade de um momento diferente do
> procedimento." — decided
>
> "foi excluído, **sem outra razão que não** a de não figurar como interveniente nesta fase…" —
> defaulted
>
> "a equipa **entendeu tratar-se de lapso manifesto** e removeu-o." — judged

A reader can tell instantly which is safe to overturn. Rendering all three as "we excluded X" removes
that.

The weakest part gets the plainest sentence:

> "O sistema **não avalia** a condução diligente do inquérito. Assume-a."

Two words in the second sentence, and the hardest thing in the document to skim past.

### Test 3 — every statement is checkable

Both failure directions, in consequences the reader recognises:

> "Se a regra estiver errada em sentido permissivo, o sistema deixa avançar um processo já caducado e
> a empresa notifica uma nota de culpa inútil, com o vício a manifestar-se apenas em juízo. Se
> estiver errada em sentido restritivo, o sistema bloqueia processos legítimos…"

A worked instance the reader can check arithmetically:

> "conclui o inquérito em 20 de maio… notificada em 5 de julho. Nessa data já decorreram mais de 60
> dias"

A restatement of the rule cannot be checked; this can.

## Rules deliberately not adopted

From the same source:

- **"Cap lists at five items."** Wrong for reference material — a glossary has sixty entries and
  should. Not carried over; long lists the reader must *act* on are a different case, and ranking them
  is covered by leading with the decision.
- **"Number multi-step tasks."** A rule about instructions, not explanatory prose, and the fixed
  schema it implies was explicitly not wanted.
- **"Restate state every turn."** Applies to a conversation, not to an artifact read once by someone
  who was not party to it. Its concern is covered by test 1.

## Open

- None of this is measured. A cheap test: give two readers the same content written with and without
  the tests, and compare answer rate or comprehension. Nobody has.
- The rules were derived from a document for a lay-but-expert reader. Fit for a purely internal
  engineering audience is assumed, not shown — PR #218 is the only other sample.
