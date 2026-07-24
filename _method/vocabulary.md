# Flow vocabulary

Shared by every skill in this repo. Read it once so the terms mean the same
thing everywhere.

## Three words: phase, stage, task

Three units of work. Use these names for them — synonyms are how two projects
become unreadable side by side.

| Word | Size | Where it lives | Written by |
| --- | --- | --- | --- |
| **Phase** | many sessions, many PRs | a **phase doc** — an overview only | `plan-phase` |
| **Stage** | one session, one PR | its **own stage doc** | `plan-stage` |
| **Task** | one commit | a numbered line in its stage doc | executed by `ship` |

- A **phase** is a big goal with a vague edge (days–weeks), never built in one
  go. Its doc is an overview: the goal, the cross-cutting decisions, and the
  ordered list of stages with their status.
- A **stage** is the working unit, ending at a **milestone** — something
  concretely demoable. It carries **one or more requests** (`R1`, `R2`, …), not
  necessarily one theme.
- A **task** is one logical change. Not a "commit slice", not a "deliverable".

If a "phase" fits in one PR, it was a stage. If a "stage" needs several PRs, it's
a phase — split it. The test is **PR size, not thematic purity**: a stage that
ships five loosely related client requests as one reviewable PR is one stage, not
five.

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

**Markers track work that spans sessions. Nothing else gets them.**

A **phase doc's stage list** uses these three markers, and nothing else:

```
- [ ] not started
- [ ] _(in progress)_ underway
- [x] done
```

A **stage doc's task list** uses **none of them** — tasks are a numbered list.

Why the line falls there: a phase runs across many sessions, so its state is
knowable only from its doc, and the doc must carry it. A stage is one session and
one PR — its state is already in `git log`, the PR, and the doc's status line.
A marker per task is a second copy of that state, and it goes stale the moment a
stage lands a fix commit that wasn't in the plan (which is most stages). The task
list is the **plan of record**, amended when reality diverges; it is not a
progress tracker.

Live state in a stage doc lives in exactly one place: the status line under the
title. No status columns, no progress tables, no per-doc variations.

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

## Three doc layers — provenance, behaviour, constraint

Every project doc answers one of three questions. Keeping them apart is what
stops a plans dir from silently becoming the source of truth:

| Layer | Answers | Lives in |
| --- | --- | --- |
| **Provenance** | why we built it this way, at the time | stage + phase docs, archived when shipped |
| **Behaviour** | what the system does now | domain docs |
| **Constraint** | what future work may not do | ADRs |

A shipped stage doc is **history**. It is kept, linked, and never deleted — but
it is not what a reader consults to learn how the system behaves today. `ship`
distils it into the other two layers before archiving it (`ship` step 5).

The constraint layer earns its own files because it must survive its plan doc's
archival: an ADR is cited by number from anywhere and stays valid. What clears
that bar is defined once, in `adr-gate.md` — the bar is high, and most stages
produce no ADR at all.

## Two ways to ship

- **Pipeline** — the normal path: plan the stage, align, branch, one commit per
  task, PR, agent checks, human smoke. Use for **everything** that isn't urgent,
  down to a one-line change; a small stage's doc is simply a short one.
- **Hotfix** — urgent. Skip the stage doc, take the fastest integration route
  the repo allows, with a shorter definition-of-done (tests still green, docs
  still updated if behaviour changed). Only when speed genuinely matters.

What a hotfix skips is the **planning**, not the review. If the repo requires a
PR, a hotfix is still a PR — just a fast-tracked one.

## Redirects — no skill invokes another

Every skill here is invoked **by the user, by name**. None of them can hand off
to another, and none should pretend to.

So when a skill finds it's the wrong one for this change — a "stage" that's
really a phase, a "hotfix" that needs design decisions, a `plan-stage` in a repo
that was never prepped — the move is always the same three steps:

> **Stop. Say what you found. Name the skill for the user to invoke.**

Never switch, never carry on in the wrong shape, and never assume permission
from the fact that the right skill exists. The user chose this path; finding out
it was the wrong one is information they need, not a decision you get to make
for them.

## Integration route

Whether `main` accepts direct commits is a **repo fact, not a method choice**.
Read it from the repo's docs (`AGENTS.md`, a contributing guide) or infer it
from branch protection. Two routes exist:

- **Direct** — commit to `main`. Only where the repo permits it.
- **Fast-tracked PR** — branch, commit, open the PR, wait for CI, merge
  immediately.

These are the same urgency at two levels of ceremony. A skill picks the fastest
route **the repo allows**; it never assumes one. Assuming direct-to-`main` is
how a portable skillset stops being portable.

## Who verifies what

Two different owners — don't conflate them:

- **Agent checks** — tests, lint, typecheck, codegen sync, CI. The agent runs
  these and hands back something that passes them.
- **Smoke verification** — exercising the changed flow in a real run, and
  **signing off on it**. That sign-off is the user's, always.
- **Deploying a rollout runbook** — also the user's. The agent writes it; it
  doesn't run it.

The agent may drive the app to check its own work — that's testing, and it's
encouraged where the tooling exists. What it may not do is treat its own pass as
the verification. It says what it checked, then says what the user still needs to
look at.

## Decisions before code

Most stages need no input beyond the initial ask — just build them. When a
choice genuinely belongs to the user (stack, data model, UX tradeoff), the agent
asks it in the doc's **Resolved decisions** section, waits, then builds. Never
guess a decision that is the user's to make.

Everything smaller goes to **Residual questions**: state the assumption, flag it
for correction, keep moving. Blocking is for the few choices that are genuinely
the user's — not for every fork in the road.

## Doc-driven, not skill-driven

Project-specific facts (where phase and stage docs live, money/timezone rules,
codegen gates, naming) belong in the **project's own docs**, not in these
skills. Every skill reads the repo's orientation docs first (`AGENTS.md` /
`CLAUDE.md`, the plans hub README, or their equivalents) and treats them as the
source of truth. These skills carry the *method*; the repo carries the
*specifics*.
