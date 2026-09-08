# The base standard

Four things determine whether a piece of writing can be read once, out of order, by someone who was
not in the conversation that produced it: what its words point at, where it puts information, how much
the reader has to carry, and how firmly its claims are held.

This document covers those four. It does not cover how to fix a draft that already exists
(`editing.md`), which spelling or term a particular document settled on (`style-sheet.md`), or the
places where one of these defaults is wrong for a specific kind of output (`outputs/`). Each of those
is named because it is somebody else's job, not because it is unimportant.

One caution before the substance. Most of what follows is craft, some is a formal model, and a little
is measured — in settings that may not resemble yours. `research/` records which is which for every
claim, and `README.md` summarises it.

It has since been tested twice, and neither run showed a measurable improvement: one comparison was
invalidated by its own design, and the other separated the two documents on nothing except length. A
single unblinded human reader preferred the standard-written document. `experiments/results.md` reports
all of it. Nothing here is demonstrated to produce better documents.

---

## 1. Reference — what a word points at

A reader meeting *the record*, *it*, or *that role* has to resolve the phrase against something. If the
referent is not on the page, or is ambiguous between two candidates, the reader either stops or — more
often — carries on with the wrong one and never finds out.

Halliday and Hasan call the connection a **tie**, and classify the ways one is made: reference,
substitution, ellipsis, conjunction, and lexical cohesion. The practical consequence is that a
document has a small number of devices for pointing backwards, and each fails differently.

**One name per thing.** Varying a term for elegance — *case manager*, then *the manager*, then *that
role* — makes the reader ask whether three things are being discussed or one. Repetition looks clumsy
to the writer, who already knows the answer, and reads as precision to everyone else.

**A pronoun is licensed only when the sentence is already about the thing it refers to.** Centering
theory states this formally:

> "If any element of Cf(Un) is realized by a pronoun in Un+1, then the Cb(Un+1) must be realized by a
> pronoun also… no element in an utterance can be realized as a pronoun unless the backward-looking
> center of the utterance is realized as a pronoun also."
> — Grosz, Joshi & Weinstein, *Computational Linguistics* 21(2), 1995, §6, p.214

Read practically: do not pronominalise a secondary entity while spelling out the main one. If the
sentence has moved on to a different subject, the old one needs its noun back.

**Names from another vocabulary are reference devices too.** An identifier — `AllowsDownloadTo`, a file
path, an error code — resolves more precisely than any phrase, and it is greppable. With a reader who
shares it, use it plainly. With a reader who does not, translate it once where it first crosses over
and say why they can now ignore it. Dumping it raw and silently omitting it are both failures; the
second is worse, because the reader later discovers something was hidden.

---

## 2. Order — where information goes

Two positions in an English sentence do different work, and confusing them is the most common
structural fault in technical prose.

> "In the stress position the reader needs and expects closure and fulfillment; in the topic position
> the reader needs and expects perspective and context."
> — Gopen & Swan, "The Science of Scientific Writing", *American Scientist* 78(6), 1990

**Topic position** — the front — announces whose story this is. *"Readers expect a unit of discourse to
be a story about whoever shows up first."* **Stress position** — the point of syntactic closure, not
merely the last word — is where emphasis lands. A semicolon or colon creates a secondary one.

This is why the passive is not a vice:

> "'Bees disperse pollen' and 'Pollen is dispersed by bees' are two different but equally respectable
> sentences about the same facts. The first tells us something about bees; the second tells us
> something about pollen."

Choose by what the paragraph is about, not by a rule against passives. Note that this puts Gopen and
Swan in direct conflict with plain-language guidance that prescribes the active voice; where they
disagree, this standard follows the topic-position account, because it explains *when* each is right
rather than banning one.

The ordering principle that follows is narrower than the folk version. Old information **that links
backward** belongs in topic position; new information **you want emphasised** belongs in stress
position. Material that does neither can go either way. The authors are explicit that no blanket rule
survives:

> "None of these reader-expectation principles should be considered 'rules.' Slavish adherence to them
> will succeed no better than has slavish adherence to avoiding split infinitives or to using the
> active voice instead of the passive."

**Keep the subject next to its verb.** An interruption between them holds the reader in suspense while
they store the subject, and the store is what runs out.

**Two axes, not one.** Williams separates them:

> "Sentences are cohesive when the last few words of one sentence set up the information that appears
> in the first few words of the next." (p.67)

> Coherence "is when all the sentences in a piece of writing add up to a larger whole." (p.69)
> — Williams & Bizup, *Style: Lessons in Clarity and Grace*, quoted by the UW-Madison Writing Center

A passage can be perfectly cohesive sentence-to-sentence and still add up to nothing. Diagnose them
separately.

**A diagnostic worth knowing.** List what occupies the front of every sentence in a paragraph. That
list is whose story the paragraph tells. If it is a jumble, the paragraph has no subject, whatever its
topic sentence claims.

---

## 3. Load — what the reader has to carry

Cognitive load theory separates the difficulty that belongs to the material from the difficulty the
presentation adds. In Sweller's own recent work the distinction is **intrinsic** and **extraneous**; the
once-standard third category, germane load, is absent — so treat the three-way version as obsolete
rather than settled.

Only extraneous load is yours to remove. Two of its forms are worth telling apart, because the fixes
differ: **redundancy** is the same information delivered twice, and the cure is deletion.
**Split-attention** is two parts that are each unintelligible alone and only make sense together — a
definition three sections from its use, a table whose key sits on another page — and the cure is to
move them together, not to cut either.

This is the empirical corner of the standard, and its evidence comes from instructional multimedia
rather than from technical documents. Treat it as a well-motivated model, not a demonstrated result.

The practical form: **the reader should never have to hold something that is not in front of them.**
Every rule in section 1 and most of section 2 are instances of it.

---

## 4. Status — how firmly a claim is held

Nothing in the reading literature covers this. It comes from ordinary scientific-communication
practice, and it is the facet most often missing from engineering writing.

A reader cannot tell what is safe to challenge unless each claim carries its standing.

**Grade a reason.** Three exclusions, three different statuses:

> *Decided* — "deliberately excluded, because it is the authority of a different stage."
> *Defaulted* — "excluded for no reason beyond not appearing in the brief for this phase."
> *Judged* — "we took this to be an obvious slip and removed it."

Rendering all three as "we excluded X" destroys the reader's ability to tell which one to push on.

**Separate what you observed from what you infer.** Report the observation and its source. Mark a
causal or mechanistic claim as inference. Do not promote an explanation to a principle because it is
tidy — this standard's own first draft did exactly that, and `research/` exists because of it.

**Source what is sourceable.** Where an authority exists — a statute, a spec, a prior decision — quote
and cite it rather than recalling it.

**Say what you did not do.** Not decided, not covered, not enforced. Silence reads as completeness.

**Give the weakest part the plainest sentence.** The reader will find it anyway; the only variable is
whether they find it from you.

---

## What this standard cannot do

It cannot make a document consistent. "One name per thing" states a goal; the artifact that achieves it
is a **style sheet**, kept per document, recording which term was chosen where more than one was
possible. See `style-sheet.md`.

It cannot fix a draft. Applying these ideas to text that already exists is a different activity with
its own order of operations and its own restraint. See `editing.md`.

It is not enforced. No tool objects when a term drifts or a pronoun loses its antecedent. That is why
the editing protocol exists, and why the only part of this system that can be *run* rather than
intended is the checklist there.
