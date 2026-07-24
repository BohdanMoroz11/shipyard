# shipyard

A portable, doc-driven set of [Cursor Agent Skills](https://docs.cursor.com)
that encode one opinionated development flow: **plan a change, align on it, ship
it as a reviewable PR, distil what it changed into the canonical docs, run agent
checks, then human smoke** — with an escape hatch for urgent fixes.

The skills carry the *method*. Each project carries its own *specifics* (money
model, timezone, naming, commands) in its own docs. The skills read those docs
at runtime, so the same skillset works across every repo.

Everything here is uniform on purpose. A stage doc has ten sections, always,
under the same names; a task list never has checkboxes; a decision is either an
ADR or it isn't. The value isn't in any single rule — it's that two projects
audited a year apart read the same way.

## The flow

| Level | Size | Skill |
| --- | --- | --- |
| **Phase** | many sessions, vague goal | [`plan-phase`](plan-phase/SKILL.md) |
| **Stage** | one PR, one session, 1–n requests | [`plan-stage`](plan-stage/SKILL.md) → [`ship`](ship/SKILL.md) |
| **Task** | one commit | (a task within `ship`) |
| **Urgent** | straight to main | [`hotfix`](hotfix/SKILL.md) |
| **Onboarding** | audit readiness + conformance, then remediate | [`audit-repo`](audit-repo/SKILL.md) → [`prep-repo`](prep-repo/SKILL.md) |

A stage doesn't need a phase — most work is one standalone stage, tracked from
the project's plans hub.

Shared vocabulary and templates live in [`_method/`](_method/) — plain markdown
the skills link to (not skills themselves):

- [`_method/vocabulary.md`](_method/vocabulary.md) — phase/stage/task, status markers, the three doc layers, who verifies what
- [`_method/stage-template.md`](_method/stage-template.md) — stage-doc shape (ten required sections)
- [`_method/phase-template.md`](_method/phase-template.md) — phase-doc shape
- [`_method/runbook-template.md`](_method/runbook-template.md) — rollout runbook, for changes a revert can't undo
- [`_method/definition-of-done.md`](_method/definition-of-done.md) — the gate

## Install

Cursor loads personal skills from `~/.cursor/skills/`. Symlink this repo so the
relative links between skills and `_method/` keep resolving.

If `~/.cursor/skills` doesn't exist yet, point it at this repo:

```bash
ln -s /absolute/path/to/shipyard ~/.cursor/skills
```

If you already have skills there, symlink each skill dir and `_method`:

```bash
mkdir -p ~/.cursor/skills
for d in audit-repo prep-repo plan-stage plan-phase ship hotfix _method; do
  ln -s /absolute/path/to/shipyard/$d ~/.cursor/skills/$d
done
```

Then restart Cursor (or reload skills) and invoke by name, e.g. `plan-stage`.

## Usage

All skills are **explicitly invoked** (they don't auto-fire). Typical session:

1. New/existing repo not set up yet → `audit-repo` (readiness + how its existing
   docs map to the format), then `prep-repo` (fix gaps, migrate legacy docs into
   the archive).
2. Big multi-session goal → `plan-phase` first. Otherwise go straight to:
3. `plan-stage` — align on the stage, write the stage doc.
4. `ship` — branch, commit the stage doc then one commit per task, tests, docs
   distilled (domain doc always, ADR only when it binds future work), PR, agent
   checks, runbook if a revert can't undo it, stage doc archived; you smoke and
   you deploy.
5. Urgent one-off → `hotfix`.
