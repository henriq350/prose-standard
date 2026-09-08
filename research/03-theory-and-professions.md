# 03 — Theory and Professions: Source Material for a Writing Standard

Compiled 2026-09-08. All URLs were fetched on 2026-09-08 unless otherwise noted.
Each source is marked **PRIMARY** (I read the source itself) or **SECONDARY** (I read
someone describing it). Each claim is marked **EMPIRICAL** (a study, with what was
measured), **THEORETICAL** (a model/formalism with no measurement), or **CRAFT**
(professional advice, no study cited). SOURCE sections quote or closely paraphrase the
source; MY CONCLUSIONS sections are clearly separated and are my own synthesis.

---

## PART A — THEORY

## A1. Halliday & Hasan, *Cohesion in English* (Longman, 1976)

### Status of this source
The physical book is not freely downloadable from a legitimate publisher site. I located
a scanned, OCR'd copy of the actual book uploaded to the Internet Archive
(identifier `139618384-halliday-hasan-cohesion-in-english`, no access restriction flag,
fetched via `https://archive.org/download/139618384-halliday-hasan-cohesion-in-english/139618384-Halliday-Hasan-Cohesion-in-English_djvu.txt`,
fetched 2026-09-08). This lets me quote the actual authored text, not a description of
it — so I am marking it **PRIMARY**, but with two caveats: (1) it is OCR output, so
spacing/spelling artifacts exist (e.g. "TEx" for "TEXT", "rorat" for "total") and page
numbers are sometimes garbled by the OCR (e.g. "II" for "11", "I2" for "12"); (2) this
is an unauthorized scan of a copyrighted book, not the publisher's edition, so I cannot
guarantee it matches every later printing pagination. Quotes below are transcribed as
literally as the OCR renders them, with obvious OCR noise noted in brackets.
A second copy exists on Archive.org under `cohesioninenglis0000hall` but that one is
access-restricted (controlled digital lending) and could not be fetched
(`archive.org/metadata/cohesioninenglis0000hall` → `"access-restricted-item": true`).

### SOURCE: the concept of "text" and "texture"

> "The word TEXT is used in linguistics to refer to any passage, spoken or written, of
> whatever length, that does form a unified whole." (Ch.1, p.1, OCR)

> "The concept of TEXTURE is entirely appropriate to express the property of 'being a
> text'. A text has texture, and this is what distinguishes it from something that is
> not a text. It derives this texture from the fact that it functions as a unity with
> respect to its environment." (Ch.1 §1.1.2, p.2, OCR)

The book's opening worked example (their own, verbatim):

> "[1:1] Wash and core six cooking apples. Put them into a fireproof dish.
> It is clear that *them* in the second sentence refers back to (is ANAPHORIC to) the
> six cooking apples in the first sentence. This ANAPHORIC function of *them* gives
> cohesion to the two sentences, so that we interpret them as a whole..." (p.2, OCR)

### SOURCE: the definition of a cohesive "tie" and the named taxonomy

> "The different kinds of cohesive tie provide the main chapter divisions of the book.
> They are: reference, substitution, ellipsis, conjunction, and lexical cohesion. A
> preliminary definition of these categories is given later in the Introduction
> (1.2.4); each of these concepts is then discussed more fully in the chapter in
> question." (p.4, OCR)

> "Cohesion occurs where the INTERPRETATION of some element in the discourse is
> dependent on that of another. The one presupposes the other, in the sense that it
> cannot be effectively decoded except by recourse to it. When this happens, a relation
> of cohesion is set up, and the two elements, the presupposing and the presupposed, are
> thereby at least potentially integrated into a text." (§1.1.4, p.4, OCR)

Named example that gets three ties from three different categories:

> "[1:5] Time flies. — You can't; they fly too quickly. ... the cohesion is expressed in
> no less than three ties: the elliptical form *you can't* (Chapter 4), the reference
> item *they* (Chapter 2) and the lexical repetition *fly* (Chapter 6)." (p.4-5, OCR)

The formal semantic-relation criterion (why *he said so* is cohesive but *John said
everything* is not, and why *lying on the floor* is cohesive by ellipsis):

> "There is one specific kind of meaning relation that is critical for the creation of
> texture: that in which ONE ELEMENT IS INTERPRETED BY REFERENCE TO ANOTHER. ... Where
> the interpretation of any item in the discourse requires making reference to some
> other item in the discourse, there is cohesion." (§1.2.4, p.11-12, OCR)

### SOURCE: the cohesion-vs-coherence ("texture") distinction

This is the load-bearing passage for the concept the user asked about. Note the book
itself does not oppose "cohesion" to "coherence" as two rival concepts in the way later
commentary sometimes implies; it treats COHESION plus REGISTER together as jointly
constituting a coherent TEXT:

> "the texture involves more than the presence of semantic relations of the kind we
> refer to as cohesive, the dependence of one element on another for its
> interpretation. It involves also some degree of coherence in the actual meanings
> expressed... The concept of COHESION can therefore be usefully supplemented by that
> of REGISTER, since the two together effectively define a TEXT. A text is a passage of
> discourse which is coherent in these two regards: it is coherent with respect to the
> context of situation, and therefore consistent in register; and it is coherent with
> respect to itself, and therefore cohesive. Neither of these two conditions is
> sufficient without the other, nor does the one by necessity entail the other."
> (Ch.7, "The meaning of cohesion," OCR)

So in Halliday & Hasan's own terms: **cohesion** = the semantic ties (reference,
substitution, ellipsis, conjunction, lexical cohesion) that make interpretation of one
element depend on another; **coherence** (their word, used loosely, twice, for two
different kinds of "hanging together") = (a) consistency of register/situation, and (b)
the cohesive hanging-together of the text itself. A passage can have register-coherence
without cohesion (and vice versa), and both are needed for something to count as "a
text."

### Evidential status
**THEORETICAL.** This is a descriptive taxonomy built from constructed and
naturally-occurring examples, not an experiment. No populations, no measurement, no
statistics. It is presented as a systematic grammar of English cohesive devices, and
Halliday & Hasan repeatedly say things like "we shall be calling" and "we shall discuss
in Chapter X" — a classification system, not a tested hypothesis.

---

## A2. Centering theory — Grosz, Joshi & Weinstein, "Centering: A Framework for
Modeling the Local Coherence of Discourse," *Computational Linguistics*, Vol. 21,
No. 2 (1995), pp. 203–225

**PRIMARY.** Full text fetched directly: `https://aclanthology.org/J95-2003.pdf`
(ACL Anthology, fetched 2026-09-08, 200 OK, 24 pages).

### SOURCE: the motivating phenomenon (why one discourse "hangs together" and another doesn't)

> "(1) a. John went to his favorite music store to buy a piano. b. He had frequented
> the store for many years. c. He was excited that he could finally buy a piano. d. He
> arrived just as the store was closing for the day.
> (2) a. John went to his favorite music store to buy a piano. b. It was a store John
> had frequented for many years. c. He was excited that he could finally buy a piano.
> d. It was closing just as John arrived.
> Discourse (1) is intuitively more coherent than Discourse (2)." (§2, p.205)

### SOURCE: Cb, Cf, and the three transition types (exact formal definitions)

> "Each utterance U in a discourse segment (DS) is assigned a set of forward-looking
> centers, Cf(U, DS); each utterance other than the segment initial utterance is
> assigned a single backward-looking center, Cb(U, DS)." (§3, p.208)

The three named transitions, given verbatim (this is the "continue / retain / shift"
taxonomy the brief asked for):

> "1. CENTER CONTINUATION: Cb(Un+1) = Cb(Un), and this entity is the most highly ranked
> element of Cf(Un+1). In this case, Cb(Un+1) is the most likely candidate for
> Cb(Un+2); it continues to be Cb in Un+1, and continues to be likely to fill that role
> in Un+2.
> 2. CENTER RETAINING: Cb(Un+1) = Cb(Un), but this entity is not the most highly ranked
> element in Cf(Un+1). In this case, Cb(Un+1) is not the most likely candidate for
> Cb(Un+2); although it is retained as Cb in Un+1, it is not likely to fill that role in
> Un+2.
> 3. CENTER SHIFTING: Cb(Un+1) ≠ Cb(Un)." (§3, p.209)

Worked example showing CONTINUE → RETAIN → SHIFT in sequence (their own numbered
example (20), reproduced exactly, with their bracketed Cb/Cf annotations):

> "(20) a. John has been having a lot of trouble arranging his vacation. b. He cannot
> find anyone to take over his responsibilities. (he = John) [Cb = John; Cf = {John}]
> c. He called up Mike yesterday to work out a plan. (he = John) [Cb = John; Cf =
> {John, Mike}] (CONTINUE) d. Mike has annoyed him a lot recently. [Cb = John; Cf =
> {Mike, John}] (RETAIN) e. He called John at 5 AM on Friday last week. (he = Mike)
> [Cb = Mike; Cf = {Mike, John}] (SHIFT)." (§7, p.217)

### SOURCE: Rule 1 — when a pronoun is licensed vs. when a full NP is required

This is the exact answer to the question the brief asked ("what licenses a pronoun
vs. requires a full noun phrase"):

> "Rule 1: If any element of Cf(Un) is realized by a pronoun in Un+1, then the Cb(Un+1)
> must be realized by a pronoun also. In particular, this constraint stipulates that no
> element in an utterance can be realized as a pronoun unless the backward-looking
> center of the utterance is realized as a pronoun also... the use of a pronoun to
> realize the CB signals the hearer that the speaker is continuing to talk about the
> same thing." (§6, p.214)

In plain terms: you may pronominalize a *non-Cb* forward-looking-center only if the Cb
itself is also pronominalized; if the Cb is not being pronominalized (e.g., because it
is not being continued and needs re-establishing), it — and by the rule's logic,
everything else pronominalized in that clause — is constrained accordingly. The rule
directly explains why (20d)'s "Mike has annoyed him a lot recently" is fine (Cb=John is
still pronominalized as "him") but a full-NP re-mention becomes necessary once the Cb
actually shifts, as in (20e), to re-anchor the reader.

### SOURCE: Rule 2 — the preference ordering among transitions

> "Rule 2: Sequences of continuation are preferred over sequences of retaining; and
> sequences of retaining are to be preferred over sequences of shifting." (§6, p.214)

> "Rule 2 reflects our intuition that continuation of the center and the use of
> retentions when possible to produce smooth transitions to a new center provides a
> basis for local coherence. In a locally coherent discourse segment, shifts are
> followed by a sequence of continuations characterizing another stretch of locally
> coherent discourse. Frequent shifting leads to a lack of local coherence..." (§6,
> p.214)

### SOURCE: claim about empirical validation of Rule 1 (cited, not run by this paper)

> "Psychological research (Gordon, Grosz, and Gilliom 1993; Hudson-D'Zmura 1988) and
> cross-linguistic research (Di Eugenio 1990; Kameyama 1985, 1986, 1988; Walker, Iida,
> and Cote 1990, 1994) have validated that the CB is preferentially realized by a
> pronoun in English and by equivalent forms (i.e., zero pronouns) in other languages."
> (§6, p.214)

**I have not independently verified Gordon, Grosz & Gilliom (1993) or Hudson-D'Zmura
(1988)** — I have only the citing sentence above, from the primary paper. Treat the
underlying empirical claim as reported-but-unverified by me.

### Evidential status
**THEORETICAL**, as a framework and formal apparatus (Cb, Cf, Rule 1, Rule 2) — the
paper itself states and motivates the rules with constructed examples and argument, not
with a controlled study. **EMPIRICAL claim embedded but not run here**: the paper cites
external psycholinguistic and cross-linguistic studies (not read by me) as validating
that pronouns are preferentially used for the Cb. The core formalism (Cb/Cf, transition
types, Rule 1/Rule 2) is Grosz/Joshi/Weinstein's own theoretical contribution;
the claim that this formalism predicts human processing difficulty/coherence judgments
is attributed by them to the cited studies, which I did not read directly.

---

## A3. Cognitive load theory (Sweller) and Mayer's multimedia-learning principles

### A3.1 What I could and couldn't get directly from Sweller (1988)

I could **not** fetch Sweller's original 1988 *Cognitive Science* paper ("Cognitive
Load During Problem Solving") full text. Candidate URLs (course mirrors, journal
archive) either 404'd or the host would not resolve
(`csjarchive.cogsci.rpi.edu` — DNS failure from this environment; Wiley's page for the
DOI returned 403 Forbidden). I did **not** reconstruct its content from memory. This is
an honest gap: I cannot quote Sweller (1988) directly.

I *could* fetch the Springer abstract page for a related classic Sweller review:

**PRIMARY (abstract only)**: Sweller, J., van Merriënboer, J. J. G., & Paas, F. G. W. C.
(1998). "Cognitive Architecture and Instructional Design." *Educational Psychology
Review*, 10(3). `https://link.springer.com/article/10.1023/A:1022193728205`
(fetched 2026-09-08). I only have the abstract, not the full paper (paywalled):

> "Cognitive load theory has been designed to provide guidelines intended to assist in
> the presentation of information in a manner that encourages learner activities that
> optimize intellectual performance. The theory assumes a limited capacity working
> memory that includes partially independent subcomponents to deal with auditory/verbal
> material and visual/2- or 3-dimensional information as well as an effectively
> unlimited long-term memory, holding schemas that vary in their degree of automation."
> (Abstract)

### A3.2 What I *could* get directly: a 2026 Sweller-coauthored review

**PRIMARY, full text**: Darejeh, A., Marcus, N., Mohammadi, G., & Sweller, J. (2026).
"Cognitive Load Measurement Methods for Usability Testing: A Critical Analysis and
Framework for Interface Evaluation." *Human Factors*, 68.
DOI: 10.1177/00187208261427867. PMC13219774. Fetched in full via Europe PMC's
fullTextXML endpoint (`https://www.ebi.ac.uk/europepmc/webservices/rest/PMC13219774/fullTextXML`,
2026-09-08). This is co-authored by John Sweller himself, so I am treating its
restatement of CLT as authoritative/primary for the current state of the theory.

> "Cognitive load is defined as the cognitive resources required to acquire a concept
> or learn a procedure (Sweller, 1988, 2011). CLT emphasises the limitations of working
> memory when processing new information during learning or problem solving." (Intro)

> "Intrinsic Load ... is defined as the natural complexity level of a specific
> instructional topic or an entity such as a mathematical concept, software, or a
> device. It is fixed and cannot be changed, except by altering the entity design or
> knowledge level of the learner... Intrinsic load is determined by the number of
> information elements that must be processed simultaneously and the extent to which
> they interact to achieve a learning goal (Sweller, 1994)." (§ Human Cognitive
> Architecture and CLT)

> "Extraneous cognitive load is related to the difficulty imposed by the method used to
> present instructional materials or the complexity level of an interface or a device
> as a result of the way they have been designed... extraneous cognitive load stems
> from preventable design inefficiencies that add unnecessary mental effort beyond what
> is required..." (same section)

**Notable finding, worth flagging explicitly**: this 2026 review, co-authored by
Sweller, **does not mention "germane load" at all** — it only discusses intrinsic and
extraneous load. This is consistent with a documented drift in Sweller's later work
(reported secondhand by Wikipedia, see below) away from treating germane load as an
independent third category. I flag this as a live disagreement/evolution in the theory,
not settled doctrine.

**Generalization beyond instructional multimedia** — this paper's whole subject is CLT
applied to *software usability testing*, not classroom instruction: it reviews 87
studies applying cognitive-load measurement (NASA-TLX, EEG, fNIRS, eye-tracking, dual-task
paradigms, pupillometry, etc.) to evaluate interfaces including virtual reality
environments, electronic health records, surgical navigation systems, and cyber-operations
software. This is direct, source-based evidence that CLT is actively used well outside
instructional-multimedia contexts — as applied usability research, not just theory.

> "Table 5. Studies That Used the NASA-TLX Test to Evaluate Usability... Personal
> health record system... Electronic health records... Surgical navigation system...
> Cyber operations battlefield application." (Results tables 4–18, listing 87 studies)

### A3.3 Mayer's principles (coherence, signaling, redundancy, split-attention /
spatial contiguity)

I could not reach Richard Mayer's own site or an open primary text of *Multimedia
Learning* directly. I instead read a peer-reviewed, open-access literature review that
quotes Mayer's and Sweller's own definitions with citations:

**SECONDARY (but a peer-reviewed literature review, not a blog)**: Trypke, M.,
Stebner, F., & Wirth, J. (2023). "Two types of redundancy in multimedia learning: a
literature review." *Frontiers in Psychology*. DOI: 10.3389/fpsyg.2023.1148035.
PMC10192876. Full text fetched via Europe PMC (2026-09-08).

> "According to CTML, the redundancy effect refers 'to any multimedia situation in
> which learning from animation (or illustration) and narration is superior to
> learning from the same materials along with printed text that matches the
> narration' (Mayer et al., 2001, p. 153)." — this is the review quoting Mayer et al.
> (2001) directly, page-cited.

> "In cases of redundancy, learners must process unnecessary information... (CTML
> refers to the exclusion of unnecessary information as the 'coherence principle';
> Mayer and Fiorella, 2014)."

> "Before CLT established this definition of redundancy, prior studies investigated
> the split-attention effect (e.g., Tarmizi and Sweller, 1988). Both the redundancy
> effect and the split-attention effect deal with multiple sources of information (e.g.,
> visualization and written text) and the associated increase in extraneous cognitive
> load (Sweller et al., 2011). However, unlike the redundancy effect, the
> split-attention effect only occurs if two sources of information are unintelligible
> in isolation but are each essential to achieving the learning goal (Sweller et al.,
> 2011)."

> "McCrudden et al. (2014) identified the signaling principle (see text-based cueing;
> Van Gog, 2014) as a further moderating factor. They explained that the inclusion of
> duplicated text segments guided learners' attention to the relevant aspects of the
> learning material... This finding also aligns with the spatial contiguity principle
> (Mayer and Fiorella, 2014), which claims that integrating written text into a
> visualization promotes learning."

**Precise distinction, as the review states it**: redundancy = same/unnecessary
information duplicated across sources; split-attention = two sources that are each
*individually unintelligible* but jointly necessary, separated in space or time so the
learner must split attention to integrate them. These are different named effects with
different licensing conditions, not synonyms — worth keeping distinct in a writing
standard.

### A3.4 SECONDARY, flagged unreliable in part: Wikipedia "Cognitive load"

`https://en.wikipedia.org/wiki/Cognitive_load` (fetched 2026-09-08). I am marking this
**SECONDARY and partially untrustworthy**: as of the fetch date, the article itself
carries an editorial maintenance banner reading (verbatim, on the live page):

> "This article may incorporate text from a large language model... It may include
> hallucinated information, copyright violations, claims not verified in cited sources,
> original research, or fictitious references... this early-2025 edit; note WP:AISIGNS
> in superficial analyses, vocab distribution typical of 2024-25 LLMs, etc (February
> 2026)"

That banner is attached specifically to the article's "Effects of the internet"
section, which I have **excluded entirely** from this briefing. I used only the
"Theory," "History," and "Categories" sections, which read as pre-existing,
citation-backed content, but I flag the whole article as reduced-confidence given that
the page is known to contain contested/possibly fabricated material elsewhere. Where
useful I have used it only to name effects (modality effect, split-attention effect,
worked-example effect, expertise-reversal effect, completion-problem effect) for further
lookup, not as a source of specifics.

### Evidential status summary for A3

- **THEORETICAL**: the three-way (or now two-way) load taxonomy itself, element
  interactivity as the mechanism of intrinsic load — a model, not a measurement.
- **EMPIRICAL, but only reported by citation, not read firsthand by me**: the specific
  learning effects (redundancy effect, split-attention effect, modality effect,
  worked-example effect, signaling/spatial-contiguity effect) are each attributed in the
  literature to specific experimental studies (e.g., Tarmizi & Sweller 1988, Mayer et
  al. 2001, McCrudden et al. 2014) which I did **not** read directly — I have the
  citing review's description and short quotes only.
- **EMPIRICAL, read firsthand (review paper, not original studies)**: the 2026 Human
  Factors review documents CLT's generalization to usability testing across 87 studies
  in non-instructional domains (healthcare software, VR, surgical navigation) — this
  paper itself is real, peer-reviewed, and I read it directly; but it is a *review*, so
  the underlying 87 primary studies are once-removed from me.
- Overall: **do not treat "cognitive load theory" as a single validated finding.** It
  is a theoretical framework with a cluster of separately-named, separately-supported
  empirical effects underneath it, and the framework itself is still being revised
  (e.g., germane load's status) by its own originator.

---

## PART B — HOW PROFESSIONS CODIFY IT

## B1. Plain language

### B1.1 US federal guidance — plainlanguage.gov (now migrated to digital.gov)

**PRIMARY.** `plainlanguage.gov/guidelines/` now redirects (301) to
`https://digital.gov/guides/plain-language` (GSA's Digital.gov, fetched 2026-09-08).
The page states explicitly:

> "This content is adapted from PlainLanguage.gov. Selections of PlainLanguage.gov
> content — especially those most relevant for digital teams — have been carried
> forward in this set of plain language guides. All of the original content from the
> PlainLanguage.gov website is archived in the PlainLanguage.gov GitHub repository."
> (`digital.gov/guides/plain-language`)

I did **not** separately fetch the GitHub-archived original site; I read the current
official successor pages. Key sub-pages fetched: `/guides/plain-language/principles`
and `/guides/plain-language/writing`.

> "Follow plain language guidelines... Have a topic sentence. Good opening sentences
> help organize the structure of writing. Use the active voice. Active voice helps the
> message stay clear and easy-to-read. Organize the information. Prepare readers for
> what to expect. Summarize lengthy documents up-front. Use tables where appropriate...
> Use lists." (`/guides/plain-language/principles`)

> "Active voice makes it clear who should do what. It eliminates ambiguity about
> responsibilities. Not 'It must be done,' but 'You must do it.' ... In an active
> sentence, the person or agency that's acting is the subject of the sentence. In a
> passive sentence, the person or item that is acted upon is the subject of the
> sentence." (`/guides/plain-language/writing`)

> "A hidden verb (or nominalization) is a verb converted into a noun. It often needs an
> extra verb to make sense. Hidden verbs come in two forms: Some have endings such as
> -ment, -tion, -sion, and -ance. Others link with verbs such as achieve, effect, give,
> have, make, reach, and take." (`/guides/plain-language/writing`)

The site makes an unsupported empirical-sounding claim I want to flag explicitly:

> "Research shows that content is easier to understand when you use language made up
> of, among other things, Shorter words, Short sections, Active voice, Present tense"
> (`/guides/plain-language/writing`)

No specific study is cited for that sentence on the page I fetched. This is exactly the
kind of claim rule 5 asks me to be strict about.

### B1.2 EU "How to Write Clearly"

**PRIMARY, full text (16 pages).** Confirmed the correct EU Commission booklet via
`op.europa.eu` publication record (title: "How to write clearly," Publications Office
of the EU), and downloaded the actual PDF via
`https://op.europa.eu/o/opportal-service/download-handler?identifier=725b7eb0-d92e-11e5-8fea-01aa75ed71a1&format=pdf&language=en`
(fetched 2026-09-08, 890,561 bytes, 16 pages, confirmed by extracting page 1: "How to
write clearly / Translation"). Bibliographic identity: European Commission,
Directorate-General for Translation, *How to write clearly*, Publications Office of the
European Union, Cellar ID `725b7eb0-d92e-11e5-8fea-01aa75ed71a1`.

Its ten named "Hints," quoted from the table of contents:

> "Hint 1: Think before you write... Hint 2: Focus on the reader... Hint 3: Get your
> document into shape... Hint 4: KISS: keep it short and simple... Hint 5: Make sense —
> structure your sentences... Hint 6: Cut out excess nouns — verb forms are livelier...
> Hint 7: Prefer active verbs to passive ones — and name the agent... Hint 8: Be
> concrete, not abstract... Hint 9: Beware of false friends, jargon and
> abbreviations... Hint 10: Revise and check."

Hint 6, the nominalization-reversal device, given with its own worked before/after
table (verbatim from the PDF):

> "by the destruction of → by destroying; for the maximisation of → for maximising; of
> the introduction of → of introducing... Many nouns ending in '-ion' are simply verbs
> in disguise." (Hint 6)

Hint 7, on naming the agent, with a worked example showing the actual failure mode of
an unnamed passive agent:

> "It is considered that tobacco advertising should be banned in the EU. Who
> considers? The writer, the Commission, the public, the medical profession or other?"
> (Hint 7)

> "You don't have to avoid passives at all costs though. They can be useful, for
> example when there's no need to say who is responsible for the action because it's
> obvious ('All staff are encouraged to write clearly')." (Hint 7)

### Evidential status for B1
**CRAFT.** Both documents are official style guidance, presented as "hints, not
rules" (the EU booklet's own words) and as institutional best practice. Neither
document I fetched cites a specific measured study for its specific recommendations
(the one exception is the vague, uncited "research shows" sentence flagged above, which
I am treating as an unsupported claim, not evidence).

---

## B2. Law: Wydick, Garner, Kimble

### B2.1 Bryan Garner — lawprose.org (his own site)

**PRIMARY.** `https://lawprose.org/lawprose-lessons/` (note: `www.lawprose.org` failed
DNS resolution from this environment; the bare `lawprose.org` resolved and worked,
fetched 2026-09-08). This is Garner's own long-running practitioner column, so it is a
primary craft source, not a study.

> "LawProse Lesson #496: Telling Good from Bad Writing... Good writing welcomes readers
> in. Its meaning is available on first reading because its sentences proceed in a
> logical order, its words carry their ordinary and intended sense, and its structure
> helps readers see where they're going. Accessibility doesn't mean oversimplification
> or a ban on technical terms." (LawProse Lesson #496, August 28, 2026)

### Evidential status
**CRAFT.** No study is cited; this is Garner's own professional judgment, stated with
his own authority as a usage/style authority, not backed by a measurement.

### B2.2 Richard Wydick, *Plain English for Lawyers* — GAP, could not verify directly

I could **not** fetch Wydick's original 1978 *California Law Review* article ("Plain
English for Lawyers," 66 Calif. L. Rev. 727) or the book. `scholarship.law.berkeley.edu`
returned 404 for every URL guessed; `jstor.org` blocked with a client-side challenge
page; general web search was unavailable (no Serper key configured, and DuckDuckGo/
Bing/Mojeek all returned CAPTCHA challenge pages to this fetcher, not results).

The only thing I could verify about Wydick from a live, reputable-ish source is a
single unsourced-to-page-number quote attributed to him on Wikipedia's "Plain language"
article (`en.wikipedia.org/wiki/Plain_language`, fetched 2026-09-08, **SECONDARY**):

> "Language that is clear, concise and correct." (Richard Wydick, as quoted, no page
> cited, in the "Definitions" section of Wikipedia's Plain language article)

**This is too thin to be useful as source material.** I am flagging Wydick as an
**honest gap**: I know the book's bibliographic identity (Wydick, R. C., *Plain English
for Lawyers*, Carolina Academic Press, multiple editions since 1979) but I did not read
it, could not find a reliable secondary summary of its actual content (e.g., its famous
"ten rules"), and am not willing to reconstruct its list of rules from memory, per rule 6.

### B2.3 Joseph Kimble, *Writing for Dollars, Writing to Please* (Carolina Academic
Press, 2012) — GAP, could not verify directly

I could **not** fetch this book, and could not locate a working, freely accessible
secondary description of its compiled empirical studies. Attempts included: Carolina
Academic Press's own book page for the ISBN (dead link, redirected to a generic search
page with no result); Michigan Bar Journal "Plain Language" column pages (site loads
navigation chrome only — article body appears to be loaded client-side via JavaScript
that this fetcher could not execute); WMU-Cooley Law School's institutional repository
domains did not resolve via DNS from this environment; SSRN and general web search were
blocked (403 / CAPTCHA respectively).

**This is the single most important gap in this cluster relative to what was asked
for**, because the brief specifically wanted Kimble's compiled empirical results. I am
stating plainly: **I could not verify any specific measured result from Kimble's book.**
I know its bibliographic identity and its reputation (it is widely cited as compiling
comprehension/preference/time studies comparing plain-language and legalese documents),
but I have not read a single one of those studies through this book or a reliable
summary of it, and I am not willing to state specific numbers or study designs from
memory. Do not treat any specific "Kimble found X%" claim as verified unless it is
re-sourced.

---

## B3. Journalism: the inverted pyramid and the nut graf

### B3.1 The inverted pyramid — history, from Poynter (Chip Scanlan)

**PRIMARY.** Chip Scanlan, "Birth of the Inverted Pyramid: A Child of Technology,
Commerce and History," Poynter Institute, published June 20, 2003 (updated Nov. 25,
2014). The live Poynter URL for this piece 404s today; I fetched the Wayback Machine's
capture instead: `https://web.archive.org/web/20151212223141/http://www.poynter.org/news/media-innovation/12755/birth-of-the-inverted-pyramid-a-child-of-technology-commerce-and-history/`
(fetched 2026-09-08; capture timestamped 2015-12-12; original article, excerpted from
Scanlan's own book *Reporting and Writing: Basics for the 21st Century*, Oxford
University Press).

> "That all changed with worldwide adoption of the telegraph, invented in 1845 by a
> portrait painter named Samuel Morse. A new and radically different story form dubbed
> 'the inverted pyramid' emerged, a product of new technology and a changing
> intellectual environment... That economic pressure more than anything else influenced
> a new kind of writing that departed from the flowery language of the 19th century —
> it was concise, stripped of opinion and detail."

> "A popular myth about the inverted pyramid holds that it came about during the
> American Civil War... The problem with that myth is that researchers who have studied
> leading American papers in the Civil War find numerous examples of stories written in
> the chronological style of the day rather than the 'first news first' style of the
> inverted pyramid. It came later than that, and a young journalism historian named
> David T. Z. Mindich makes a persuasive case that 'the inverted pyramid was born with
> the coverage of Lincoln's death.'"

This is useful precisely because it **debunks** the standard telegraph-cutoff origin
myth using historical research (Mindich), while still being a craft essay itself, not a
study.

### B3.2 The inverted pyramid — applied to hypertext/web writing, Jakob Nielsen

**PRIMARY**, still live: Jakob Nielsen, "Inverted Pyramids in Cyberspace," Nielsen
Norman Group, May 31, 1996. `https://www.nngroup.com/articles/inverted-pyramids-in-cyberspace/`
(fetched 2026-09-08).

> "Journalists have long adhered to the inverse approach: start the article by telling
> the reader the conclusion... follow by the most important supporting information, and
> end by giving the background. This style is known as the inverted pyramid for the
> simple reason that it turns the traditional pyramid style around. Inverted-pyramid
> writing is useful for newspapers because readers can stop at any time and will still
> get the most important parts of the article."

> "On the Web, the inverted pyramid becomes even more important since we know from
> several user studies that users don't scroll, so they will very frequently be left to
> read only the top part of an article."

Nielsen cites "several user studies" for the scrolling claim but the specific studies
are not named/linked in the text I extracted (a footnote marker "(*)" is present in the
HTML but the referenced note was not captured in my extraction). **I have not verified
the underlying scrolling studies** — treat that sentence as EMPIRICAL-by-assertion, not
independently confirmed by me.

### B3.3 The nut graf — American Press Institute craft essay (archived)

**Access note**: The live americanpressinstitute.org site no longer hosts this page; I
retrieved it via the Wayback Machine CDX index and a snapshot:
`https://web.archive.org/web/2015id_/http://americanpressinstitute.org/roundtable/nutgrafs/`
(fetched 2026-09-08). Title: "Sometimes You (Should) Feel Like a Nut," dated April 25,
2003, "first printed in *Media*, the magazine of the Canadian Association of
Journalists." **The author's byline is present in the page markup as an empty link**
(`<a href="...author_archive.cfm#"></a>`), so I could not recover the author's name from
this capture — treat authorship as unverified. I am marking this **SECONDARY-leaning**
because of that missing attribution, even though the text itself reads as a first-person
craft essay (i.e., it may function as primary craft advice, but I cannot name its
source person).

> "It's the Why Does It Matter that gives meaning to the Who, What, Where, When, Why
> and How. Nuts can come dressed as single phrases, explainer sentences, single
> hard-working grafs or whole sections... At its simplest, a nut graf is a segue between
> the lede and the body of the story; it summarizes the significance, background and
> projected impact of the news."

> "Other magic to a nut graf is a selfish one: It provides you, the struggling writer,
> with a roadmap... In those stories (think Wall Street Journal), the nut should
> foreshadow all the conflicts inherent in your subject and all the key issues necessary
> to explaining the subject (think Cliff Notes)."

### B3.4 Encyclopedic cross-check: Wikipedia, "Inverted pyramid (journalism)"

**SECONDARY**, used only to confirm the naming and to surface the Mindich/Scanlan/
Pöttker citation trail, not quoted for substantive claims beyond what I verified in the
primary Scanlan piece above. `https://en.wikipedia.org/wiki/Inverted_pyramid_(journalism)`
(fetched 2026-09-08).

### Evidential status for B3
**CRAFT**, throughout. The inverted pyramid's *origin story* is treated historically
(Scanlan cites historian Mindich's argument, and there is an academic literature —
Pöttker 2003, Canavilhas 2007 — that studies its emergence and evolution, none of which
I read directly). Its *effectiveness as a device* (readers can stop anywhere and still
get the gist; important material survives editing cuts) is asserted as professional
lore/design rationale in every source I read, not measured in any of them. Nielsen's
"users don't scroll" is the one EMPIRICAL-flavored claim, and I could not verify its
underlying studies. The nut graf material is pure craft advice with no measurement
claimed anywhere.

---

## MY CONCLUSIONS (mine, not sourced)

Separated per rule 1 — everything below is my synthesis, not a source's claim.

1. **Most directly transferable to a writing standard, ranked:**
   - **Centering theory's Rule 1** (pronoun licensed only if the backward-looking
     center is also pronominalized) is the most precise, mechanically checkable rule in
     this whole cluster. It could become a concrete, testable style rule: "if you
     pronoun something else in this sentence, the thing your reader is currently
     tracking as the topic must also be a pronoun; otherwise use a full noun phrase."
   - **Halliday & Hasan's five-way taxonomy** (reference / substitution / ellipsis /
     conjunction / lexical cohesion) is the right vocabulary for auditing *why* two
     sentences feel connected or disconnected — useful as a checklist during editing,
     not as a generative rule.
   - **The EU booklet's Hint 6/7 (de-nominalize, name the agent)** and
     **plainlanguage.gov's "hidden verbs"** section are the same craft device
     independently arrived at by two institutions — that convergence is itself
     evidence the device is a stable, teachable unit, even though neither institution
     cites a study for it.
   - **Redundancy vs. split-attention** (from the Mayer/Sweller literature) is a useful
     precise distinction for a writing standard concerned with diagrams/tables/text
     combinations: don't just say "avoid redundancy" — ask whether the two channels are
     each independently intelligible (if not, you have a split-attention problem, fixed
     by integration, not deletion) or whether one channel is simply repeating the other
     (a true redundancy problem, fixed by deletion).
   - **The inverted pyramid / nut graf** pairing is the right model for "decision-first"
     document structure (put the conclusion first, then support it) — but note it is
     craft, unmeasured, and its own history is one of debunked myth-making (the
     telegraph story), which is itself a useful cautionary example for how badly
     "everyone knows why" explanations can survive without evidence.

2. **Where evidence is genuinely thin and should not be oversold in the standard:**
   - Cognitive load theory's specific *named effects* (redundancy, split-attention,
     modality, worked-example, signaling) are each backed by particular experiments I
     did not read directly — the standard should cite them as "empirically studied
     effects, see X for the original studies," not as settled facts I re-derived.
   - "Germane load" appears to be *contested even by its originator's own recent work*
     (absent from the 2026 Sweller-coauthored review) — do not build a rule on germane
     load as if it were stable theory.
   - Nielsen's "users don't scroll" and plainlanguage.gov's "research shows" sentence
     are both unverified-by-me empirical-flavored claims; useful as motivation, not as
     citable evidence.

3. **Gaps that should be closed before finalizing a standard, if these sources matter:**
   - Wydick's actual "ten rules" (or however his book is organized) — not verified here.
   - Kimble's compiled empirical studies in *Writing for Dollars, Writing to Please* —
     not verified here at all; this is the biggest hole in the "law" third of this
     cluster, and it was explicitly the part the brief said would be "the most valuable
     thing in this cluster" if found. It was not found.
   - Sweller's original 1988 paper — not read directly; only reachable today through a
     paywalled DOI and a co-authored 2026 restatement.

---

## Fetch log (all URLs actually retrieved, with outcome)

- `https://aclanthology.org/J95-2003.pdf` — 200, PRIMARY, Grosz/Joshi/Weinstein 1995.
- `https://archive.org/download/139618384-halliday-hasan-cohesion-in-english/139618384-Halliday-Hasan-Cohesion-in-English_djvu.txt` — 200, PRIMARY (OCR scan), Halliday & Hasan 1976.
- `https://archive.org/metadata/cohesioninenglis0000hall` — confirmed access-restricted, could not use.
- `https://digital.gov/guides/plain-language`, `/principles`, `/writing` — 200, PRIMARY, current US federal plain-language guidance (successor to plainlanguage.gov).
- `https://op.europa.eu/o/opportal-service/download-handler?identifier=725b7eb0-d92e-11e5-8fea-01aa75ed71a1&format=pdf&language=en` — 200, PRIMARY, EU "How to write clearly."
- `https://www.ebi.ac.uk/europepmc/webservices/rest/PMC13219774/fullTextXML` — 200, PRIMARY, Darejeh/Marcus/Mohammadi/Sweller 2026, Human Factors.
- `https://link.springer.com/article/10.1023/A:1022193728205` — 200, PRIMARY abstract only, Sweller/van Merriënboer/Paas 1998.
- `https://www.ebi.ac.uk/europepmc/webservices/rest/PMC10192876/fullTextXML` — 200, SECONDARY literature review, Trypke/Stebner/Wirth 2023.
- `https://en.wikipedia.org/wiki/Cognitive_load` — 200, SECONDARY, flagged partially unreliable (LLM-content warning banner on page).
- `https://en.wikipedia.org/wiki/Cohesion_(linguistics)` — 200, SECONDARY cross-check only.
- `https://en.wikipedia.org/wiki/Plain_language` — 200, SECONDARY, thin Wydick quote only.
- `https://en.wikipedia.org/wiki/Inverted_pyramid_(journalism)` — 200, SECONDARY cross-check only.
- `http://lawprose.org/lawprose-lessons/` — 200, PRIMARY, Garner's own site (note: `www.` subdomain failed DNS).
- `https://web.archive.org/web/20151212223141/http://www.poynter.org/.../birth-of-the-inverted-pyramid.../` — 200, PRIMARY, Scanlan/Poynter 2003.
- `https://www.nngroup.com/articles/inverted-pyramids-in-cyberspace/` — 200, PRIMARY, Nielsen 1996.
- `https://web.archive.org/web/2015id_/http://americanpressinstitute.org/roundtable/nutgrafs/` — 200, byline unrecoverable, "Sometimes You (Should) Feel Like a Nut," 2003.

### Attempted but failed (for transparency)
- Sweller (1988) *Cognitive Science* original paper: 404/DNS-failure/403 on every mirror tried.
- Wydick, *Plain English for Lawyers* (1978 article and book): 404 (Berkeley repository), client-challenge block (JSTOR).
- Kimble, *Writing for Dollars, Writing to Please*: dead publisher link, JS-rendered Michigan Bar Journal pages, unresolvable Cooley Law School repository domains.
- General web search (websearch skill, Google, Bing, DuckDuckGo, Mojeek): all either lacked API keys or returned bot-challenge pages, not results.
