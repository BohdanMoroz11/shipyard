---
name: hotfix
description: Ship an urgent fix straight to main, skipping the plan-doc and PR ceremony, with a shorter but non-negotiable definition-of-done (regression test, docs if behaviour changed, lint/typecheck). Use only when speed genuinely matters and the change is small and well understood. Invoke explicitly.
disable-model-invocation: true
---

# hotfix — urgent fix, fast path

The escape hatch from the `plan → ship` pipeline. Use it **only** when the fix
is urgent, small, and well understood. Anything ambiguous or sizeable goes
through `plan` instead.

First read:
- `../_method/definition-of-done.md` — the hotfix subset still applies

## Workflow

```
- [ ] 1. Confirm this is genuinely a hotfix (small + urgent + clear)
- [ ] 2. Reproduce / pinpoint the bug
- [ ] 3. Write a regression test that fails
- [ ] 4. Fix it; test now passes; suite stays green
- [ ] 5. Update docs if behaviour changed
- [ ] 6. Lint + typecheck; commit to main
```

### 1. Gate

If the fix needs design decisions, touches many files, or you're unsure of the
blast radius — **stop and use `plan`**. Hotfix trades safety ceremony for
speed; only spend that trade when it's warranted.

### 2–4. Fix with a regression test

Pinpoint the cause. Write a test that **fails before the fix** (proves the bug),
then fix it so the test passes and the full suite stays green. A hotfix without
a regression test tends to come back.

### 5–6. Docs + commit

If behaviour changed, update the relevant docs in the same change. Run the
project's lint + typecheck, then commit **straight to `main`** (run `git`
unsandboxed if the repo requires it). No PR unless the repo enforces one.

## Boundaries

- No plan doc, no branch/PR — that's the whole point. If you find yourself
  wanting them, it wasn't a hotfix.
- Never skip the regression test to "save time"; it's the one safety net kept.
