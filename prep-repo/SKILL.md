---
name: prep-repo
description: Remediate gaps from an audit-repo readiness report so a repository can use the shipyard plan/ship flow. Implements an agreed report (or an explicit subset of gaps); does not re-audit from scratch unless the user asks. Use after audit-repo, or when the user hands you a known list of readiness gaps to fix. Invoke explicitly.
disable-model-invocation: true
---

# prep-repo — remediate readiness gaps

Takes an **agreed readiness report** (from `audit-repo`, or a list of gaps the
user already knows) and implements the fixes. It does not invent a fresh audit
unless the user asks — start from the report.

First read `../_method/vocabulary.md`. If there is no report yet, suggest
`audit-repo` first.

## Workflow

```
- [ ] 1. Load the agreed report (or the user's gap list)
- [ ] 2. Confirm scope — which gaps to fix now
- [ ] 3. Implement (or hand large work to plan/ship)
- [ ] 4. Summarize what changed; note anything still open
```

### 1. Load the report

Use the readiness report from `audit-repo` in this conversation, a report the
user pastes/points at, or an explicit gap list they give you. If none of those
exist, run `audit-repo` (or ask the user to) before changing anything.

### 2. Confirm scope

Confirm which gaps to fix in this pass. Don't silently expand into "also tidy
everything else." Facts only the repo owner can decide (money model, timezone,
naming) stay as **open decisions** — propose, don't guess.

### 3. Implement

Prefer minimal, convention-matching additions over rewrites.

- **Small remediation** — apply the fixes directly (scaffold doc structure,
  wire commands, add missing stubs with clear TODOs).
- **Large remediation** — if it spans many files or needs collaborative
  decisions, create a stage plan (use `../_method/plan-template.md`) and drive
  it through `plan` / `ship` instead of dumping a mega-diff here.

### 4. Summarize

Report what was fixed, what was deferred, and whether another `audit-repo`
pass is worth it.

## Boundaries

- Don't fabricate domain content. Scaffold doc *structure* and leave clearly
  marked TODOs for the owner to fill business rules.
- This skill sets a repo up; it doesn't ship features. Once ready, use `plan`.
- Don't start implementing without an agreed gap list — audit first.
