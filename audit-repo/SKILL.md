---
name: audit-repo
description: Audit whether a repository is ready for the shipyard plan/ship flow (docs structure, tests, lint, typecheck, CI, git/PR hygiene, codegen sync, local-run instructions) and produce a readiness report. Does not remediate — stop after the report. Use when onboarding a project or asking whether a repo is "ready for the flow". Invoke explicitly.
disable-model-invocation: true
---

# audit-repo — readiness report only

The other skills (`plan-stage`, `ship`, `hotfix`) are **doc-driven**: they read a
repo's docs to learn its conventions. This skill checks whether those docs —
and the surrounding tooling — actually exist, then **stops**. Remediation is
`prep-repo`.

First read `../_method/vocabulary.md`.

## Workflow

```
- [ ] 1. Audit against the readiness checklist
- [ ] 2. Audit conformance — how the existing docs map to the format
- [ ] 3. Write a readiness report (present / partial / missing per item)
- [ ] 4. Propose a remediation order — then STOP
```

### 1. Readiness checklist

Inspect the repo and grade each item **present / partial / missing**:

- **Agent entry doc** — `AGENTS.md`/`CLAUDE.md` that orients an agent and links
  to canonical docs.
- **Docs tree** — a `docs/` (or equivalent) with: a **plans hub** (README
  describing the working model + a backlog), **domain docs**, and a
  **decisions/ADR** dir.
- **Plans dir + naming** — a place phase and stage docs live, with a naming
  convention and a dir-per-phase layout (one file per stage, never tasks in the
  phase doc).
- **Plans archive** — a place shipped stage docs go, with a README saying they
  are provenance and not current behaviour. Without it, plan docs and canonical
  docs compete to be the source of truth.
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
  human smoke).

### 2. Conformance pass

Readiness asks *does the tooling exist*. Conformance asks *does the existing
documentation already match the format, and where doesn't it* — the harder half
on a repo with real history. A mature project usually has plan docs; they are
just under other names, in other shapes, sometimes in two competing formats at
once.

Inventory every planning-ish doc in the repo and classify each:

- **Already a stage doc** — a session/PR-sized plan under a different name
  (`release-N.md`, `batch-N.md`, `<feature>-plan.md`). New work uses the format;
  this one is history.
- **Already a phase doc** — a multi-stage overview. Same.
- **A foreign format** — imported from another tool or workflow (a plugin's
  plan/spec pairs, a generated task list). Note the count and where it came from.
- **Decisions with no home** — reasoning trapped inside a plan doc that will
  become invisible the moment that doc is archived. List the ones that would pass
  the ADR gate (`ship` step 5); these are the highest-value extraction.
- **Canonical-layer gaps** — no domain docs, no decisions dir, no docs index.

Report the mapping, not a verdict on quality. Legacy docs are provenance and stay
that way; the question is only what the *next* doc should look like and what is
worth extracting.

### 3. Readiness report

Present a table (item / status / gap). Lead with a one-line verdict: **ready**,
**ready with gaps**, or **not ready**. Follow it with the conformance mapping.

### 4. Propose remediation order — then STOP

List the changes needed, ordered by what unblocks the flow first (docs
structure usually comes before CI). Include enough detail that `prep-repo` (or
the user) can act on the report without re-auditing.

**Do not implement.** Hand the report back and wait. If the user wants the gaps
fixed, they invoke `prep-repo` with this report (or an agreed subset).

## Boundaries

- Report only. No scaffolding, no config edits, no commits.
- If the verdict is already **ready**, say so and stop — nothing to prep.
