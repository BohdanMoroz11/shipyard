---
name: ship
description: Execute an aligned stage end to end — create a branch, commit the stage doc, then one commit per task, keep tests green, match existing UI/UX, distil docs (domain doc, ADR when warranted), open a PR, run agent checks, write a rollout runbook when the change isn't revert-safe, then hand off smoke. Use when a stage is planned and it's time to implement and ship it as one PR. Invoke explicitly.
disable-model-invocation: true
---

# ship — execute a stage → PR → verify

Turns an aligned stage (from `plan-stage`) into a merge-ready PR. Urgent changes
that skip the stage doc are `hotfix`'s job, not a mode of this skill.

First read:
- `../_method/vocabulary.md` — stage/task, status markers, who verifies what
- `../_method/definition-of-done.md` — the gate before "done"

## Workflow

```
- [ ] 1. Load the stage doc + project conventions
- [ ] 2. Create a branch; commit the stage doc if it isn't on the branch yet
- [ ] 3. Implement one task at a time, committing as you go
- [ ] 4. Keep tests green + match existing UI/UX throughout
- [ ] 5. Distil the docs: domain doc always, ADR only if it binds future work
- [ ] 6. Open a PR
- [ ] 7. Agent checks: suite + CI green; hand off smoke
- [ ] 8. Rollout runbook, if the stage isn't revert-safe
- [ ] 9. Archive the stage doc; run the definition-of-done gate
```

### 1. Load context

Read the stage doc — its context and task descriptions are your brief — plus the
repo's conventions (`AGENTS.md`, testing doc, run instructions). The stage's task
list is your task list, and its milestone is what you're aiming at. Note its
**Prod risk** answer now; step 8 depends on it.

### 2–3. Branch, commit the stage doc, then one commit per task

Create a branch. **Commit the stage doc onto the branch** if it isn't already
there — it's part of the stage's deliverable, not a side note. Then work **one
task at a time**: make the change, verify it, commit it right away. One logical
change per commit; no mixed-concern commits. Run `git` unsandboxed if the repo
requires it (check its docs).

Don't tick task markers — there aren't any (`vocabulary.md` → Status markers).
What you *do* keep current is the status line, and any task description whose
reality diverged — amend those in place with a dated note. The doc is the record.

### 4. Tests + UI/UX as you go

Every behaviour change gets a test; keep the full suite green — don't defer it to
the end. For UI, **reuse existing components and patterns** — read a sibling
screen before building. These two slip most often; treat them as non-optional.

### 5. Distil the docs

The stage doc is **provenance** — why it was built this way. It is not where
current behaviour lives. Three separate gates, all in this stage's commits:

**Domain doc — always, if behaviour changed.** State the new behaviour in the
present tense, as though it had always been true. A domain doc that reads like a
changelog ("we now also…") has failed. If no domain doc covers this surface yet,
create one.

**ADR — only when it binds future work.** Run the gate in
`../_method/adr-gate.md` — two questions, both must be yes — and write the ADR
into this stage's commits if it passes. Most stages produce none, and that's the
expected outcome, not a miss. Don't re-derive the bar from memory; read the file.

**Phase marker — if this stage has a phase.** Update its marker in that phase
doc's stage list. That's the tracking layer, and a stale one is worse than none.

### 6–7. PR and verify

Open a PR with a summary drawn from the stage doc. Then run **agent checks**:

- **CI**: confirm the pipeline is green (e.g. `gh pr checks`). If it fails,
  diagnose and fix with a follow-up commit — don't leave it red.
- **Smoke handoff**: if the change touches a runnable UI/UX or behaviour flow,
  tell the user smoke verification is needed and **list exactly what to look at**.
  Drive the app yourself if you can — then say what you checked and what still
  needs their eyes. The sign-off is theirs, never yours (`vocabulary.md` → Who
  verifies what).

### 8. Rollout runbook — only when the stage isn't revert-safe

Re-answer the stage doc's **Prod risk** question against the real diff — planning
answered it against an intention, and diffs drift.

**Yes, or the project has no production users** → no runbook. Say so and move on.

**No** → write one, using `../_method/runbook-template.md`. Destructive
migrations, deploy-topology changes, and cutovers all answer no. It goes next to
the stage doc, commits with the PR, and **you do not execute it** — the deploy is
the user's, like the smoke. After they run it, amend it with what actually
happened and what's left over.

### 9. Archive and gate

Once the stage has shipped, move its doc (and its runbook, if any) to the plans
archive and update the link in the plans hub. The archive carries a README saying
these explain *why*, not *what is true now* — create one if the repo lacks it.

Then walk `../_method/definition-of-done.md` explicitly, including whether the
milestone actually holds. Report anything still open — pending smoke, pending
deploy, an amendment the user still owes.

## Boundaries

- Don't merge unless the user asks; leave the PR ready and CI green. (`hotfix`
  is the one exception — see its step 6.)
- Don't deploy. Write the runbook; the user runs it.
- If mid-stage you discover it's really two stages, **stop and say so** rather
  than ballooning one PR — the user decides whether to split, and re-planning is
  `plan-stage`'s job, not yours (`../_method/vocabulary.md` → Redirects). Two
  *requests* in one stage is normal; two PRs' worth of work is not.
