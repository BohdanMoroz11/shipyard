# Definition of done

The gate a stage (or hotfix) must pass before it's "done". Shared by `ship` and
`hotfix`. Treat it as a checklist, not a suggestion — these are the things that
otherwise get forgotten.

Copy and tick these off before declaring completion. They are **transient
session state** — a scratch checklist you work through and report from. They
never land in a committed doc, which carries no task markers at all
(`vocabulary.md` → Status markers).

```
- [ ] Tests green — new behaviour has tests; the full suite passes locally.
- [ ] UI/UX matches existing patterns — reuse the app's components, spacing,
      and interaction conventions; don't invent a new style. Read a sibling
      screen/component first.
- [ ] Domain doc distilled — behaviour changes are stated in the project's
      canonical domain doc in the present tense, as though always true. Not a
      changelog entry, not "we now also…". This is the gate that slips most.
- [ ] ADR written, or consciously not — run the gate in `adr-gate.md`. Most
      stages produce none; that's the expected answer, not a miss.
- [ ] Stage doc archived — moved to the plans archive with its link updated in
      the plans hub, once the stage has shipped.
- [ ] Lint + typecheck pass — run the project's configured commands.
- [ ] Generated artifacts in sync — if the repo has codegen (API client,
      schema, etc.), regenerate and commit it together with the source change.
- [ ] One commit per task — one logical change each, no mixed-concern commits.
      Where the work diverged from a task description, the description carries a
      dated amendment. The phase doc's marker for this stage is current (stage
      docs carry no task markers — see `vocabulary.md` → Status markers).
- [ ] Milestone holds — the stage's stated milestone is actually true, not
      approximately true.
- [ ] CI green — after the PR opens, the pipeline passes. Investigate and fix
      failures rather than ignoring them.
- [ ] Rollout runbook — if `git revert` + redeploy does **not** fully undo this
      change and the project has production users, a runbook exists
      (`../_method/runbook-template.md`). Otherwise say explicitly that the
      change is revert-safe.
- [ ] Smoke handed off — if the change affects a runnable UI/UX (or behaviour)
      flow, tell the user smoke verification is needed and exactly what to check.
      Driving the app to check your own work is fine; treating that as the
      sign-off is not. The smoke is the user's.
```

## Hotfix subset

A hotfix skips the planning ceremony but still owes:

```
- [ ] Tests green (at least a regression test for the bug being fixed).
- [ ] Domain doc updated if behaviour changed — same gate as a stage, in the
      same commit. A hotfix skips the ceremony, not the canonical docs.
- [ ] Lint + typecheck pass.
- [ ] CI green — only if the repo forced a PR (see `vocabulary.md` →
      Integration route). Merging a red hotfix PR is worse than the bug.
- [ ] Route reported — the user is told whether the fix went direct to `main`
      or through a merged PR.
```

This is the **hotfix** subset, not the small-change subset. A small non-urgent
change is an ordinary stage and owes the full list above.

## Where the specifics live

The *commands* to run tests/lint/typecheck/codegen are project-specific — read
them from the repo's docs (`AGENTS.md`, `README.md`, `docs/testing.md`, or
equivalents). Never hardcode them here.
