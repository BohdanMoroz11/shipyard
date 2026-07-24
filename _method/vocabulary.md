# Flow vocabulary

Shared by every skill in this repo. Read it once so the terms mean the same
thing everywhere.

## Three words: phase, stage, task

There are exactly three units of work, and no synonyms for them. Never write
"roadmap", "plan", "initiative", "epic", "todo", "slice", "batch", or "request"
to mean one of these — write phase, stage, or task.

| Word | Size | Where it lives |
| --- | --- | --- |
| **Phase** | many sessions, many PRs | a **phase doc** — an overview only |
| **Stage** | one session, one PR | its **own stage doc** |
| **Task** | one commit | a line in its stage doc |

- **Phase** — a big goal with a vague edge (days–weeks). Never built in one go.
  Its doc is an **overview**: the goal, the cross-cutting decisions, and the
  ordered list of stages with their status. Written by `plan-phase`.
- **Stage** — the working unit. One theme, one PR, one session, ending at a
  **milestone**: something concretely demoable. Every stage gets **its own doc**,
  which holds its context and its tasks. Written by `plan-stage`.
- **Task** — one logical change = one commit. Lives in its stage's doc, never in
  the phase doc. Executed by `ship`.

If a "phase" fits in one PR, it was a stage. If a "stage" needs several PRs,
it's a phase — split it.

## One doc per stage

**A phase doc never contains tasks.** It stays an overview you can read in a
minute: what the phase is for, what was decided across stages, which stages
exist, and where each one stands. Every stage's context and task list lives in
its own file, linked from that list.

Why: a stage's context is long (see below), and a phase has many stages. Inlined,
the phase doc becomes thousands of lines and the overview is lost — you can no
longer see the shape of the work. Separate files also mean a stage doc can be
read, edited, and committed with the PR it describes, without touching the
tracking layer.

Layout, unless the project already has one:

```
docs/plans/
  README.md                 the plans hub — index + how this project works
  phase-3/
    README.md               the phase doc (overview)
    s1-foundation.md        one stage doc per stage
    s2-reference-data.md
  fix-statement-rounding.md a standalone stage, no phase
```

A stage doesn't need a phase — plenty of work is one stage that stands alone, and
it gets a doc in the plans dir like any other.

## Status markers

Every stage list and task list uses the same three markers, and nothing else:

```
- [ ] not started
- [ ] _(in progress)_ underway
- [x] done
```

Status lives in the marker. No status columns, no progress tables, no per-doc
variations.

## Docs carry the context

The stage and task descriptions are the **primary place project context is
stored** — for the agent picking the work up next session, and for the reader
asking a year later why it was built this way. They are meant to be long.

A task description says what changes, where (real paths, modules, field and
endpoint names), the constraints or gotchas that shaped it, links to the domain
docs and ADRs it touches, and the end state. Several sentences to a paragraph is
normal. One-liners are only for genuinely trivial tasks.

When reality diverges after the fact, amend the description in place with a
dated note rather than silently rewriting history — the doc is the record.

## Two ways to ship

- **Pipeline** — the normal path: plan the stage, align, branch, one commit per
  task, PR, agent checks, human smoke. Use for anything non-trivial.
- **Hotfix** — urgent. Skip the stage doc and the PR, ship straight to `main`,
  with a shorter definition-of-done (tests still green, docs still updated if
  behaviour changed). Only when speed genuinely matters.

## Who verifies what

Two different owners — don't conflate them:

- **Agent checks** — tests, lint, typecheck, codegen sync, CI. The agent runs
  these and hands back something that passes them.
- **Smoke verification** — exercising the changed flow in a real run. **Done by
  the user, never by the agent.** The agent flags that smoke is needed and what
  to look at; it does not perform it.

## Decisions before code

Most stages need no input beyond the initial ask — just build them. When a
choice genuinely belongs to the user (stack, data model, UX tradeoff), the agent
asks it in the doc's **Decisions** section, waits, then builds. Never guess a
decision that is the user's to make.

## Doc-driven, not skill-driven

Project-specific facts (where phase and stage docs live, money/timezone rules,
codegen gates, naming) belong in the **project's own docs**, not in these
skills. Every skill reads the repo's orientation docs first (`AGENTS.md` /
`CLAUDE.md`, the plans hub README, or their equivalents) and treats them as the
source of truth. These skills carry the *method*; the repo carries the
*specifics*.
