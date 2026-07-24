# Definition of done

The gate a stage (or hotfix) must pass before it's "done". Shared by `ship` and
`hotfix`. Treat it as a checklist, not a suggestion — these are the things that
otherwise get forgotten.

Copy and tick these off before declaring completion:

```
- [ ] Tests green — new behaviour has tests; the full suite passes locally.
- [ ] UI/UX matches existing patterns — reuse the app's components, spacing,
      and interaction conventions; don't invent a new style. Read a sibling
      screen/component first.
- [ ] Docs updated — every behaviour change is reflected in the project's
      canonical docs (domain docs, ADRs, plan doc). This is the one that slips
      most; do it in the same stage.
- [ ] Lint + typecheck pass — run the project's configured commands.
- [ ] Generated artifacts in sync — if the repo has codegen (API client,
      schema, etc.), regenerate and commit it together with the source change.
- [ ] One commit per logical change — no mixed-concern commits.
- [ ] CI green — after the PR opens, the pipeline passes. Investigate and fix
      failures rather than ignoring them.
- [ ] Smoke flagged for the user — if the change affects a runnable UI/UX (or
      behaviour) flow, call out that **human smoke verification** is needed and
      what to check. Do not perform the smoke yourself; the user owns it.
```

## Hotfix subset

A hotfix skips the PR/CI ceremony but still owes:

```
- [ ] Tests green (at least a regression test for the bug being fixed).
- [ ] Docs updated if behaviour changed.
- [ ] Lint + typecheck pass.
```

## Where the specifics live

The *commands* to run tests/lint/typecheck/codegen are project-specific — read
them from the repo's docs (`AGENTS.md`, `README.md`, `docs/testing.md`, or
equivalents). Never hardcode them here.
