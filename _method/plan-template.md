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

## Open questions / risks

Numbered. These block coding until resolved. Ask them before writing code.

1. ...
2. ...

## Resolved decisions

Fill in as the user answers. This table is the alignment record.

| # | Question | Choice |
| --- | --- | --- |
| 1 | ... | ... |

## Commit slices

The ordered commits (tasks) this stage will ship as. One logical change each.

1. **<area>** — ...
2. **<area>** — ...
3. **docs** — update the docs this behaviour change affects.
```

## Rules

- **Open questions come before code.** Write them, wait for answers, fold them
  into the decisions table. Don't guess on collaborative choices.
- **Keep it living.** Update status and slices as reality changes; a stale plan
  is worse than none.
- **Docs are always a slice.** If behaviour changes, a docs commit is part of
  the plan, not an afterthought.
