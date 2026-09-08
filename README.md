# prose-standard

A writing standard for prose that another person has to read: pull requests, commit messages, design
notes, specifications, and documents for readers outside the team.

It exists because the usual advice — *be clear, be concise* — cannot be checked, and because an earlier
version of this material was written from one person's impressions of one document that happened to
come out well. That version was replaced. What is here is built from sources, and `research/` records
them.

## The documents

| File | What it is | When you use it |
| :--- | :--- | :--- |
| `base.md` | The standard. Four facets: reference, order, load, status | While writing |
| `editing.md` | Levels of edit, the deletion pass, how to query | On a draft that exists |
| `style-sheet.md` | Per-document record of terms and decisions | Alongside each document |
| `outputs/` | Carve-outs where a base default is wrong for a specific output | When writing that output |
| `research/` | The sources, quoted and cited, with evidential status | When you want to check a claim |

`base.md` is the one to read first, and the only one that must be read in full.

## What this standard claims, and what it does not

Every claim in `research/` is tagged **EMPIRICAL** (measured, with what was measured), **THEORETICAL**
(a model or formalism), or **CRAFT** (received professional practice, no study cited). The distribution
matters:

- **Formal models:** Centering theory's licensing condition for pronouns; Halliday & Hasan's taxonomy
  of cohesive ties. These are precise, and precise is not the same as verified.
- **Empirical, in adjacent settings:** cognitive load and multimedia-learning effects. Measured mostly
  in instructional contexts, not on technical documents.
- **Craft:** reader-expectation writing, plain-language guidance, legal and journalistic convention,
  the whole editing tradition. Long practice, little measurement.

**This standard has not been tested.** No comparison was run between documents written with and
without it. A cheap experiment exists — give two readers the same content in both forms and compare
answer rate — and nobody has done it. Until someone does, treat this as a defensible design, not a
demonstrated improvement.

Two places where sources conflict, and where this standard picks a side:

- **Passive voice.** Gopen & Swan defend it as a topic-position decision; plain-language guidance
  prescribes the active. We follow Gopen & Swan, because their account explains when each is right
  instead of banning one.
- **Levels of edit.** JPL's levels are a menu chosen per document; modern trade practice treats them as
  a sequence. We use them as a menu and say which level was chosen.

Corrections already applied: "germane load" is dropped in Sweller's own recent work and does not appear
here; the inverted pyramid's telegraph-origin story is a myth and is not repeated; "query, don't change"
is a slogan with no traceable source and is described as a threshold practice instead.

## Gaps

The clearest hole is the empirical case for plain language. Kimble's compilation of studies could not be
reached, nor could Wydick. Carroll's minimalism is secondary throughout. Search engines were
unavailable during the research, so gaps were filled by direct fetching and archive lookups, and some
could not be filled at all. Each file lists its own.

## Installing it

Two mechanisms, two surfaces. They are not interchangeable.

**The skill — for artifacts.** `.claude/skills/prose/`. Loads only when the task is to write or revise
something. Keep it on-demand: it is ~1 500 words of standard behind a short entry point, and it has no
business being in context while someone writes C#.

```
cp -r .claude/skills/prose <target-repo>/.claude/skills/     # one project
cp -r .claude/skills/prose ~/.claude/skills/                 # everywhere
```

**The output style — for replies.** `.claude/output-styles/agent-responses.md`. Always on once
selected, because turn-shaping that applies only sometimes is not turn-shaping. Select with
`/output-style`.

```
cp .claude/output-styles/agent-responses.md ~/.claude/output-styles/
```

**Not `CLAUDE.md`.** A pointer line is fine; the standard itself is not. It would sit in context on
every turn of every task, including tasks with no prose in them.

**Not a path-scoped rule**, except as a supplement. Rules fire on file globs, so `docs/**/*.md` works
and PR descriptions and commit messages — the cases that matter most — never match anything.

Copying is the install, so drift is the cost. This repository is the source of truth; re-copy rather
than editing a copy in place.
