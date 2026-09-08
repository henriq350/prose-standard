# Document Architecture: How Established Systems Split Document Types

Research cluster for a writing-standard design question: **should a writing standard be ONE document,
or a base plus variants per output type — and if variants, split along which axis?**

Fetch date for all live URLs in this file: **2026-09-08** (fetched directly via HTTP in this session,
`httpx` with a standard User-Agent; no JS rendering, no search API — Serper/websearch was unavailable
and unused per task constraints).

---

## 1. Diátaxis (Daniele Procida) — https://diataxis.fr

**Status: PRIMARY.** Read directly from the live site. Pages fetched and quoted below:
`/`, `/foundations/`, `/map/`, `/compass/`, `/start-here/`, `/application/`, `/tutorials-how-to/`,
`/reference-explanation/`, `/how-to-guides/`, `/explanation/`, `/colophon/`.
Author: Daniele Procida (stated on `/colophon/`); copyright line on every page reads
"Copyright © Daniele Procida."

### 1.1 What the source says

**The two axes it splits on.** Diátaxis derives its four document types from two orthogonal
dimensions of "craft," not from four arbitrarily-observed genres.

> "A skill or craft or practice contains both **action** (practical knowledge, knowing **how**, what we
> do) and **cognition** (theoretical knowledge, knowing **that**, what we think). The two are completely
> bound up with each other, but they are counterparts, wholly distinct from each other, two different
> aspects of the same thing."
> — https://diataxis.fr/foundations/ ("Action/cognition"), fetched 2026-09-08

> "Similarly, the relationship of a practitioner with their practice is that it is something that needs
> to be both **acquired**, and **applied**. Being 'at work' (concerned with applying the skill and
> knowledge of their craft) and being 'at study' (concerned with acquiring them) are once again
> counterparts, distinct but bound up with each other."
> — https://diataxis.fr/foundations/ ("Acquisition/application"), fetched 2026-09-08

Procida is explicit that this yields a **complete, closed map** — not a list that could arbitrarily
contain three or five items:

> "This gives us two dimensions of skill, that we can lay out on a map — a map of the territory of
> craft: This is a **complete** map. There are only two dimensions, and they don't just cover the
> entire territory, they define it. This is why there are necessarily four quarters to it, and there
> could not be three, or five. It is not an arbitrary number."
> — https://diataxis.fr/foundations/ ("The map of the territory"), fetched 2026-09-08

The mapping of needs to document types, given verbatim as a table on `/foundations/`:

| need | addressed in | the user | the documentation |
|---|---|---|---|
| learning | tutorials | acquires their craft | informs action |
| goals | how-to guides | applies their craft | informs action |
| information | reference | applies their craft | informs cognition |
| understanding | explanation | acquires their craft | informs cognition |

**The compass — the exact decision procedure.** This is the tool the site itself provides for
classifying a piece of content or a user need. Given verbatim from `/compass/`:

> "If the content… …and serves the user's… …then it must belong to…
> informs action / acquisition of skill / a tutorial
> informs action / application of skill / a how-to guide
> informs cognition / application of skill / reference
> informs cognition / acquisition of skill / explanation"
> — https://diataxis.fr/compass/, fetched 2026-09-08

> "To use the compass, just two questions need to be asked: **action or cognition?** **acquisition or
> application?** And it yields the answer."
> — https://diataxis.fr/compass/, fetched 2026-09-08

**Why the four types must not be mixed.** The site names this "blur," and treats it as the central
failure mode of documentation architecture:

> "However, there is a kind of natural affinity between each of the different forms of documentation and
> its neighbours on the map, and a natural tendency to **blur** the distinctions... When these
> distinctions are allowed to blur, the different kinds of documentation bleed into each other. Writing
> style and content make their way into inappropriate places. It also causes structural problems, which
> make it even more difficult to maintain the discipline of appropriate writing. In the worst case there
> is a complete or partial collapse of tutorials and how-to guides into each other, making it impossible
> to meet the needs served by either."
> — https://diataxis.fr/map/ ("Blur"), fetched 2026-09-08

> "Crossing or blurring the boundaries described in the map is at the heart of a vast number of problems
> in documentation."
> — https://diataxis.fr/start-here/, fetched 2026-09-08

The site names the specific "natural affinity" pairs that tend to blur together (tutorials↔how-to
guides via shared *guide-action*; how-to↔reference via shared *propositional knowledge in use*;
reference↔explanation via shared *cognition*; explanation↔tutorials via shared *acquisition*) — i.e.
the blur risk runs along the map's own edges, not randomly.

**What it says about a document serving two purposes at once — a per-document-type statement, not a
general one.** Diátaxis does not have one abstract rule "a document may not serve two purposes"; it
states the prohibition separately for each adjacent pair, in the strongest terms for
how-to-guides/reference:

> "How-to characteristics: focused on tasks or problems / assume the user knows what they want to
> achieve / **action and only action** / **no digression, explanation, teaching**. Anything else that's
> added distracts both you and the user and dilutes the useful power of the guide. Typically, the
> temptations are to explain or to provide reference for completeness. Neither of these are part of
> guiding the user in their work. **They get in the way of the action; if they're important, link to
> them.**"
> — https://diataxis.fr/how-to-guides/ ("Key principles"), fetched 2026-09-08

> "**Describe and only describe**... You will certainly not expect to find for example recipes or
> marketing claims mixed up with this information; that could be literally dangerous. The way reference
> material is presented on food products is so important that it's usually governed by law, and the same
> kind of seriousness should apply to all reference documentation."
> — https://diataxis.fr/reference/, fetched 2026-09-08

> "It usually happens while writing reference material that starts to become expansive... As a result
> one often finds explanatory material sprinkled into reference. **This is bad for the reference,
> interrupted and obscured by digressions. But it's bad for the explanation too, because it's not
> allowed to develop appropriately and do its own work.**"
> — https://diataxis.fr/reference-explanation/, fetched 2026-09-08

> "Often, writers of tutorials who are anxious that their students should **know** things overload their
> tutorials with distracting and unhelpful explanation. It would be much more useful to give the learner
> the most minimal explanation ('Here, we use HTTPS because it's safer') and then link to an in-depth
> article... for when the user is ready for it."
> — https://diataxis.fr/start-here/, fetched 2026-09-08

The general principle underlying all four specific statements is stated once, directly, on `/foundations/`
and `/map/`: each document type has "**one particular job to do**," and that job is "**clearly
distinguished from and contrasted with the other functions of documentation**" (`/map/`, "Expectations
and guidance"). Diátaxis's answer to "can a document serve two purposes" is therefore: no — not because
of a single abstract rule, but because each type sits at a distinct, named coordinate on the
action/cognition × acquisition/application map, and importing the concerns of a neighbouring quadrant
degrades both.

**Named contrast table (verbatim, `/map/`):**

| | Tutorials | How-to guides | Reference | Explanation |
|---|---|---|---|---|
| what they do | introduce, educate, lead | guide | state, describe, inform | explain, clarify, discuss |
| answers the question | "Can you teach me to…?" | "How do I…?" | "What is…?" | "Why…?" |
| oriented to | learning | goals | information | understanding |
| purpose | to provide a learning experience | to help achieve a particular goal | to describe the machinery | to illuminate a topic |
| form | a lesson | a series of steps | dry description | discursive explanation |
| analogy | teaching a child how to cook | a recipe in a cookery book | information on the back of a food packet | an article on culinary social history |

**Tutorial vs. how-to guide — the specific distinction (the most commonly conflated pair, per the
source):**

> "In Diátaxis, tutorials and how-to guides are strongly distinguished. It's a distinction that's often
> not made; in fact **the single most common conflation made in software product documentation is that
> between the tutorial and the how-to guide.**"
> — https://diataxis.fr/tutorials-how-to/, fetched 2026-09-08

> "A tutorial serves the needs of the user who is **at study**. Its obligation is to provide a
> successful learning experience. A how-to guide serves the needs of the user who is **at work**. Its
> obligation is to help the user accomplish a task... tutorials are **learning-oriented**, and how-to
> guides are **task-oriented**."
> — https://diataxis.fr/tutorials-how-to/, fetched 2026-09-08

**Reference vs. explanation — the specific distinction, plus a stated intuition heuristic (explicitly
flagged by the source as unreliable on its own):**

> "If it's boring and unmemorable it's probably **reference**... if you can imagine reading something in
> the bath, probably, it's **explanation**... But only *mostly* — because it's also quite easy to slip
> between one form and the other."
> — https://diataxis.fr/reference-explanation/, fetched 2026-09-08

**Functional quality vs. deep quality** (a secondary but relevant Diátaxis concept about what
"quality" means once architecture is fixed):

> "We need documentation to meet standards of accuracy, completeness, consistency, usefulness, precision
> and so on. We can call these aspects of its **functional quality**... There are other characteristics,
> that we can call **deep quality**... feeling good to use, having flow, fitting to human needs, being
> beautiful, anticipating the user... deep quality is **conditional upon** functional quality."
> — https://diataxis.fr/quality/, fetched 2026-09-08

**On the cyclical, non-mixing relationship between the four types in actual use:**

> "learning-oriented phase: We begin by learning... goal-oriented phase: Next we want to put the skill
> to work. information-oriented phase: As soon as our work calls upon knowledge that we don't already
> have in our head, it requires us to consult technical reference. explanation-oriented phase: Finally,
> away from the work, we reflect on our practice and knowledge to understand the whole."
> — https://diataxis.fr/map/ ("The journey around the map"), fetched 2026-09-08

### 1.2 My conclusions (not the source's words)

- Diátaxis is a **THEORETICAL** framework: its four-type claim is derived deductively from a
  2×2 model of "craft" (action/cognition × acquisition/application), not from a study measuring
  documentation outcomes. The site itself frames this as the difference between "it seems to work" and
  a "theory of documentation" that explains *why* — i.e. it is explicitly offered as theory, not as an
  empirical result (`/foundations/`).
- The "must not mix" claim is best read as **CRAFT** advice with a theoretical justification bolted on:
  no experiment is cited anywhere on the site; the argument for non-mixing is that each quadrant serves
  a distinct, incompatible user state (at work vs. at study; wants to act vs. wants to think), so mixing
  necessarily under-serves at least one state. This is asserted, not measured.
  Testimonials on the homepage (Vonage, Cloudflare, Gatsby employees) are anecdotal endorsements, not
  studies.
- For a writing standard, the directly transferable idea is not "four document types" per se but the
  **general mechanism**: pick the axes that define your users' needs, verify the resulting quadrants are
  jointly exhaustive and mutually exclusive, then forbid a document from serving two quadrants at once —
  and state the forbidden mixing case-by-case (tutorial-explanation, how-to-reference, etc.) rather than
  as one abstract rule, because the failure mode differs by pair.
- Diátaxis splits by **reader stance** (what the reader needs at the moment of reading), not by output
  medium, audience seniority, or component/topic. That is a specific, falsifiable design choice worth
  contrasting with Google's and Microsoft's choice (below), which split mostly by **linguistic/formatting
  facet**, with only narrow carve-outs by content type.

---

## 2. Google developer documentation style guide — developers.google.com/style

**Status: PRIMARY.** Read directly. Pages fetched: `/style` (full nav + "About this guide"),
`/style/philosophy`, `/style/reference-verbs` (called "Verb forms in reference documentation" — the
page title differs from the nav label "Verbs in reference documents").

### 2.1 What the source says

**Organisation.** The guide is a single, flat reference document, not a base-plus-per-type-variant
structure. Its own top-level table of contents (fetched from `/style`) is organised by **linguistic and
production facet**, not by document type:

> Guides: Introduction · General principles · Language and grammar · Punctuation · Formatting and
> organization · Linking · Computer interfaces · HTML and CSS · Names and naming
> — https://developers.google.com/style, nav sidebar, fetched 2026-09-08

There is **no** "Tutorials" / "How-to" / "Reference" / "Explanation" split anywhere in the navigation.
The closest thing to a document-type carve-out is two narrow pages nested inside "Language and grammar"
and "Computer interfaces": "Verb forms in reference documentation" and the "API reference code
comments" / "Command-line syntax" pages under "Computer interfaces" — i.e. small, topic-scoped
exceptions inside an otherwise flat structure, not a parallel document-type track.

**How it says to use itself** (a reference-lookup document, read non-linearly):

> "If you're new to the guide and looking for introductory topics about our style, then start with
> Highlights, Voice and tone, and Text-formatting summary. Otherwise, **use the guide as a reference
> document for specific questions.** For example, you can look up terms in the word list."
> — https://developers.google.com/style ("About this guide"), fetched 2026-09-08

**Explicit statement that it is NOT a general writing standard, and its place in a reference
hierarchy** (this is Google's own document-architecture policy — a stack of increasingly general
sources, consulted in order):

> "Use the following references, including this guide, in this order: 1. Project-specific style... 2.
> This style guide... 3. Third-party references... [table:] Spelling → Merriam-Webster.com. Nontechnical
> style → The Chicago Manual of Style, 17th edition. Technical style → the Microsoft Writing Style
> Guide."
> — https://developers.google.com/style, fetched 2026-09-08

**Explicit statement of intent — a house style, not an industry standard, and not a complete writing
guide:**

> "This guide is **not** intended to do the following: Provide an industry documentation standard.
> Compete with other well-known style guides. Replace another style guide that you already follow.
> **Provide a complete set of basic writing guidelines.** Provide legal advice."
> — https://developers.google.com/style/philosophy, fetched 2026-09-08

**Stated reason for withholding rationale** (relevant to whether a standard should explain its own
rules):

> "We generally don't explain the reasoning behind most of our guidelines... Too much explanation can
> clutter up a page. Readers most often want a brief answer to a specific question, rather than a
> detailed explanation."
> — https://developers.google.com/style/philosophy, fetched 2026-09-08

**The one place Google does split guidance explicitly by document type — reference documentation gets
its own grammatical rule** (a narrow, concrete example of type-specific style, not a whole parallel
document):

> "When you're writing reference documentation for a method, phrase the main method description in
> terms of what the method does (**gets, lists, creates, searches**), rather than what the developer
> would use it to do (get, list, create, search)... Recommended: `tasks.insert: Creates a new task on
> the specified task list.` Not recommended: `tasks.insert: Create a new task on the specified task
> list.`"
> — https://developers.google.com/style/reference-verbs, fetched 2026-09-08

### 2.2 My conclusions (not the source's words)

- Google's architecture is **one document**, organised by **grammar/formatting facet** (verbs,
  punctuation, headings, lists…), with a small number of **type-scoped exceptions** embedded inline
  where a facet behaves differently for one content type (reference-doc verb tense is the clearest
  example found). This is architecturally the opposite choice from Diátaxis: Diátaxis splits by reader
  need first and lets style follow; Google splits by linguistic facet first and calls out document-type
  exceptions only where they demonstrably diverge.
- Google explicitly declines to be a complete writing standard and defers upward (to Chicago) and
  sideways (to Microsoft, for "technical style") — i.e. its own documented architecture is a **stack of
  guides consulted by precedence**, not a single-document standard covering everything. That
  three-tier precedence model (project-specific → house style → third-party) is itself a reusable
  architecture pattern, independent of the tutorial/how-to/reference/explanation question.
- Every claim in this section is **CRAFT**: no study or measurement is cited anywhere in the fetched
  pages for why the guide is organised this way; it states preferences and policy, not evidence.

---

## 3. Microsoft Writing Style Guide — learn.microsoft.com/en-us/style-guide

**Status: PRIMARY.** Read directly, including the full table of contents pulled from the site's own
`toc.json` (`https://learn.microsoft.com/en-us/style-guide/toc.json`, fetched 2026-09-08), plus the
`Welcome`, `Developer content`, and `Developer content → Reference documentation` pages.

### 3.1 What the source says

**Overall organisation**, from the TOC (structure, not prose commentary — this is the site's actual
navigation tree, reproduced faithfully):

> Microsoft Writing Style Guide → Welcome · Brand voice · Top 10 tips · **Checklists** (acronyms,
> capitalization, grammar, numbers, **procedures and instructions**, punctuation, responsive content,
> text-formatting, word choice) · A–Z word list · Accessibility guidelines · Acronyms · Bias-free
> communication · Capitalization · **Chatbots and virtual agents** (own subsection: structural/technical
> considerations, writing for bots, care and feeding of the bot) · Content planning · Design planning ·
> **Developer content** (own subsection: Reference documentation, Code examples, Formatting developer
> text elements) · Final publishing review · Global communications · Grammar and parts of speech ·
> Numbers · **Procedures and instructions** (own subsection: writing step-by-step instructions,
> describing UI interactions, alternative input methods, formatting text in instructions) ·
> Punctuation · Responsive content · Scannable content · Search and writing · Text formatting…
> — https://learn.microsoft.com/en-us/style-guide/toc.json, fetched 2026-09-08

This shows Microsoft's architecture is, like Google's, primarily a **single flat guide organised by
linguistic/production facet** (grammar, punctuation, text formatting, capitalization…), but with
**three explicit content-type subsections carved out as their own branches**: "Developer content,"
"Chatbots and virtual agents," and "Procedures and instructions." These are not full parallel document
types in the Diátaxis sense (there is no separate "explanation-type" or "conceptual-content" branch);
they are channel/genre-specific exceptions layered onto the flat facet-based structure.

**Microsoft's own stated two-type split inside developer content** — this is the most direct
"per-output-type" statement found on the site, and it names exactly two types, not four:

> "Two types of content form the foundation of developer documentation: **reference documentation and
> code examples**. Reference documentation provides an encyclopedia of all the programming elements,
> such as classes, methods, and properties, that are available for writing applications. Code examples
> show how to use those elements. This section provides guidelines for creating: Reference documentation
> · Code examples. It also has guidelines for formatting developer text elements."
> — https://learn.microsoft.com/en-us/style-guide/developer-content/, fetched 2026-09-08

**What Microsoft prescribes specifically for reference documentation** (structural template, given as a
table on the page — reproduced verbatim, condensed):

> "Reference documentation provides details about the programming elements associated with technologies
> and languages... **Consistency is essential in reference documentation. A standard article design,
> predictable headings and structure, and consistent wording help developers find what they need
> quickly.**"
> — https://learn.microsoft.com/en-us/style-guide/developer-content/reference-documentation, fetched 2026-09-08

> "Article titles: Use the name of a programming element (such as Clear), followed by an element type
> (such as Class, Method, Property, or Event)... Elements of a reference article: Title and description,
> Declaration/syntax, Parameters, Return value, Remarks..."
> — same page, fetched 2026-09-08

**Overall self-description** — a house voice/terminology guide "for all communication," not
segmented by output type at the top level:

> "Welcome to the Microsoft Writing Style Guide, your guide to writing style and terminology for **all
> communication—whether an app, a website, or a white paper**. If you write about computer technology,
> this guide is for you... Here's what you will find: Top 10 tips for mastering Microsoft style and
> voice, Bias-free communication, Global communications."
> — https://learn.microsoft.com/en-us/style-guide/welcome/, fetched 2026-09-08

### 3.2 My conclusions (not the source's words)

- Microsoft's architecture answers the "one document vs. variants" question with: **mostly one document,
  organised by linguistic facet, plus a small number of named exceptions for content types whose
  conventions genuinely diverge** (developer reference docs, chatbot/conversational UI copy, and
  step-by-step procedures). This is a third pattern, distinct from both Diátaxis (four fully parallel
  reader-need-based document types) and from a naive single monolithic document: it is **base + narrow
  variant sections**, where a variant section exists only when the facet-level guidance would otherwise
  be wrong for that content type (e.g., a reference article needs a fixed schema of parts; a chatbot
  needs turn-taking rules; a procedure needs numbered-step conventions) — not because someone decided in
  advance that four/five types must each get equal treatment.
- Microsoft's own explicit two-type split ("reference documentation and code examples" as "the
  foundation of developer documentation") is narrower than Diátaxis's four types and is scoped only to
  developer content, not to the whole style guide. It should not be read as Microsoft endorsing a
  four-type or Diátaxis-style split generally.
- As with Google, every claim here is **CRAFT**: house style asserted for internal consistency and
  discoverability ("help developers find what they need quickly"), no study cited.

---

## 4. John Carroll's minimalism / "The Nurnberg Funnel"

**Status: SECONDARY throughout.** The book itself (Carroll, John M. *The Nurnberg Funnel: Designing
Minimalist Instruction for Practical Computer Skill*. MIT Press, 1990. ISBN 9780262031639 hardcover /
9780262531116 paperback) and its sequel (Carroll, John M., ed. *Minimalism Beyond the Nurnberg Funnel*.
MIT Press, 1998. ISBN 026203249X) were **not fetchable in full**. The 1998 book is on the Internet
Archive (`archive.org/details/minimalismbyond0000unse`) but is access-restricted controlled digital
lending (`"access-restricted-item": "true"`, confirmed via the Archive's own metadata API,
`archive.org/metadata/minimalismbeyond0000unse`, fetched 2026-09-08); its full-text-search endpoint
returned "Item not available" for this session. MIT Press's own catalogue page for *The Nurnberg Funnel*
(`mitpress.mit.edu/9780262031639/`) blocks direct bot fetches (HTTP 403) but was recovered via the
Wayback Machine capture below. IEEE Xplore, Wiley Online Library, and MIT Press Direct (for the DOI
`10.7551/mitpress/4616.003.0003`, the van der Meij & Carroll chapter) all blocked automated fetches
(403 or a JS robot check). Google Books API and Semantic Scholar API were rate-limited (429) in this
session and returned no data.

What follows is built from three sources I *could* fetch and verify:

1. **MIT Press's own catalogue description** of *The Nurnberg Funnel*, recovered via Wayback Machine —
   publisher-authored, so still secondary to Carroll's text but authoritative for what the book claims
   about itself.
2. **Wikipedia**, "Minimalism (technical communication)" — flagged by Wikipedia's own maintenance
   banners as written "like a personal reflection... states a Wikipedia editor's personal feelings," and
   as lacking sufficient inline citations (both banners present as of the fetched revision). Treated
   here with that caveat; used only for claims that are also corroborated elsewhere or are clearly
   sourced to a named citation in its reference list.
3. **Virtaluoto, Jenni; Suojanen, Tytti; Isohella, Suvi. "Minimalism Heuristics Revisited: Developing a
   Practical Review Tool." *Technical Communication* (journal of the Society for Technical
   Communication), issue 68.1, February 2021.** This is a peer-reviewed journal article that
   itself directly summarizes and quotes van der Meij & Carroll's canonical formulation. Fetched via a
   Wayback Machine capture of `stc.org/techcomm/...` dated 2024-04-17 (the live STC site was down /
   "under update" when checked directly on 2026-09-08). This is the strongest available secondary
   source for the *specific content* of Carroll's minimalism principles, because it quotes and cites the
   primary methodological paper (van der Meij, H., & Carroll, J. M. (1995). "Principles and heuristics
   for designing minimalist instruction." *Technical Communication*, 42(2), 243–261) directly, rather
   than paraphrasing from memory.

### 4.1 What the sources say

**Publisher description of the book's central claim** (MIT Press catalogue page, via Wayback Machine
capture 20231217154120):

> "How do people acquire beginning competence at using new technology? The legendary Funnel of Nurnberg
> was said to make people wise very quickly when the right knowledge was poured in; it is an approach
> that designers continue to apply in trying to make instruction more efficient. This book describes a
> quite different instructional paradigm that uses what learners do spontaneously to find meaning in the
> activities of learning. It presents the 'minimalist' approach to instructional design — its origins in
> the study of people's learning problems with computer systems, its foundations in the psychology of
> learning and problem solving, and its application in a variety of case studies. Carroll demonstrates
> that the minimalist approach outperforms the standard 'systems approach' in every relevant way — the
> learner, not the system, determines the model and the methods of instruction. It supports the rapid
> achievement of realistic projects right from the start of training, instead of relying on drill and
> practice techniques, and designing for error recognition and recovery as basic instructional events,
> instead of seeing error as failure."
> — https://web.archive.org/web/20231217154120/https://mitpress.mit.edu/9780262031639/the-nurnberg-funnel/,
> captured 2023-12-17, retrieved via Wayback Machine 2026-09-08. Author bio on the same page: "John M.
> Carroll... Manager of User Interface Theory and Design at IBM's Watson Research Center" (at time of
> the book) / "a professor in the School of Information Sciences and Technology at Penn State
> University" (current, per page).

**The canonical four principles**, as reported (and directly quoted/summarised) by Virtaluoto,
Suojanen & Isohella (2021), citing van der Meij & Carroll (1995) — this is the load-bearing passage for
the "impatient, goal-directed reader" claim requested:

> "The central design elements of minimalism are captured in its four principles presented by van der
> Meij and Carroll (1995), and they each include a set of heuristics (Table 1). **The first principle
> states that users should be given an immediate opportunity to act instead of giving general
> introductions; they should be encouraged to try things out on their own, and help should always be
> available. The second principle emphasizes the importance of real tasks: The product is not an end in
> itself, but the user has a real goal to achieve. According to the third principle, errors should be
> prevented by using hints, and users should be given effective error prevention information. This
> information should be provided near actions that are error-prone or when it is difficult to recover
> from the error. Information for correcting the error should be located near the actions where the
> error might occur. The fourth principle states that the documentation should be concise; not
> everything needs to be explained** (van der Meij, 1995, pp. 244–257)."
> — Virtaluoto, Suojanen & Isohella, "Minimalism Heuristics Revisited: Developing a Practical Review
> Tool," *Technical Communication* 68.1 (Feb 2021), via
> https://web.archive.org/web/20240417194857/https://www.stc.org/techcomm/2021/02/04/minimalism-heuristics-revisited-developing-a-practical-review-tool/,
> retrieved 2026-09-08. **Note on evidential status:** these are named "principles," not results; the
> article calls them "heuristics... based on solid empirical research" but the empirical support is
> cited to van der Meij & Carroll (1995) itself, which I could not fetch — I have not independently
> verified the underlying studies, only this secondary article's characterisation of them.

> "Van der Meij and Carroll (1995, p. 244) have emphasized that **neither the principles nor the
> heuristics of minimalism are rules that should be followed blindly but that they enable better
> designs.**"
> — same source, fetched 2026-09-08.

**An explicit EMPIRICAL claim, with citation, about outcomes of minimalist manuals** — this is the one
sentence in this cluster's Carroll material that names a measured effect rather than a design
prescription:

> "The benefits of minimalism seemed promising from the start: **Minimalist manuals helped users make
> fewer mistakes, complete tasks faster, and explore the software with more independence** (van der
> Meij, 1992, p. 15)."
> — Virtaluoto, Suojanen & Isohella (2021), same source as above, fetched 2026-09-08. I could not fetch
> van der Meij (1992) itself, so I cannot verify what was measured, the sample, or the effect size —
> only that this 2021 peer-reviewed article attributes this specific empirical claim to that citation.

**On the origin of "impatient, goal-directed reader" as Carroll's target user** (Wikipedia, used here
only because it is corroborated by the MIT Press description above and by the STC article's framing of
"real tasks" / "immediate opportunity to act"):

> "Minimalism is a way to make rapid achievement of realistic projects right from the start of training,
> allowing people to understand the content just from reading it once. **Minimalism strives to reduce
> interference of information delivery with the user's sense-making process.** It does not try to
> eliminate any chance of the user making a mistake, but regards an error as a teachable moment that
> content can exploit. **Minimalism is action-oriented, using as little words as possible to understand
> how to fulfill a task.**"
> — Wikipedia, "Minimalism (technical communication)," "Definition" section, fetched 2026-09-08. Marked
> SECONDARY-of-secondary and lower-confidence: this Wikipedia article carries the site's own
> "written like a personal essay" and "lacks inline citations" maintenance banners as of the fetched
> revision. The "rapid achievement of realistic projects right from the start of training" phrase is
> corroborated verbatim in the MIT Press description above, which increases confidence in that specific
> clause; the rest of the paragraph is not independently corroborated in this session.

**DITA connection** (Wikipedia, same article, citing Tony Self, "Introduction to DITA,"
oxygenxml.com — this secondary citation was independently fetched and found live, though the specific
Carroll-attribution sentence was not located verbatim on the fetched oxygenxml page in this session):

> "Darwin Information Typing Architecture (DITA) is built on Carroll's theories of Minimalism and
> [Robert E.] Horn's theories of Information Mapping."
> — Wikipedia, "Minimalism (technical communication)," "History" section, fetched 2026-09-08. **Not
> independently verified against the cited oxygenxml.com source in this session** — flagged as unverified.

### 4.2 My conclusions (not the source's words)

- Carroll's minimalism is best described as a **THEORETICAL framework with claimed empirical grounding
  that I could not independently verify**. The 2021 STC/TechComm article explicitly calls the four
  principles "heuristics... based on solid empirical research," and separately reports one specific
  empirical finding (fewer mistakes, faster task completion, more independent exploration) attributed to
  van der Meij (1992) — but neither the original 1990 book, the 1995 methodology paper, nor the 1992
  study were fetchable in this session, so I cannot confirm study design, sample size, or effect
  magnitude. Treat the "it works" claim as **CRAFT-with-cited-empirical-backing I could not check**, not
  as verified EMPIRICAL fact.
- The four principles, as reported by the 2021 secondary source, map onto a document-architecture
  question distinct from Diátaxis's: Diátaxis asks "which of four reader-need quadrants does this
  document belong to," while minimalism's four principles are about **what any single task-oriented
  document should do internally** (act immediately, anchor in a real task, front-load error recovery,
  omit what isn't needed) — i.e. minimalism is closer to a **within-type craft standard for
  action-oriented documents** than to a competing top-level split of document types. It is most directly
  comparable to Diátaxis's "how-to guide" quadrant specifically (task-oriented, at-work, action, "action
  and only action" — language that is strikingly close to Diátaxis's own "action and only action" phrase
  for how-to guides, though I found no evidence the two are historically connected; this parallel is my
  own observation, not stated by either source).
- Given the fetch failures above, anything beyond what is quoted verbatim in §4.1 (e.g. the exact
  wording of all "heuristics" under each of the four principles, case-study specifics, or the book's
  full argument) is a genuine gap in this research, not something I have reconstructed from memory.

---

## Verification gaps (explicit)

- **Full text of Carroll (1990) and van der Meij & Carroll (1995/1998)**: not fetchable (access
  restrictions / paywalls / bot blocks on archive.org lending, MIT Press Direct, IEEE Xplore, Wiley).
  Only the publisher blurb and one peer-reviewed secondary source could be verified.
- **The full "Table 1" list of sub-heuristics under each of van der Meij & Carroll's four principles**:
  referenced by the 2021 STC article but the table itself did not render as extractable text from the
  Wayback capture; only the prose summary of the four principles was recoverable.
- **The DITA↔Carroll attribution's original source (Self, "Introduction to DITA," oxygenxml.com)**: the
  page was fetched and returned HTTP 200, but I did not locate the specific attributing sentence within
  it in this session — flagged above as unverified rather than silently dropped.
- **Whether Google's or Microsoft's *internal* engineering teams maintain unpublished per-document-type
  templates beyond what's public on developers.google.com/style and learn.microsoft.com/style-guide**:
  out of scope / not fetchable — this briefing covers only the public style guides as published.
- websearch (Serper) was confirmed unavailable in this session (explicit "no Serper API key configured"
  error); all sourcing above is direct HTTP fetch plus link-following, as instructed.
