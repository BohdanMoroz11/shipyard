---
name: plan-roadmap
description: Plan a phase / initiative — a big, multi-session goal — as a living roadmap doc that decomposes into stages and tracks progress across sessions. Use when the work is clearly larger than one PR, spans days to weeks, or the user wants a roadmap rather than a single-PR plan. Invoke explicitly.
disable-model-invocation: true
---

# plan-roadmap — phase / initiative planning

A **phase** is a big goal with a vague edge that spans **many sessions** and is
**never built in one go**. This skill produces the living roadmap that
decomposes it into stages and tracks state over time. It plans; it does not
build.

First read:
- `../_method/vocabulary.md` — the phase/stage/task hierarchy

## When this vs `plan`

- One PR / one session of work → use **`plan`**.
- Multi-session, needs sequencing and progress tracking → use **`plan-roadmap`**,
  then use `plan` per stage as you pick each one up.

## Workflow

```
- [ ] 1. Read the project's plans hub + conventions
- [ ] 2. Clarify the goal and its rough boundary
- [ ] 3. Surface cross-cutting risks / sequencing questions — wait
- [ ] 4. Decompose into ordered stages (each ≈ one PR)
- [ ] 5. Write the living roadmap doc; wire it into the plans index/backlog
```

### Decompose into stages

Break the phase into an **ordered list of stages**, each small enough to be one
PR / one session. For each stage: a one-line goal and its rough dependency on
earlier stages. Don't over-specify commit slices here — that's the per-stage
`plan`'s job.

### The roadmap doc

Write a living doc in the project's plans dir. It must make **current state**
legible at a glance — which stages are done / in progress / not started. Match
the project's existing roadmap/phase style if one exists. Update it as stages
land; it's the tracking layer, so keep it honest.

## Boundaries

- No feature code, and no per-stage detail-planning — hand each stage to `plan`
  when you start it.
- If the "phase" turns out to fit in one PR, it was a stage — switch to `plan`.
