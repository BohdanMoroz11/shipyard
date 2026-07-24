---
name: ship
description: Execute an aligned stage plan end to end — create a branch, commit the plan + each logical slice, keep tests green, match existing UI/UX, update docs, open a PR, run agent checks (tests/CI), then flag human smoke if needed. Use when a plan is agreed and it's time to implement and ship a scoped change (one PR). Invoke explicitly.
disable-model-invocation: true
---

# ship — execute a stage → PR → verify

Turns an aligned plan (from `plan`) into a merged-ready PR. For urgent changes
that skip the plan/PR ceremony, use `hotfix` instead.

First read:
- `../_method/vocabulary.md` — stage/task, the two modes, verification split
- `../_method/definition-of-done.md` — the gate before "done"

## Workflow

```
- [ ] 1. Load the plan + project conventions
- [ ] 2. Create a branch; commit the plan doc if it isn't on the branch yet
- [ ] 3. Implement one commit slice at a time, committing as you go
- [ ] 4. Keep tests green + match existing UI/UX throughout
- [ ] 5. Update docs for every behaviour change
- [ ] 6. Open a PR
- [ ] 7. Agent checks: suite + CI green; flag human smoke if needed
- [ ] 8. Run the definition-of-done gate
```

### 1. Load context

Read the stage's plan doc and the repo's conventions (`AGENTS.md`, testing doc,
run instructions). The plan's commit slices are your task list.

### 2–3. Branch, commit the plan, then commit per slice

Create a branch. **Commit the plan doc onto the branch** if it isn't already
there (the plan is part of the stage's deliverable, not a side note). Then
implement **one commit slice at a time** — make the change, verify it, commit
it right away. One logical change per commit; no mixed-concern commits. Run
`git` unsandboxed if the repo requires it (check its docs).

### 4. Tests + UI/UX as you go

Every behaviour change gets a test; keep the full suite green — don't defer it
to the end. For UI, **reuse existing components and patterns** — read a sibling
screen before building. These two slip most often; treat them as non-optional.

### 5. Docs

Reflect every behaviour change in the project's canonical docs (domain docs,
ADRs, the plan doc's status). Docs updates are commit slices, not afterthoughts.

### 6–7. PR and verify

Open a PR with a summary drawn from the plan. Then run **agent checks** only:
- **CI**: confirm the pipeline is green (e.g. `gh pr checks`). If it fails,
  diagnose and fix with a follow-up commit — don't leave it red.
- **Smoke**: if the change touches a runnable UI/UX (or behaviour) flow, **flag
  that the user should smoke-verify it** and list what to look at. Do **not**
  perform the smoke yourself — that verification belongs to the user (see
  `vocabulary.md` → Verification split).

### 8. Definition of done

Walk `../_method/definition-of-done.md` explicitly before calling the stage
done. Report anything still open (including pending human smoke).

## Boundaries

- Don't merge unless the user asks; leave the PR ready and CI green.
- If mid-stage you discover it's really two stages, say so and split rather than
  ballooning one PR.
