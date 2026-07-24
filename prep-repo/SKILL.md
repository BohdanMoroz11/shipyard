---
name: prep-repo
description: Remediate gaps from an audit-repo readiness report so a repository can use the shipyard plan/ship flow. Implements an agreed report (or an explicit subset of gaps); does not re-audit from scratch unless the user asks. Use after audit-repo, or when the user hands you a known list of readiness gaps to fix. Invoke explicitly.
disable-model-invocation: true
---

# prep-repo — remediate readiness gaps

Takes an **agreed readiness report** (from `audit-repo`, or a list of gaps the
user already knows) and implements the fixes. It does not invent a fresh audit
unless the user asks — start from the report.

First read `../_method/vocabulary.md` — including **Redirects**, since this skill
hands work back more than most. If there is no report yet, ask the user to run
`audit-repo` first.

## Workflow

```
- [ ] 1. Load the agreed report (or the user's gap list)
- [ ] 2. Confirm scope — which gaps to fix now
- [ ] 3. Implement — docs/scaffolding and tooling directly; product code as a stage
- [ ] 4. Summarize what changed; note anything still open
```

### 1. Load the report

Use the readiness report from `audit-repo` in this conversation, a report the
user pastes/points at, or an explicit gap list they give you. If none of those
exist, **stop and ask the user to invoke `audit-repo`** before changing anything
(`../_method/vocabulary.md` → Redirects). Don't audit from scratch here.

### 2. Confirm scope

Confirm which gaps to fix in this pass. Don't silently expand into "also tidy
everything else." Facts only the repo owner can decide (money model, timezone,
naming) stay as **open decisions** — propose, don't guess.

The **integration route** is not one of those — it's observable. Record it in
`AGENTS.md` (or the plans hub) as a plain statement: whether `main` takes direct
commits or a hotfix must go through a fast-tracked PR. One line, and `hotfix`
stops guessing.

### 3. Implement — sorted by what the fix touches, not by how big it is

"Small enough to just do" is a judgment call that lands differently every time.
The question that actually matters is **does this change product code?** Three
categories, one sorting question:

**A — docs and scaffolding.** Doc structure, a plans hub, an archive dir with its
provenance README, an `AGENTS.md`, moving a foreign format into the archive,
extracting ADRs out of a plan doc, **recording the repo's integration route**.
Touches no product code and cannot break a build. **Do it, however large it is**
— a fifteen-file migration is still Category A.

**B — tooling and config.** A lint config, a typecheck script, a CI workflow, a
codegen gate. No product logic, but it gates everyone's work and can go red.
**Do it, one commit per item, each verified green before starting the next.**

**C — anything that changes product code.** Writing the missing test suite,
splitting an oversized file, fixing a bug the new typecheck exposed. **Always a
stage** — so it leaves this skill: name it in the summary as work for
`plan-stage`, and don't start it here. No size exception, no "it's only a few
files".

**The one exception**, so that Category B is finishable in a single pass:
errors surfaced by a tooling change *you just made in this prep* — the six lint
violations your new config found — are part of **that** Category B item, fixed
in its commit, when the fix is **mechanical** (a formatter's own output, an
unused import, an explicit `any` the rule wanted annotated) **and the tests stay
green**. The moment a fix needs a judgment call about behaviour, it's Category C
and it stops being your call to make here.

Work the report's gap list **in its order, one gap at a time, reporting between
each**, so a large prep is a sequence the user can stop rather than one mega-diff.

Prefer minimal, convention-matching additions over rewrites.

### 3a. Migrating an existing docs corpus

When the conformance pass found legacy or foreign-format docs, the migration is
**additive and archival only**:

- **Never delete or rewrite a legacy doc.** Move it to the archive and add a
  provenance README saying these explain *why*, not *what is true now*. The
  cost of keeping a stale doc in an archive is near zero; the cost of destroying
  the reasoning behind a decision is permanent.
- **Don't rename history to match the convention.** New docs follow the format;
  old ones keep their names and get archived. Renaming breaks every inbound link
  for no behavioural gain.
- **Extract decisions, don't summarise them.** A decision that passes the ADR
  gate (`../_method/adr-gate.md`) becomes `docs/decisions/NNNN-slug.md` carrying
  its original reasoning and what it rejected. Link back to the source doc.
- **One format going forward.** After the migration, exactly one shape is live.
  Say so in the plans hub so the next agent doesn't copy the archived shape.

### 4. Summarize

Report what was fixed, what was deferred, and whether another `audit-repo`
pass is worth it.

## Boundaries

- Don't fabricate domain content. Scaffold doc *structure* and leave clearly
  marked TODOs for the owner to fill business rules.
- This skill sets a repo up; it doesn't ship features. Once ready, say so — the
  user takes it from there with `plan-stage`.
- Don't start implementing without an agreed gap list — audit first.
- **Never touch product code here.** That's Category C and it's always a stage,
  no matter how mechanical it looks.
