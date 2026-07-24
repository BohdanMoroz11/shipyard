---
name: plan
description: Write or refine a stage-level implementation plan as a living doc in the project's plans dir. Analyzes the request, surfaces decisions (question + proposed answer + risks), aligns with the user, and decomposes the work into commit slices — without writing feature code. Use when starting a scoped piece of work (roughly one PR) and the user wants a plan before implementation. Invoke explicitly.
disable-model-invocation: true
---

# plan — stage-level planning

A **stage** = one PR = one session. This skill produces the alignment and the
living plan doc *before* any code. It does not implement — that's `ship`.

First read:
- `../_method/vocabulary.md` — phase/stage/task, stage kinds, the two modes
- `../_method/plan-template.md` — the plan-doc shape

## Workflow

```
- [ ] 1. Read the project's conventions (docs are the source of truth)
- [ ] 2. Analyze the request; draft approach + rough steps
- [ ] 3. Surface decisions (Q + proposed + risks) — then STOP and wait
- [ ] 4. Mark answers resolved in place; finish the plan doc
- [ ] 5. Confirm scope is one stage; hand off to `ship`
```

### 1. Read conventions first

Read the repo's orientation docs (`AGENTS.md`/`CLAUDE.md`, `docs/plans/README.md`
or equivalents, and any domain doc the request touches). Never assume project
rules — the docs decide them. If the repo clearly isn't set up for this flow,
suggest `audit-repo` first (then `prep-repo` if gaps need fixing).

### 2. Analyze

Produce: current state, what needs to change, the rough approach, and a
provisional list of commit slices. Match the project's existing plan style
(read a recent plan doc).

### 3. Decisions — the important part

List numbered **decisions** using the shape in `plan-template.md`: each item
is a question with a **proposed answer**, **risks** the user should know, and
a **mitigation**. Then **stop and wait**. Do not write code and do not guess
collaborative choices.

### 4. Align and write

When the user answers, mark each item **resolved in place** (status + choice)
and write/update the plan doc. Update the plans index/backlog if the project
keeps one.

### 5. Scope check

If the work is clearly bigger than one PR / one session, it's a **phase**, not
a stage — use `plan-roadmap` to break it down instead. Otherwise, the plan is
ready for `ship`.

## Boundaries

- No feature code. Editing the plan doc / plans index is fine; touching source
  is not.
- One theme per stage. If two unrelated things appear, split into two plans.
