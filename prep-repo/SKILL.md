---
name: prep-repo
description: Audit whether a repository is ready for the shipyard plan/ship flow (docs structure, tests, lint, typecheck, CI, git/PR hygiene, codegen sync, local-run instructions) and remediate the gaps. Use when onboarding a new or existing project to this workflow, or when the user asks whether a repo is "ready for the flow". Invoke explicitly.
disable-model-invocation: true
---

# prep-repo — make a repo ready for the flow

The other skills (`plan`, `ship`, `hotfix`) are **doc-driven**: they read a
repo's docs to learn its conventions. This skill makes sure those docs — and
the surrounding tooling — actually exist. First read `../_method/vocabulary.md`.

## Workflow

```
- [ ] 1. Audit against the readiness checklist
- [ ] 2. Write a readiness report (present / partial / missing per item)
- [ ] 3. Propose remediation; if large, create a temp plan doc
- [ ] 4. Implement the fixes (hand off to `plan`/`ship` if it's a big stage)
```

### 1. Readiness checklist

Inspect the repo and grade each item **present / partial / missing**:

- **Agent entry doc** — `AGENTS.md`/`CLAUDE.md` that orients an agent and links
  to canonical docs.
- **Docs tree** — a `docs/` (or equivalent) with: a **plans hub** (README
  describing the working model + a backlog), **domain docs**, and a
  **decisions/ADR** dir.
- **Plans dir + naming** — a place plan docs live and a naming convention.
- **Lint + format** — configured, with a single command to run.
- **Typecheck / build** — a command that fails on type errors.
- **Tests** — a runner, a conventions doc, a command; ideally an expectation of
  what must be covered.
- **CI** — runs on PRs; gates lint + typecheck + test; queryable (e.g. `gh`
  installed and authed).
- **Git / PR hygiene** — can branch and open PRs; a commit-message convention;
  optional PR template.
- **Generated-artifact sync** — if codegen exists, a documented "regenerate +
  commit together" gate.
- **Local run instructions** — a README section on running locally (needed for
  smoke checks).

### 2. Readiness report

Present a table (item / status / gap). Lead with a one-line verdict: **ready**,
**ready with gaps**, or **not ready**.

### 3. Propose remediation

List the changes needed, ordered by what unblocks the flow first (docs
structure usually comes before CI). If remediation is small, just do it. If it
spans many files or decisions, create a **temporary remediation plan doc** in
the project's plans dir (use `../_method/plan-template.md`) and drive it through
`plan` / `ship`.

### 4. Implement

Apply the fixes. Prefer minimal, convention-matching additions over rewrites.
Never invent project quirks — put facts the repo owner must decide (money model,
timezone, naming) as **open questions**, don't guess them.

## Boundaries

- Don't fabricate domain content. Scaffold doc *structure* and leave clearly
  marked TODOs for the owner to fill business rules.
- This skill sets a repo up; it doesn't ship features. Once ready, use `plan`.
