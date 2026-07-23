# Flow vocabulary

Shared by every skill in this repo. Read it once so the terms mean the same
thing everywhere.

## The hierarchy

- **Phase / initiative** — a big goal with a vague edge (days–weeks). Spans
  **many** sessions and is **never built in one go**. Tracked in a living
  roadmap doc that gets decomposed into stages and ticked off over time.
- **Stage** — one scoped chunk = **one PR** = **one session**. Has a plan doc
  (or a section of one) and is decomposed into commit slices.
- **Task** — **one commit**. A single logical change.

A stage is the working unit a session revolves around. A skill operates at the
stage level; the phase is only a tracking layer above it.

## Stage kinds

- **Automated** — just build it (≈95% of stages). No human decision needed
  beyond the initial alignment.
- **Collaborative** — a decision has to be made *with the user* first (e.g.
  which stack, which data model). Surface the choice, wait, record it, then
  build.

## The two execution modes

- **Pipeline** — the normal path: `plan → align → branch → commit slices →
  PR → verify (smoke + CI)`. Use for anything non-trivial.
- **Hotfix** — urgent. Skip the plan doc and the PR ceremony, ship straight to
  `main`, keep a shorter definition-of-done (tests still green, docs still
  updated if behaviour changed). Use only when speed genuinely matters.

## Doc-driven, not skill-driven

Project-specific facts (where plans live, money/timezone rules, codegen gates,
naming) belong in the **project's own docs**, not in these skills. Every skill
reads the repo's orientation docs first (`AGENTS.md` / `CLAUDE.md`,
`docs/plans/README.md`, or their equivalents) and treats them as the source of
truth. These skills carry the *method*; the repo carries the *specifics*.
