# shipyard

A portable, doc-driven set of [Cursor Agent Skills](https://docs.cursor.com)
that encode one opinionated development flow: **plan a change, align on it, ship
it as a reviewable PR, verify it** — with an escape hatch for urgent fixes.

The skills carry the *method*. Each project carries its own *specifics* (money
model, timezone, naming, commands) in its own docs. The skills read those docs
at runtime, so the same skillset works across every repo.

## The flow

| Level | Size | Skill |
| --- | --- | --- |
| **Phase / initiative** | many sessions, vague goal | [`plan-roadmap`](plan-roadmap/SKILL.md) |
| **Stage** | one PR, one session | [`plan`](plan/SKILL.md) → [`ship`](ship/SKILL.md) |
| **Task** | one commit | (a slice within `ship`) |
| **Urgent** | straight to main | [`hotfix`](hotfix/SKILL.md) |
| **Onboarding** | make a repo ready | [`prep-repo`](prep-repo/SKILL.md) |

Shared vocabulary and templates live in [`_method/`](_method/) — plain markdown
the skills link to (not skills themselves):

- [`_method/vocabulary.md`](_method/vocabulary.md) — phase/stage/task + modes
- [`_method/plan-template.md`](_method/plan-template.md) — plan-doc shape
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
for d in prep-repo plan plan-roadmap ship hotfix _method; do
  ln -s /absolute/path/to/shipyard/$d ~/.cursor/skills/$d
done
```

Then restart Cursor (or reload skills) and invoke by name, e.g. `plan`.

## Usage

All skills are **explicitly invoked** (they don't auto-fire). Typical session:

1. New/existing repo not set up yet → `prep-repo`.
2. Big multi-session goal → `plan-roadmap`, then per stage:
3. `plan` — align on the stage, write the plan doc.
4. `ship` — branch, commit slices, tests, docs, PR, verify.
5. Urgent one-off → `hotfix`.
