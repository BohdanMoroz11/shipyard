# Stage-doc template

A stage doc is a **living document**: forward-looking pre-work before the PR, and
the historical record of *why* it was built this way afterwards. It is the
primary place this stage's context lives — write it for the agent that picks the
work up next session with no memory of this conversation.

Every stage gets **its own file** — either in a phase's dir alongside that
phase's other stages, or directly in the plans dir when the stage stands alone
(see `vocabulary.md` → One doc per stage). Never as a section of the phase doc.

## Every section is required

The shape below is not a menu. **Write every section, under these names, in this
order**, even when a section is one line — `Prod risk: none, additive and
revert-safe` is a complete and useful answer. The point of a fixed shape is that
any stage doc in any repo can be read the same way, and that the sections nobody
enjoys writing (Prod risk, Non-goals) don't quietly disappear.

If a section is genuinely empty, say so in it. Don't delete the heading.

**There is no short version of this shape**, and a small change doesn't need one
— on a one-commit stage half the sections are honestly one line each, and the
doc lands at thirty lines without dropping anything (see "A small stage, in
full" below). A change too small to be worth those thirty lines is not a
different tier of stage; it's either still a stage, or it's urgent and it's a
`hotfix`. Nothing else exists.

Match the project's file naming and link conventions if it has them (read a
recent stage doc first). The section list is not negotiable per-project; the
naming and location of the file is.

## Shape

```markdown
# <Stage title>

> **Status: planning | in progress | shipped (YYYY-MM-DD).** One line on what
> this stage is.

Part of: <link to the phase doc, or "standalone — no phase">.
Canonical behaviour: <link to the domain doc(s) this touches>.
Rationale: <link to ADR, if a notable decision was made>.

## Goal

What is true when this stage is done, in two or three sentences. The outcome,
not the implementation.

## Why now

What makes this the next thing — the client asked, it blocks a later stage, it's
bleeding in production, a dependency just landed. One paragraph. A stage that
can't answer this probably belongs in the backlog.

## Context (as of YYYY-MM-DD)

What exists today that this stage builds on, with real paths and `file:line`
citations: the modules involved, the current behaviour, what's already
half-built, and the surprises worth knowing (a schema field that exists but is
unused, a helper that already accepts the param nobody passes). Name the gap
that makes the work necessary or hard.

Date the heading. Context goes stale; a dated section says so honestly instead
of misleading the next reader.

This is the section that saves the next session an hour of code reading. Be
specific and generous — it is normal for it to be the longest section in the doc.

## R1 — <request title>

One section per request this stage carries, numbered `R1`, `R2`, … — always,
even when there is only one. Each holds that request's ask, its design, and the
surfaces it touches.

A stage is one PR and one session; it is **not** limited to one theme. Real work
arrives as a handful of related-enough asks, and shipping them as one reviewable
PR is correct. Numbering them keeps a five-request PR legible.

Quote or paraphrase the ask itself before the design — keep the original intent
visible rather than laundering it into implementation-speak.

## R2 — <request title>

<same>

## Tasks

One logical change each, in order. Numbered — **no checkboxes** (see
`vocabulary.md` → Status markers). This is the plan of record for how the work
splits into commits; it is written before the work and amended when reality
diverges, not maintained as live progress state.

Each task names what changes, **which files and modules**, the shapes and names
involved, the constraints it must respect, the docs or ADRs it implements, and
the end state that proves it works. Several sentences to a paragraph is normal;
a one-line task is a bug unless the task is genuinely trivial. The file paths in
these descriptions are what a reader uses to find the work — don't summarise
them away.

1. **<slug>** — <full description>
2. **<slug>** — <full description>
3. **docs** — <which docs change and how>

Docs are always a task, never an afterthought.

## Milestone

The one concrete, demoable thing that is true when this stage is done — phrased
so it can be checked, not felt. If the project calls this "exit criteria", it is
the same thing under a different name; use "Milestone".

## Resolved decisions (YYYY-MM-DD)

The questions that needed the user's answer, and what was chosen. During
planning this section holds the open questions and blocks the work; once
answered it becomes the record, and the design in the R-sections reflects it.

| # | Question | Choice |
| --- | --- | --- |
| 1 | <question> | <what was chosen, and the reason if it isn't obvious> |
| 2 | <question> | <choice> |

Only questions that genuinely belong to the user — stack, data model, UX
tradeoff, anything ambiguous in the ask. Most stages have a handful at most.

## Residual questions

Smaller things settled by assumption rather than by asking, so the work isn't
blocked on them. State the assumption and invite correction — "assumed yes;
confirm during build if this feels wrong". This is the pressure valve that keeps
the Resolved-decisions gate from stalling the stage over trivia.

1. **<assumption>** — <what was assumed and why it's probably right>

## Non-goals

What a reader might reasonably expect here but won't find, and where it went — a
later stage, the backlog, or deliberately never.

## Prod risk

What could break in production and what the blast radius is. Then the question
that decides whether this stage needs a rollout runbook:

> **Does `git revert` + redeploy fully undo this?**

Answer it explicitly. Destructive migrations, deploy-topology changes, and
cutovers answer *no* — those need a runbook. Additive features answer *yes* and
this section is one line.
```

## A small stage, in full

The smallest realistic stage — a dependency bump — with **all ten sections**.
This is what "one line beats a missing heading" looks like in practice, and it's
why there's no short version of the shape: six of these sections are a single
line, and the whole doc is thirty lines.

```markdown
# Bump `csv-stringify` to 6.5.2

> **Status: shipped (2026-07-19).** Patch bump clearing the CVE on the export
> path.

Part of: standalone — no phase.
Canonical behaviour: [netsuite-csv.md](../integrations/netsuite-csv.md).

## Goal

`csv-stringify` is on 6.5.2, the advisory is off the dependency audit, and
exports behave exactly as before.

## Why now

The advisory fails the weekly audit job, so every unrelated PR now opens with a
red check. Cheap to clear.

## Context (as of 2026-07-19)

Pinned at 6.4.0 in `package.json`; the only consumer is the streaming export in
`src/routes/export.ts:41`. 6.5.x changes no API we touch — the release notes
list a quoting fix for embedded CRLF, which `test/export.spec.ts:88` already
covers.

## R1 — clear the advisory

Bump to the patched line. No behaviour change intended; the existing export
fixtures are the contract.

## Tasks

1. **bump** — `csv-stringify` to `6.5.2` in `package.json` + lockfile.
   `test/export.spec.ts` stays green **unchanged** — that it needed no edits is
   the evidence the bump is behaviour-neutral. End state: audit clean.

## Milestone

The dependency audit passes on `main` and a seed-tenant export byte-matches the
pre-bump fixture.

## Resolved decisions (2026-07-19)

None — nothing here was the user's to decide.

## Residual questions

None.

## Non-goals

The other four advisories in the audit. They're on transitive dev-only deps and
go in the backlog, not here.

## Prod risk

None. Patch bump on one code path, covered by existing fixtures. `git revert` +
redeploy fully undoes it — no runbook.
```

Nothing was dropped, and nothing was padded. **Non-goals** did real work — it's
the section that stops this stage quietly becoming "fix all the advisories" —
and it's exactly the section a short version would have cut first.

## A task description, sized right

Too thin — the next session has to re-derive everything:

```markdown
1. **api** — add the export endpoint
```

Right — the description *is* the context:

```markdown
1. **s4-exports** — `POST /export` streaming CSV with the 37 NetSuite columns
   (verbatim field order from `LoL-main/exports.py`) and the
   OR-of-quick-pay/direct-payment row filter. Stream through `csv-stringify` so
   a full-year export can't OOM the pod; the endpoint must stay inside the
   tenant-scoped Prisma extension. Column contract and the filter rationale:
   [netsuite-csv.md](../integrations/netsuite-csv.md). End state: an export of
   the seed tenant's paid loads imports into NetSuite with zero rejected rows.
```

When reality diverges later, amend in place with a dated note rather than
rewriting:

```markdown
1. **s4-exports** — ... End state: ... **2026-05-22:** the brokerage
   multi-select moved to the loads page filter bar instead; the export modal now
   reads its filters from the URL state, so the two surfaces can't drift.
```

## Rules

- **Every section, every time.** One line beats a missing heading, and on a
  small stage most sections *are* one line.
- **Decisions come before code.** Ask the few that are the user's, assume the
  rest into Residual questions, and don't guess a choice that isn't yours.
- **Tasks are numbered, not checked** — `vocabulary.md` → Status markers.
- **Descriptions are long on purpose.** They're the context store, not a
  checklist label.
- **Keep the status line current.** It is the one piece of live state in the doc.
- **Amend, don't rewrite.** When reality diverges, add a dated note in place —
  the doc is the record.
