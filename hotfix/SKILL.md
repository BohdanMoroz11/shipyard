---
name: hotfix
description: Ship an urgent fix by the fastest route the repo allows — straight to main where that's permitted, otherwise a fast-tracked PR — skipping the stage doc, with a shorter but non-negotiable definition-of-done (regression test, docs if behaviour changed, lint/typecheck). Use only when speed genuinely matters and the change is small and well understood; a small change that isn't urgent is an ordinary stage, not a hotfix. Invoke explicitly.
disable-model-invocation: true
---

# hotfix — urgent fix, fast path

The escape hatch from the `plan-stage → ship` pipeline. Use it **only** when the
fix is urgent, small, and well understood. Anything ambiguous or sizeable is a
`plan-stage` job — hand it back rather than forcing it through here.

First read:
- `../_method/definition-of-done.md` — the hotfix subset still applies
- `../_method/vocabulary.md` → Integration route — direct vs fast-tracked PR

## Workflow

```
- [ ] 1. Confirm this is genuinely a hotfix (small + urgent + clear)
- [ ] 2. Reproduce / pinpoint the bug
- [ ] 3. Write a regression test that fails
- [ ] 4. Fix it; test now passes; suite stays green
- [ ] 5. Update docs if behaviour changed
- [ ] 6. Lint + typecheck; ship by the fastest route the repo allows
```

### 1. Gate

Three conditions, all required: **urgent, small, clear.**

If the fix needs design decisions, touches many files, or you're unsure of the
blast radius — **stop, say so, and tell the user to invoke `plan-stage`**
(`../_method/vocabulary.md` → Redirects). Hotfix trades safety ceremony for
speed; only spend that trade when it's warranted.

**Small but not urgent is not a hotfix** — and this is the gate that matters,
because smallness is what tempts people here. A dependency bump, a copy fix, a
missing index: those are ordinary stages. Their docs come out short on their own
(`../_method/stage-template.md` → "A small stage, in full") and they keep their
PR. What a hotfix buys is skipping **review**, and that is paid for with
*urgency*, never with size. Same redirect: stop, say so, name `plan-stage`.

### 2–4. Fix with a regression test

Pinpoint the cause. Write a test that **fails before the fix** (proves the bug),
then fix it so the test passes and the full suite stays green. A hotfix without
a regression test tends to come back.

### 5–6. Docs, then the fastest route the repo allows

If behaviour changed, update the relevant docs in the same change — a hotfix
skips the ceremony, not the canonical docs. Run the project's lint + typecheck.

Then ship by whichever route the repo permits (run `git` unsandboxed if the repo
requires it):

- **`main` accepts direct commits** → commit straight to it.
- **`main` is protected, or the repo requires PRs** → branch, commit, open the
  PR, wait for CI, and **merge it yourself**. This is the one flow where the
  agent merges without being asked — the whole point is speed, and a hotfix
  parked in an open PR has failed at the only thing it was for.

Check before assuming: the repo's docs, or a failed push, will tell you. **Say
which route you took and why**, so the user knows whether the fix is live or
merged.

## Boundaries

- No stage doc — that's the ceremony being skipped. If you find yourself wanting
  one, it wasn't a hotfix.
- **A branch and PR are not the skipped ceremony.** Where the repo requires
  them, a hotfix is still a PR — just a fast-tracked one that you merge.
- Never skip the regression test to "save time"; it's the one safety net kept.
