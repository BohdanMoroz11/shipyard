# Roadmap-doc template (phase-level)

A phase roadmap is a **living tracking doc** in the project's plans dir. It
holds a multi-session goal, decomposes it into stages, and stays honest about
what's done / in progress / not started. It is **not** a stage plan — no
commit slices here; those belong in each stage's plan doc.

Match the project's existing roadmap/phase style if it has one. Otherwise use
this shape:

```markdown
# <Phase / initiative title>

> **Status: exploring | in progress | done (YYYY-MM-DD).** One-line summary of
> the goal.

## The goal

What outcome we're driving toward, in the user's words where possible. Keep the
boundary rough but legible — what is in / out of this phase.

## Context

Current state, why this phase exists now, and any constraints that shape
sequencing (deps, migrations, external dates).

## Decisions

Cross-cutting questions that affect *multiple stages* or the sequencing itself.
Same shape as stage decisions: propose an answer, call out risks, wait, mark
resolved in place. Don't re-litigate per-stage detail here — hand that to
`plan` when a stage starts.

1. **Q:** ...
   - **Proposed:** ...
   - **Risks:** ...
   - **Mitigation:** ...
   - **Status:** open | resolved — <user's choice / confirmation>

## Stages

Ordered. Each stage ≈ one PR / one session. One-line goal + dependency note.
Tick status as stages land; link each to its plan doc once it exists.

| # | Stage | Depends on | Status | Plan |
| --- | --- | --- | --- | --- |
| 1 | ... | — | not started \| in progress \| shipped | <link or —> |
| 2 | ... | 1 | not started | — |

## Progress notes

Short dated bullets as reality changes (scope shifts, stage splits, deferred
work). Keep the table above as the source of truth for status; use this for
narrative.
```

## Rules

- **Phase = tracking + sequencing.** Stages do the detailed planning via `plan`.
- **Decisions here are cross-cutting only.** Stack-wide or order-shaping
  choices belong; a single stage's UX tradeoff does not.
- **Keep it living.** Update stage status when work lands; a stale roadmap is
  worse than none.
