# shipyard

A portable, doc-driven set of agent skills — plain markdown, no tool-specific
instructions — that encode one opinionated development flow: **plan a change,
align on it, ship it as a reviewable PR, distil what it changed into the
canonical docs, run agent checks, then human smoke** — with an escape hatch for
urgent fixes.

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
| **Stage** | one PR, one session, 1–n requests — down to a one-line change | [`plan-stage`](plan-stage/SKILL.md) → [`ship`](ship/SKILL.md) |
| **Task** | one commit | (a task within `ship`) |
| **Urgent** | fastest route the repo allows | [`hotfix`](hotfix/SKILL.md) |
| **Onboarding** | audit readiness + conformance, then remediate | [`audit-repo`](audit-repo/SKILL.md) → [`prep-repo`](prep-repo/SKILL.md) |

**You pick the path; the skills never pick each other.** Every skill here is
invoked by you and by name. When one turns out to be the wrong fit, it stops and
tells you which to run instead — it doesn't switch.

A stage doesn't need a phase — most work is one standalone stage, tracked from
the project's plans hub.

**Small work is a stage, not a hotfix.** A dependency bump gets the same ten
sections, most of them one line, and the same PR. `hotfix` is bought with
*urgency*, not smallness — it's the only path that trades away review, so
reaching for it on a calm two-line change spends the wrong currency.

Shared vocabulary and templates live in [`_method/`](_method/) — plain markdown
the skills link to (not skills themselves):

- [`_method/vocabulary.md`](_method/vocabulary.md) — phase/stage/task, status markers, the three doc layers, integration route, who verifies what
- [`_method/stage-template.md`](_method/stage-template.md) — stage-doc shape (ten required sections), with a worked small stage
- [`_method/phase-template.md`](_method/phase-template.md) — phase-doc shape
- [`_method/adr-gate.md`](_method/adr-gate.md) — what earns an ADR, and what stays in the archived stage doc
- [`_method/runbook-template.md`](_method/runbook-template.md) — rollout runbook, for changes a revert can't undo
- [`_method/definition-of-done.md`](_method/definition-of-done.md) — the gate

## Install

This repo is a plugin, and it hosts its own marketplace. No symlinks — install
and update are git operations, so a remote or SSH machine runs the same two
commands as your laptop.

### Claude Code

```
/plugin marketplace add BohdanMoroz11/shipyard
/plugin install shipyard
```

Update later with `/plugin update shipyard`. The CLI equivalents are
`claude plugin marketplace add …` and `claude plugin install shipyard@shipyard`.

### Cursor

[`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json) declares the same
skills for Cursor's plugin loader. If your Cursor build doesn't offer plugin
install yet, fall back to a clone symlinked at `~/.cursor/skills` (see below) —
the manifest is inert when unused, so both routes can coexist.

### Any tool that loads a skills directory

Point it at a clone. The skill dirs sit at the repo root next to `_method/`, so
the relative links between them resolve wherever the tree lands:

```bash
git clone https://github.com/BohdanMoroz11/shipyard.git ~/src/shipyard
ln -s ~/src/shipyard ~/.config/<tool>/skills   # or copy it
```

### Working on the skills themselves

`claude --plugin-dir /path/to/shipyard` loads the checkout directly, with no
install step and nothing registered — edits take effect on the next session.
`claude --plugin-dir . plugin details shipyard` prints what actually loaded
(note that `--plugin-dir` goes before the subcommand).

Then invoke by name — `/plan-stage` in Claude Code. Every skill sets
`disable-model-invocation: true`, so none of them fire on their own and their
descriptions cost nothing per session; you always pick the skill.

## Usage

All skills are **explicitly invoked** (they don't auto-fire). Typical session:

1. New/existing repo not set up yet → `audit-repo` (readiness + how its existing
   docs map to the format), then `prep-repo` (fix gaps, migrate legacy docs into
   the archive).
2. Big multi-session goal → `plan-phase` first. Otherwise go straight to:
3. `plan-stage` — align on the stage, write the stage doc. Small changes come
   here too; the doc just comes out short.
4. `ship` — branch, commit the stage doc then one commit per task, tests, docs
   distilled (domain doc always, ADR only when it binds future work), PR, agent
   checks, runbook if a revert can't undo it, stage doc archived; you smoke and
   you deploy.
5. Urgent one-off → `hotfix` (direct to `main` where the repo allows it, a
   fast-tracked PR where it doesn't).
