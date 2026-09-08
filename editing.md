# The editing protocol

Editing is not writing done later. It is a different activity, on a different object, with a different
risk: the writer's failure is omission, the editor's is unnecessary change.

Two conventions from professional practice shape everything below.

**A change needs a reason you can name.** From the JPL levels-of-edit system, on the Language Edit:

> "All editorial changes in a Language Edit are made on the basis of specific and identifiable reasons
> rather than the personal preferences of the editor."
> — Van Buren & Buehler, *The Levels of Edit*, JPL Publication 80-1, 1980

**Not every difference is an error.** Carol Fisher Saller's formulation:

> "it's not a matter of being correct or incorrect. It's only a style"

If you cannot name which rule in `base.md` a change serves, or which line of the style sheet it
enforces, do not make it.

---

## Levels

JPL defines nine *types* of edit that combine into named levels. Their design property is the one worth
copying: **every type states what it must not do, and hands that off to a named other type.** The
Integrity Edit "will not resolve any apparent inconsistencies… discussed in Substantive Edit."

Note what JPL's levels are not: they are a menu chosen once per document against its budget, not a
pipeline every document runs through. The sequential version is later trade practice. Choose the level
first, and say which you chose.

Four levels, adapted:

### L1 — Structural

**Owns:** whether the document answers what it exists to answer; what is missing; what should not be
there; order of sections.
**Must not touch:** sentences. Do not polish prose that may be cut. If L1 finds a section unnecessary,
nothing below it applies.
**Hands off to:** L2 for everything at paragraph scale and below.

### L2 — Information order

**Owns:** the two axes from `base.md` §2. Cohesion — does each sentence set up the next. Coherence —
does the passage add up. Topic position: run the diagnostic, list what occupies the front of each
sentence, and check the paragraph has a subject. Stress position: does the emphasis land where the
substance is.
**Must not touch:** terminology and consistency, which are L3's. Do not rename things here.
**Hands off to:** L3.

### L3 — Reference and consistency

**Owns:** one name per thing; pronouns that resolve; terms glossed where they first cross into a
reader's vocabulary; the style sheet.
**This is the level that maintains the style sheet.** Every choice made here is recorded there, or it
will not survive the next edit.
**Must not touch:** claims. Whether an assertion is too strong is L4's.
**Hands off to:** L4.

### L4 — Claim status

**Owns:** `base.md` §4. Is each reason graded. Is inference marked as inference. Is anything asserted
that could have been sourced and was not. Is the boundary stated — what was not done.
**Must not touch:** anything else. By L4 the prose is settled; this level changes only what the
document claims about itself.

---

## The deletion pass

Mechanical, and the only part of this system that can be run rather than intended. Applies at L2–L4.

1. An opening sentence that announces what you are about to do.
2. A closing sentence that recaps, or offers further help.
3. "Note that", "it is worth noting", "as mentioned above".
4. Any sentence explaining the document's purpose to its reader.
5. Hedging adverbs carrying no information — "perhaps", "possibly", "somewhat". Keep a hedge that marks
   real uncertainty; deleting that one manufactures confidence.
6. Idioms and figurative phrases.
7. Marketing register in internal text: "powerful", "seamless", "robust".
8. Restatement of the diff in a commit message.
9. Any claim you could have sourced and did not.

---

## Query rather than resolve

When an ambiguity cannot be settled from the text, raise it instead of choosing. The threshold in
professional practice is roughly: fix silently what is small and unambiguous; ask about anything that
could change meaning, or that you cannot resolve alone.

A good query names the specific ambiguity, gives the candidate readings, and says what each would
change. "Unclear" is not a query.

Queries belong in a log alongside the document, not only in a conversation, because the useful thing
later is the record of what was asked and left open.

*(A note on provenance: "query, don't change" is a convenient slogan but not a term of art — it appears
in no source we could find. The underlying practice is documented by CIEP and several publishers as a
threshold model, which is what is described above.)*
