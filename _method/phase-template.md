# Phase-doc template

A phase doc is the **overview** of a multi-session goal: what it's for, what was
decided across its stages, and where each stage stands. It is a living tracking
layer, not a work doc.

**It never contains tasks.** Each stage gets its own doc holding its context and
task list (see `vocabulary.md` → One doc per stage); the phase doc links to them
and stays readable in a minute.

Match the project's existing style if it has one. Otherwise use this shape.

## Shape

```markdown
# <Phase title>

> **Status: planning | in progress | done (YYYY-MM-DD).** One-line summary of the
> goal.

A paragraph or two on what this phase is and what forces shape it — the ask
behind it, what's driving the priority, what the end of it looks like.

Design detail lives elsewhere; jump to:

- [<architecture doc>](...) — <what it settles>
- [<domain doc>](...) — <what it settles>
- [<ADR>](...) — <the decision it records>

## Context

Where the project is today, why this phase exists now, and the constraints that
shape the order (dependencies, migrations, external dates, a single-replica
limit — whatever will bite). Be honest about work that shipped outside the plan;
the doc is only useful if it matches reality.

## Decisions

Only questions that affect **several stages** or the order itself. Same shape as
in a stage doc: the question, the options, a note when one is warranted, the
answer recorded in place. Per-stage questions wait for that stage's own
`plan-stage` pass.

1. **<question>?**
   Options: <a> / <b>. Leaning: <a>.
   → **Answer:** ...

## Stages

Ordered. `- [ ]` not started · `- [ ] _(in progress)_` · `- [x]` done. One entry
per stage: what it delivers, its dependency, its milestone, and a link to its
doc. **Tasks live in those docs, not here.**

- [x] **[S1 — <title>](s1-<slug>.md)** — <what it delivers and why it comes
      first; the surfaces it touches; anything it deliberately defers>.
      **Milestone:** <the concrete demoable outcome>.
- [ ] _(in progress)_ **[S2 — <title>](s2-<slug>.md)** — <what it delivers>.
      **Milestone:** <outcome>.
- [ ] **S3 — <title>** — <one-line goal>; needs S2 for <reason>. Doc written when
      the stage is picked up.

## Exit criteria (every stage)

The bar each stage clears before it counts as done — the project-wide ones that
would otherwise get relitigated per stage (integration test per endpoint, codegen
diff clean, cross-tenant isolation asserted, and so on). Read the repo's own docs
for the real list; don't invent it.

## Order and parallelism

Which stages are independent, which are gated on which, and where two can run
side by side. One bullet per non-obvious dependency.

## Out of scope

What this phase deliberately doesn't cover, and where it went — the next phase,
the backlog, or nowhere on purpose.
```

## Rules

- **No tasks here.** If you're writing a task, you're in the wrong file — it
  belongs to a stage doc.
- **A stage entry is one paragraph plus a milestone.** Enough to know what the
  stage is and whether to pick it up next; the depth lives in its own doc.
- **A stage doc doesn't exist until the stage is planned.** List the stage with a
  one-line goal and no link; `plan-stage` writes the doc and adds the link when
  the work starts. Never stub a doc with placeholders.
- **Decisions here are cross-cutting only.** A single stage's tradeoff belongs to
  that stage.
- **Keep it living.** Update markers as stages land, and amend entries in place
  with a dated note when reality diverges. A stale phase doc is worse than none.
