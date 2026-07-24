# Stage-doc template

A stage doc is a **living document**: forward-looking pre-work before the PR,
and the historical record of *why* it was built this way afterwards. It is the
primary place this stage's context lives — write it for the agent that picks the
work up next session with no memory of this conversation.

Every stage gets **its own file** — either in a phase's dir alongside that
phase's other stages, or directly in the plans dir when the stage stands alone
(see `vocabulary.md` → One doc per stage). Never as a section of the phase doc.

Match the project's existing style if it has one (read a recent stage doc
first). Otherwise use this shape.

## Shape

```markdown
# <Stage title>

> **Status: planning | in progress | shipped (YYYY-MM-DD).** One-line summary of
> the single theme of this stage.

Part of: <link to the phase doc, or "standalone — no phase">.
Canonical behaviour: <link to the domain doc(s) this touches>.
Rationale: <link to ADR, if a notable decision was made>.

## The request

What the user asked for, in their words (paraphrased is fine), including the
constraints they stated. Keep the original intent visible — don't launder it
into implementation-speak.

## Context

What exists today that this stage builds on, with real names and paths: the
modules and files involved, what the current behaviour is, what's already
half-built, and the surprises worth knowing (a schema field that exists but is
unused, a helper that already accepts the param nobody passes). This is the
section that saves the next session an hour of code reading — be specific and
generous.

## Approach

The shape of the change and why this way over the alternative. Call out anything
risky, anything deliberately deferred, and any constraint the implementation
must respect.

## Decisions

Questions that need the user's answer before coding. Keep each as short as the
question deserves — the question and the options you see. Add a note only when
there's something the user would actually want to know (a real risk, a tradeoff,
a cost). Record the answer in place; don't move it elsewhere.

1. **<question>?**
   Options: <a> / <b>. Leaning: <a>.
   → **Answer:** ...

2. **<question>?**
   Options: <a> / <b>.
   Note: <the tradeoff or risk worth flagging — only when there is one>.
   → **Answer:** ...

## Tasks

One commit each, in order. `- [ ]` not started · `- [ ] _(in progress)_` ·
`- [x]` done. Slugs (`<stage>-<area>`) so other docs can reference a task.

- [ ] **<slug>** — <a full description: what changes, in which files/modules,
      the names and shapes involved, the constraints it must respect, links to
      the domain doc or ADR it implements, and the end state that proves it
      works>
- [ ] **<slug>** — <same>
- [ ] **docs** — <which docs change and how>

## Milestone

The one concrete, demoable thing that's true when this stage is done — phrased
so it can be checked, not felt.

## Out of scope

What a reader might reasonably expect here but won't find, and where it went
(later stage, backlog, deliberately never).
```

## A task description, sized right

Too thin — the next session has to re-derive everything:

```markdown
- [ ] **api** — add the export endpoint
```

Right — the description *is* the context:

```markdown
- [ ] **s4-exports** — `POST /export` streaming CSV with the 37 NetSuite columns
      (verbatim field order from `LoL-main/exports.py`) and the
      OR-of-quick-pay/direct-payment row filter. Stream through `csv-stringify`
      so a full-year export can't OOM the pod; the endpoint must stay inside the
      tenant-scoped Prisma extension. Column contract and the filter rationale:
      [netsuite-csv.md](../integrations/netsuite-csv.md). End state: an export of
      the seed tenant's paid loads imports into NetSuite with zero rejected rows.
```

When reality diverges later, amend in place with a dated note rather than
rewriting:

```markdown
- [x] **s4-exports** — ... End state: ... **2026-05-22:** the brokerage
      multi-select moved to the loads page filter bar instead; the export modal
      now reads its filters from the URL state, so the two surfaces can't drift.
```

## Rules

- **Decisions come before code.** Ask, wait, record the answer in place. Don't
  guess a choice that's the user's to make.
- **Tasks are commits.** One logical change each; if a task needs two commits,
  it's two tasks.
- **Descriptions are long on purpose.** They're the context store, not a
  checklist label.
- **Keep it living.** Update the status line and the task markers as work lands.
- **Docs are always a task.** If behaviour changes, a docs commit is part of the
  stage, not an afterthought.
