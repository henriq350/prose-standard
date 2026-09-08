# Prose standard

A standard for prose that a reader has to get through once, out of order, without the conversation
that produced it: PR descriptions, commit messages, design notes, specifications, and documents for
someone outside the team. The standard itself is `base.md` — four constraints (reference, order, load,
status), about 1,500 words, meant to be read whole before you write anything. Everything else in this
repository either narrows it for a specific situation, supports revising instead of writing, records
the evidence behind it, or tests whether it does anything.

This repository is packaged to be installed into another project as a Claude Code skill, but nothing
about the content is tool-specific — `base.md` is a document you can read and apply by hand.

## Whether it is worth your time

Two comparisons exist in `experiments/`, and both are honestly reported in `experiments/results.md`,
which is the source for everything in this section.

The first pitted a document written with `base.md` against one written without it, on the same task.
It is marked invalid by its own author: both writers had also been given prose already written under
the standard as background reading, so the "control" could imitate the target register from its
sources rather than write independently. The one measurable gap it produced — fewer sentences opening
on a bare pronoun (8.8% vs. 14.5%) — did not survive the second test.

The second comparison closed that gap: both writers worked from code only, with no prose written under
the standard anywhere in their input. On that test, no criterion the author had committed to in advance
separated the two documents — not forward references, not terminological drift, not whether a
deliberate trade-off was framed as deliberate. The one difference that held up was length: the
standard-following document was 13% shorter for the same source material and used 92 sentences against
133. The bare-pronoun rate, the one signal from the first test, reversed (8.7% vs. 6.8%, this time in
the control's favour). The scoring in both tests was done by the person who wrote the standard,
unblinded, against criteria they chose themselves — a limit the results file states about its own method,
not one this README is adding.

`experiments/results.md` states its own conclusion plainly: these tests do not show that the standard
produces better documents. What they do show is narrower — shorter output for the same content, and a
record of decisions (which passive to prefer and why, what a style sheet is for, which claims here are
craft and which are measured) that would otherwise be re-argued, or silently re-decided, each time
someone writes. That narrower claim is what this repository is worth reading for.

## How the pieces relate

- **`base.md`** — the standard. Four sections: reference (what a word points at), order (where
  information goes in a sentence), load (what the reader has to carry), status (how firmly a claim is
  held). Read it whole, once, before writing.
- **`editing.md`** — a different activity: revising a draft that already exists, not writing a new one.
  Four levels, adapted from the JPL "Levels of Edit" system, each stating what it must not touch and
  handing that off to the next. Ends in a mechanical deletion pass, the one part of this system that
  can be run rather than merely intended.
- **`style-sheet.md`** — a template, not a guide. Kept per document, not per project, for the decisions
  `base.md` cannot make in advance: which of several possible names for a thing this document uses,
  which term crosses in from another vocabulary and where it was glossed, open questions raised during
  editing and not yet resolved.
- **`outputs/README.md`** — carve-outs. States, for commit messages, PR descriptions, documents for a
  reader outside the team, design notes and decision records, and specifications, only what differs
  from `base.md`'s defaults and why. Silence on an output type means nothing differs for it.
- **`.claude/skills/prose/SKILL.md`** and **`.claude/output-styles/agent-responses.md`** — the
  operational form. The skill tells an agent to read `base.md` before producing a written artifact, and
  routes to `editing.md`, `outputs/README.md`, or `style-sheet.md` depending on the task. The output
  style is separate and governs an agent's own conversational replies, which are turns, not artifacts,
  and are held to a different (shorter) set of rules.
- **`research/`** — the evidence behind every quoted claim in `base.md` and `editing.md`. Four
  documents, close to 25,000 words total, one per cluster of sources (reader expectation and sentence
  order; document-architecture systems like Diátaxis, Google's and Microsoft's style guides;
  cohesion/centering/cognitive-load theory plus how law, plain-language and journalism codify similar
  ideas; the editing and style-sheet literature). Each source is graded twice: by access — was the
  primary text read directly, or only a secondary paraphrase, or not obtained at all — and, in most of
  the four documents, by kind — a formal theoretical model, a measured effect, or professional craft
  with no study behind it. Read them for that grading, not as a settled bibliography; several sources
  the researcher wanted could not be obtained and are recorded as gaps rather than silently dropped.
- **`experiments/`** — the two comparisons above, plus the criteria committed to before each one was read,
  and `results.md`, which reports what they showed. The documents themselves are under
  `experiments/outputs/`.

## What it claims, and on what basis

`base.md` grades its own four sections, and the research files back that grading up with sources rather
than merely asserting it:

- **Reference** rests on named, checkable formalisms: Halliday & Hasan's taxonomy of cohesive ties, and
  centering theory's rule for when a pronoun is licensed (`research/03`, both read from the primary
  text). Centering theory's rule is the single most precise, mechanically checkable claim in the whole
  standard; the psychological studies said to validate it were not independently read by the research
  and are cited, not verified.
- **Order** rests on Gopen & Swan's topic/stress-position account, read directly from the primary
  article, and on Williams & Bizup's cohesion/coherence distinction, read only through a university
  writing-center page that quotes it with page numbers the research could not check against the book
  itself (`research/01`). Both are argued from editorial experience, not from a controlled study.
- **Load** is, in `base.md`'s own words, "the empirical corner of the standard, and its evidence comes
  from instructional multimedia rather than from technical documents." The intrinsic/extraneous split
  is a live theoretical model, not settled; the once-standard third category, germane load, is missing
  from the theory's own originator's recent work. The individual named effects redundancy and
  split-attention are each attributed to specific studies the research did not read firsthand
  (`research/03`).
- **Status** — distinguishing decided from defaulted from judged, marking inference as inference — is
  the one facet `base.md` does not cite anywhere. It says so itself: "nothing in the reading literature
  covers this," and attributes it only to "ordinary scientific-communication practice," unnamed. No
  research file documents this specific three-way grading; the closest material, in `research/04`, is
  about professional editing practice (JPL's edit levels, CIEP's definitions, Carol Fisher Saller), and
  it grounds `editing.md` and `style-sheet.md`, not this claim in `base.md`. It is the weakest-evidenced
  of the four facets, and `base.md` states that plainly rather than dressing it up — which is itself the
  facet's own rule, applied to itself.

None of this is evidence that following the standard produces a better document; see the results above.
The grading is a record of which claims are borrowed, from where, and how firmly — nothing more.

## Installing and using it

There is no package manager entry point, and no install script in this repository.
`.claude/skills/prose/SKILL.md` states its own fallback: "Paths are relative to this repository. If it is installed elsewhere, they are
relative to the skill directory." That only works if the layout travels together — the skill and
output style copied into the target project's own `.claude/` directory, and `base.md`, `editing.md`,
`style-sheet.md` and `outputs/` copied alongside them, at the same relative positions they hold here.
This repository was itself extracted out of a client codebase once the standard outgrew living inside
one project. Edit the standard here, and re-copy into any project that has installed it — not the other
way around.

Once installed: read `base.md` in full before writing anything new. If the output is a commit message,
a PR description, an issue, a specification, a design note, or anything for a reader outside the team,
also read the matching entry in `outputs/README.md`. If you are revising a draft that already exists,
read `editing.md` instead of applying `base.md` directly — it is a different activity, with its own
order of operations. If the document has more than one term for the same thing, or a term that crosses
in from a vocabulary the reader may not share, start a `style-sheet.md` beside it, from the template.

None of this is enforced by tooling. Nothing objects when a term drifts or a pronoun loses its
antecedent. The deletion pass in `editing.md` is the one part that can be run as a checklist rather than
merely intended; everything else depends on the writer applying it.
