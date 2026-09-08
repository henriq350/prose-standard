# Output carve-outs

Two production style guides — Google's and Microsoft's — are organised the same way: one base arranged
by linguistic facet, with type-specific guidance added only where a facet's default is actually wrong
for that type. Microsoft carves out three such branches, Google essentially one.

That is the pattern here. `base.md` holds. Each entry below states only **what differs and why**. If an
output is not listed, nothing differs.

Diátaxis supplies the discipline for writing these: name the failure that occurs at a specific
boundary, rather than issuing a general instruction. It names distinct failures for each adjacent pair
of document types — how-to plus reference produces *distraction*; tutorial plus explanation produces
*overload*. A carve-out should be that specific.

---

## Commit messages

**Differs on order.** The stress position is the subject line, and it is read in a list, out of context,
years later. It carries the consequence, not the mechanism.

**Differs on load.** The diff is adjacent and free to consult, so restating it is pure redundancy in
Mayer's sense. Say why the change was made and what would otherwise surprise a reader.

**Same on everything else**, especially §4: a commit that made a judgement call says so.

## Pull request descriptions

**Differs on reference.** Identifiers, paths and error codes are the reader's own vocabulary. Use them
plainly and exactly; a phrase like "the download check" in place of `AllowsDownloadTo` costs the reader
a search and gains nothing.

**Differs on status.** This is where §4 earns most. Distinguish what was decided, what was defaulted
because nothing said otherwise, and what remains deliberately undecided. Reviewers calibrate their
attention on that distinction.

**Boundary to watch:** a PR body legitimately carries several threads. Finish each before opening the
next; interleaving them produces the failure Diátaxis calls distraction.

## Documents for a reader outside the team

Clients, counsel, regulators, anyone who owns the decision but not the system.

**Differs on reference.** Every term from your vocabulary is a crossing. Gloss it once where it first
appears and say why they can then ignore it. Their own domain vocabulary is used exactly and never
explained back to them.

**Differs on order.** Lead with what they must decide. Reasoning follows the decision; it does not
build to it.

**Differs on load.** Describe behaviour by what happens to a person in their world, not by what the
system does internally. This removes most of the vocabulary that would otherwise need a crossing.

**Same on status, and it matters more here.** A provisional choice presented as settled fact will be
accepted as one.

## Design notes and decision records

**Differs on status.** The document exists to record why, so §4 is the substance rather than a
refinement: what was decided, what was rejected and on what grounds, what was left open.

**Differs on order.** The decision goes first. A note that reconstructs the reasoning chronologically
and reveals the outcome at the end cannot be skimmed by the person who needs only the outcome.

## Specifications

**Differs on reference.** Terminological drift is a correctness defect here, not a style defect. The
style sheet is mandatory.

**Differs on status.** Every requirement carries whether it is settled, provisional, or blocked, and on
what. An unmarked requirement reads as agreed.
