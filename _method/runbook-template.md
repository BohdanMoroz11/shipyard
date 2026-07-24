# Rollout-runbook template

A runbook is written **when a stage isn't revert-safe** — see `ship` step 8. It
is not a plan doc and not a design doc. It is an operational script, read during
a deploy by someone who may not have written the change and may be reading it at
2am while something is wrong.

Write for that reader. Short lines, real commands, no design rationale — the
stage doc holds the *why*, this holds the *how* and the *undo*.

## When one is required

Both must be true:

1. The project has **production users** to harm. Pre-MVP, skip it.
2. **`git revert` + redeploy does not fully undo the change.**

Destructive migrations (a revert restores the code, not the dropped rows),
deploy-topology changes, and any cutover with a window where old and new both
run — or neither serves — answer *no* to #2.

Everything additive answers *yes*: no runbook, one line in the stage doc's Prod
risk section, done.

## Where it lives

Next to its stage doc, named for it (`<stage-slug>-rollout.md`). It commits with
the stage's PR and is archived with the stage doc once the rollout holds.

The runbook attaches to the **deploy unit, not the PR**. When a cutover is the
tail of a multi-stage phase, the cutover work is itself a stage — the runbook
belongs to that one.

## Who does what

The agent **writes** it. The **user executes it** — same split as smoke
verification (`vocabulary.md` → Who verifies what). Afterwards the agent amends
it with what actually happened.

## Shape

All four sections are required. Anything beyond them (a gotcha checklist, a file
map, a scale note) is welcome when the change earns it.

```markdown
# <Stage title> — rollout

> **Status: not deployed | deployed (YYYY-MM-DD).** One line on what this
> rollout changes at runtime.

Stage: <link to the stage doc>.

## What changes at runtime

What is running before, and what is running after. Services, queues, containers,
schema, env vars, cron. If old and new coexist for a window, say so here and say
how long.

## Deploy

The ordered steps, as commands. Number them; they get followed literally.

Call out explicitly:
- Anything that must happen at low traffic, and why.
- Any step that is irreversible once started, and the last point at which
  stopping is still clean.
- Migrations: which run before the new code, which after.

## Verification

How you know it worked — the specific checks, in order, with what a healthy
result looks like. Logs to tail, a query to run, an endpoint to hit, a number
that should move. "Looks fine" is not a verification step.

Include the first thing that would show a **silent** failure — a rollout that
half-worked is the dangerous case, not one that crashes.

## Rollback

How to undo it, step by step. Then, plainly: **what cannot be undone.** Dropped
columns, consumed messages, sent emails, external side effects. If the rollback
is only partial, the reader needs to know before they start, not after.

## Known residuals / follow-ups

Filled in **after** the deploy: what actually happened, what differed from the
plan, what is still outstanding. This is the only section with a life beyond the
rollout — residuals become backlog items or the next stage's "Why now".
```

## Rules

- **Four sections minimum, always.** A short runbook is fine; a runbook missing
  its rollback section is not.
- **Commands, not descriptions.** "Restart the workers" is not a step;
  `docker compose up -d --no-deps worker` is.
- **State what can't be undone.** This is the single most valuable line in the
  document.
- **Amend after execution.** An unamended runbook is a plan, not a record.
