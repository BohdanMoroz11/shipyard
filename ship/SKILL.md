---
name: ship
description: Execute an aligned stage end to end — create a branch, commit the stage doc, then one commit per task, keep tests green, match existing UI/UX, update docs, open a PR, run agent checks (tests/CI), then flag human smoke if needed. Use when a stage is planned and it's time to implement and ship it as one PR. Invoke explicitly.
disable-model-invocation: true
---

# ship — execute a stage → PR → verify

Turns an aligned stage (from `plan-stage`) into a merge-ready PR. For urgent
changes that skip the stage doc and the PR, use `hotfix` instead.

First read:
- `../_method/vocabulary.md` — stage/task, status markers, who verifies what
- `../_method/definition-of-done.md` — the gate before "done"

## Workflow

```
- [ ] 1. Load the stage doc + project conventions
- [ ] 2. Create a branch; commit the stage doc if it isn't on the branch yet
- [ ] 3. Implement one task at a time, committing as you go
- [ ] 4. Keep tests green + match existing UI/UX throughout
- [ ] 5. Update docs for every behaviour change
- [ ] 6. Open a PR
- [ ] 7. Agent checks: suite + CI green; flag human smoke if needed
- [ ] 8. Run the definition-of-done gate
```

### 1. Load context

Read the stage doc — its context and task descriptions are your brief — plus the
repo's conventions (`AGENTS.md`, testing doc, run instructions). The stage's task
list is your task list, and its milestone is what you're aiming at.

### 2–3. Branch, commit the stage doc, then one commit per task

Create a branch. **Commit the stage doc onto the branch** if it isn't already
there — it's part of the stage's deliverable, not a side note. Then work **one
task at a time**: make the change, verify it, commit it right away, and update
its marker in the stage doc. One logical change per commit; no mixed-concern
commits. Run `git` unsandboxed if the repo requires it (check its docs).

If a task's reality diverges from its description, amend the description in
place with a dated note — the doc is the record.

### 4. Tests + UI/UX as you go

Every behaviour change gets a test; keep the full suite green — don't defer it to
the end. For UI, **reuse existing components and patterns** — read a sibling
screen before building. These two slip most often; treat them as non-optional.

### 5. Docs

Reflect every behaviour change in the project's canonical docs (domain docs,
ADRs, the stage doc's status). If the stage belongs to a phase, update its marker
in that phase doc's stage list too — that's the tracking layer, and a stale one
is worse than none. Docs updates are tasks, not afterthoughts.

### 6–7. PR and verify

Open a PR with a summary drawn from the stage doc. Then run **agent checks**
only:
- **CI**: confirm the pipeline is green (e.g. `gh pr checks`). If it fails,
  diagnose and fix with a follow-up commit — don't leave it red.
- **Smoke**: if the change touches a runnable UI/UX (or behaviour) flow, **flag
  that the user should smoke-verify it** and list what to look at. Do **not**
  perform the smoke yourself — that verification belongs to the user (see
  `vocabulary.md` → Who verifies what).

### 8. Definition of done

Walk `../_method/definition-of-done.md` explicitly before calling the stage done,
including whether the milestone actually holds. Report anything still open
(including pending human smoke).

## Boundaries

- Don't merge unless the user asks; leave the PR ready and CI green.
- If mid-stage you discover it's really two stages, say so and split rather than
  ballooning one PR.
