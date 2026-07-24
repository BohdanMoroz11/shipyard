---
name: plan-stage
description: Write or refine a stage — one PR of work, carrying one or more requests — as its own living doc in the project's plans dir. Reads the repo's conventions, captures the requests and the existing context, settles the decisions that need the user, assumes the rest openly, and breaks the work into tasks with descriptions dense enough to hand off. Use for small non-urgent changes too — a one-commit change is still a stage, and its doc is simply short. Does not write feature code. Invoke explicitly.
disable-model-invocation: true
---

# plan-stage — plan one PR of work

A **stage** = one PR = one session, ending at a demoable milestone. It carries
**one or more requests** — real work arrives as a handful of related-enough asks,
and shipping them as one reviewable PR is correct. This skill produces the
alignment and the living stage doc *before* any code. It does not implement —
that's `ship`.

First read:
- `../_method/vocabulary.md` — phase/stage/task, status markers, doc conventions
- `../_method/stage-template.md` — the ten required sections and how dense to
  write them

## Workflow

```
- [ ] 1. Read the project's conventions (docs are the source of truth)
- [ ] 2. Read the code this touches; write the dated context
- [ ] 3. Draft an R-section per request, the tasks, and the milestone
- [ ] 4. Ask only the decisions that are the user's — then STOP and wait
- [ ] 5. Assume the rest into Residual questions
- [ ] 6. Answer the Prod risk question; write the doc
```

### 1. Read conventions first

Read the repo's orientation docs (`AGENTS.md`/`CLAUDE.md`, the plans hub README
or equivalent, and any domain doc the request touches). Never assume project
rules — the docs decide them. If the repo clearly isn't set up for this flow,
stop and tell the user to invoke `audit-repo` first (then `prep-repo` if gaps
need fixing) — `../_method/vocabulary.md` → Redirects.

**A standalone stage is the normal case.** Most work is one stage with no phase
above it; its doc goes straight in the plans dir and the plans hub's list is what
tracks it. Only reach for `plan-phase` when the work genuinely spans many PRs and
needs sequencing.

If this stage *does* belong to a phase, read that phase doc first: its
cross-cutting decisions and standing requirements already bind this stage, so
don't relitigate them. Either way the stage's doc is a **new file** — never a section
appended to the phase doc.

### 2. Read the code, then write the context

Before proposing anything, read the modules the requests touch. The **Context**
section is the deliverable here: real paths with `file:line` citations, current
behaviour, what's already half-built, the field or helper that exists but goes
unused, and the gap that makes the work hard. Be specific and generous — this is
what saves the next session an hour of code reading.

Date the heading (`## Context (as of YYYY-MM-DD)`). Context goes stale; the date
is what tells the next reader how much to trust it.

### 3. R-sections, tasks, milestone

One `## R1 — <title>` section per request, always numbered even when there's only
one. Each carries the ask in the user's words before the design — keep the
original intent visible rather than laundering it into implementation-speak.

Then the tasks: numbered, in order, one logical change each, **no checkboxes**.
Each names what changes and **where** — real files and modules — the shapes and
names involved, the constraints it must respect, the docs or ADRs it implements,
and the end state. Several sentences to a paragraph is normal; see
`stage-template.md` → "A task description, sized right". A one-line task is a bug
unless the task is genuinely trivial. Docs are always a task.

Then the milestone: the one concrete, demoable thing that's true when the stage
is done, phrased so it can be checked rather than felt.

### 4. Decisions — ask few, ask well

Most stages need nothing here. Only ask what genuinely belongs to the user: stack,
data model, UX tradeoff, anything materially ambiguous in the ask. Keep each
question short — the question, the options you see, your leaning. Add a note only
when there's a real risk, tradeoff, or cost.

Then **stop and wait**. Do not write code and do not guess these answers. Record
them in the **Resolved decisions** table, dated.

### 5. Assume the rest

Everything smaller goes to **Residual questions**: state the assumption, say why
it's probably right, invite correction. "Assumed yes; confirm during build if
this feels wrong" is a complete entry.

This is the pressure valve. Blocking is for the few choices that are genuinely
the user's — not for every fork in the road. A stage that stalls on trivia was
planned badly.

### 6. Prod risk, then write

Answer the **Prod risk** question explicitly, including:

> **Does `git revert` + redeploy fully undo this?**

A *no* means `ship` will owe a rollout runbook, and — more importantly — it may
change the design now (an expand/contract migration instead of a destructive one,
a two-release cutover instead of one). Reversibility is decided while planning,
not while deploying.

Then write the doc using `stage-template.md`, **every section, under its own
name**. A one-commit stage answers most of them in a line each — see
"A small stage, in full" — and that is the correct output, not a sign you should
have trimmed the shape. If the stage belongs to a phase, link it from that phase's stage list and
set its marker. Update the plans index/backlog if the project keeps one.

If the work is clearly bigger than one PR / one session, it's a **phase**: stop,
say why, and tell the user to invoke `plan-phase` (`../_method/vocabulary.md` →
Redirects). Don't decompose it here.

## Boundaries

- No feature code. Writing the stage doc and linking it from the phase doc /
  plans index is fine; touching source is not.
- **Small work still gets the full shape and a PR.** The doc gets shorter on its
  own; the shape and the pipeline don't change.
- **Many requests per stage is fine; many PRs' worth of work is not.** The test
  is PR size, not thematic purity. Don't split a coherent PR into five stages
  because the asks are unrelated to each other.
