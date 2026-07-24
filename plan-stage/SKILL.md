---
name: plan-stage
description: Write or refine a stage — one PR of work — as its own living doc in the project's plans dir. Reads the repo's conventions, captures the request and the existing context, asks the decisions that need the user's answer, and breaks the work into tasks (one commit each) with descriptions dense enough to hand off. Does not write feature code. Invoke explicitly.
disable-model-invocation: true
---

# plan-stage — plan one PR of work

A **stage** = one PR = one session, ending at a demoable milestone. This skill
produces the alignment and the living stage doc *before* any code. It does not
implement — that's `ship`.

First read:
- `../_method/vocabulary.md` — phase/stage/task, status markers, doc conventions
- `../_method/stage-template.md` — the stage-doc shape and how dense to write it

## Workflow

```
- [ ] 1. Read the project's conventions (docs are the source of truth)
- [ ] 2. Read the code this touches; write the request + context
- [ ] 3. Draft the approach, the tasks, and the milestone
- [ ] 4. Ask the decisions that need the user — then STOP and wait
- [ ] 5. Record the answers in place; finish the stage doc
- [ ] 6. Confirm it's one stage; hand off to `ship`
```

### 1. Read conventions first

Read the repo's orientation docs (`AGENTS.md`/`CLAUDE.md`, the plans hub README
or equivalent, and any domain doc the request touches). Never assume project
rules — the docs decide them. If the repo clearly isn't set up for this flow,
suggest `audit-repo` first (then `prep-repo` if gaps need fixing).

If this stage belongs to a phase, read that phase doc first: its cross-cutting
decisions and exit criteria already bind this stage, so don't relitigate them.
The stage's own doc is a **new file** (in the phase's dir, or the plans dir if
it's standalone) — never a section appended to the phase doc.

### 2. Read the code, then write the context

Before proposing anything, read the modules the request touches. The **Context**
section is the deliverable here: real paths, current behaviour, what's already
half-built, the field or helper that exists but goes unused. Be specific and
generous — this section is what saves the next session an hour of code reading.

### 3. Approach, tasks, milestone

Produce the shape of the change and why this way, the ordered tasks (one commit
each), and the milestone that proves the stage is done.

**Write task descriptions long.** Each one names what changes and where, the
shapes and names involved, the constraints it must respect, the docs or ADRs it
implements, and the end state. Several sentences to a paragraph is normal — see
`stage-template.md` → "A task description, sized right". A one-line task
description is a bug unless the task is genuinely trivial.

### 4. Decisions — the important part

Most stages need nothing here; just build them. Only ask what genuinely belongs
to the user (stack, data model, UX tradeoff, anything ambiguous in the request).
Keep each question short: the question, the options you see, your leaning. Add a
note only when there's a real risk, tradeoff, or cost — don't pad every question
with one.

Then **stop and wait**. Do not write code and do not guess these answers.

### 5. Align and write

Record each answer **in place** under its question, then write/update the stage
doc using `stage-template.md`. If the stage belongs to a phase, link the new doc
from that phase's stage list and set its marker. Update the plans index/backlog
if the project keeps one.

### 6. Scope check

If the work is clearly bigger than one PR / one session, it's a **phase** — use
`plan-phase` to decompose it instead. Otherwise the stage is ready for `ship`.

## Boundaries

- No feature code. Writing the stage doc and linking it from the phase doc /
  plans index is fine; touching source is not.
- One theme per stage. If two unrelated things appear, that's two stages.
