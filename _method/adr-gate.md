# The ADR gate

An ADR records a **constraint** — something a future PR would violate without
knowing it. It is not a record of what happened; that's what the archived stage
doc is for.

Cited by `ship` (step 5), `definition-of-done.md`, `audit-repo`, and `prep-repo`.
This file is the only place the gate is defined — everywhere else links here.

## The two questions

Both must be **yes**:

1. **Would a future PR be wrong if its author didn't know this?**
2. **Is there nothing mechanical already enforcing it?**

Worked examples:

- *"Money is integer cents"* — **passes both.** A `BigInt` holding dollars
  typechecks fine and fails as wrong numbers in production.
- *"Vitest, not Jest"* — **fails the second.** The config and the testing doc
  already enforce it. That's a domain doc.
- *"This page uses a modal, not a detail route"* — **fails the first.** The next
  author copies the neighbouring page.

## More than one ADR from a stage is a smell

It usually means recording narration instead of constraints; the right answer is
normally zero ADRs and a better domain doc.

Nothing is lost when the gate says no. The reasoning stays in the stage doc,
which gets **archived, not deleted**. That's what lets the bar be this high.

## Writing one

`docs/decisions/NNNN-slug.md`, or the repo's existing convention. Record **what
was rejected and why**, not just what was chosen — the rejected option is the
part a future author needs, because it's the one they're about to propose.

Take the next free number when you write it, and commit it with the stage. Then
**re-check the number before merge** and renumber if another branch has taken it
meanwhile — two branches both grabbing `0016` is the common collision.

## Why the constraint layer gets its own files

An ADR must survive its plan doc's archival. It is cited by number from
anywhere and stays valid. A decision that doesn't bind future work isn't an ADR
— it stays in the archived stage doc, which is a perfectly good home.
