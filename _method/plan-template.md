# Plan-doc template (stage-level)

A plan is a **living document** that lives in the project's plans dir. It
starts as forward-looking pre-work and, once shipped, stays as the historical
record of *why* the stage was built the way it was.

Match the project's existing plan style if it has one (read a recent plan doc
first). Otherwise use this shape:

```markdown
# <Stage title>

> **Status: planning | in progress | shipped (YYYY-MM-DD).** One-line summary
> of the single theme of this stage.

Canonical behaviour: <link to the domain doc(s) this touches>.
Rationale: <link to ADR if a notable decision was made>.

## The request

What the user/client asked for, in their words (paraphrased is fine). Keep the
original intent visible — don't launder it into implementation-speak.

## Analysis

Current state, what needs to change, and the rough approach. Call out anything
surprising or risky here.

## Decisions

Numbered. These block coding until resolved. For each item, propose an answer
up front (don't just ask blank questions) — include risks the user should know
and how you'd handle them. Then **stop and wait**. When the user answers, mark
the item resolved in place (don't move it to a separate section).

1. **Q:** ...
   - **Proposed:** ...
   - **Risks:** ...
   - **Mitigation:** ...
   - **Status:** open | resolved — <user's choice / confirmation>

2. **Q:** ...
   - **Proposed:** ...
   - **Risks:** ...
   - **Mitigation:** ...
   - **Status:** open | resolved — ...

## Commit slices

The ordered commits (tasks) this stage will ship as. One logical change each.

1. **<area>** — ...
2. **<area>** — ...
3. **docs** — update the docs this behaviour change affects.
```

## Rules

- **Decisions come before code.** Write questions with proposed answers, wait
  for the user, mark status in place. Don't guess on collaborative choices.
- **Keep it living.** Update status and slices as reality changes; a stale plan
  is worse than none.
- **Docs are always a slice.** If behaviour changes, a docs commit is part of
  the plan, not an afterthought.
