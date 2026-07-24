---
name: plan-phase
description: Plan a phase — a big, multi-session goal — as a living overview doc that decomposes it into ordered stages with milestones, tracks which have landed, and records cross-cutting decisions. Writes no tasks and no stage docs; each stage is planned separately by plan-stage. Use when the work is clearly larger than one PR or spans days to weeks. Invoke explicitly.
disable-model-invocation: true
---

# plan-phase — decompose a multi-session goal

A **phase** is a big goal with a vague edge that spans many sessions and is never
built in one go. This skill produces the phase doc: a living **overview** that
decomposes the goal into stages and tracks state over time. It plans; it does not
build, and it does not write stage docs or tasks.

First read:
- `../_method/vocabulary.md` — phase/stage/task, status markers, doc conventions,
  redirects
- `../_method/phase-template.md` — the phase-doc shape

## When this vs `plan-stage`

- One PR / one session of work → **`plan-stage`**.
- Many PRs, needs sequencing and progress tracking → **`plan-phase`**, then
  `plan-stage` per stage as you pick each one up.

## Workflow

```
- [ ] 1. Read the project's plans hub + conventions
- [ ] 2. Clarify the goal, what's in, what's out
- [ ] 3. Ask cross-cutting decisions — then STOP and wait
- [ ] 4. Decompose into ordered stages, each with a milestone
- [ ] 5. Write the phase doc; wire it into the plans index/backlog
```

### 2. Goal and boundary

Write what the phase delivers, the forces driving it, and the context that shapes
the order — dependencies, migrations, external dates, platform limits. Be honest
about what's already shipped outside the plan; the doc is only useful if it
matches reality.

### 3. Cross-cutting decisions

Only questions that shape **several stages** or the **order** itself. Keep them
short — the question, the options, a note when there's a real tradeoff. Then stop
and wait. Stage-level questions belong to that stage's `plan-stage` pass; don't
pre-answer them here.

Unanswered ones live in the phase doc's **Open decisions** table; answered ones
move to **Resolved decisions** with the date. A phase runs for weeks, so the date
is what distinguishes a live decision from one the plan has moved past — and
those entries are the raw material for the ADRs its stages will distil.

### 4. Decompose into stages

An ordered list of stages, each small enough to be one PR / one session. Per
stage: a paragraph on what it delivers, its dependency on earlier stages, and a
**milestone** — the concrete demoable outcome that ends it. That's the whole
entry.

**Write no tasks.** Tasks live in stage docs, and a stage doc gets written by
`plan-stage` when that stage is picked up. Don't pre-create stage files or stub
them with placeholders — an unlinked one-line entry is the correct state for a
stage nobody has started.

### 5. Write the phase doc

Use `phase-template.md` (or the project's existing style), in the layout the
project uses for phases (see `vocabulary.md` → One doc per stage; default is a
dir per phase with the phase doc as its `README.md`). Current state must be
legible at a glance from the status markers. Note the project-wide standing
requirements every stage clears, read from the repo's own docs. Update the doc as stages land —
add each stage doc's link when `plan-stage` writes it.

## Boundaries

- No feature code, no tasks, no stage docs — hand each stage to `plan-stage` when
  you start it.
- If the phase turns out to fit in one PR, it was a stage: stop, say so, and
  tell the user to invoke `plan-stage` (`../_method/vocabulary.md` → Redirects).
