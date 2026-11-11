# Craftgate — project rules

Work at Level 3 using the `level-3-engineer` plugin on every task.

## Product

A measured craft gate for front-end work: a Claude Code plugin + free CI check that renders UI in a real browser, flags motion and accessibility defects, and publishes per-rule precision/recall from CraftBench. Full brief: `docs/brief.md`.

## Constraints

- Free to build and use: public repo, free GitHub Actions, users bring their own Claude plan.
- No model calls in CI. Model-graded evals run locally through Claude Code.
- Complement existing craft skills (Impeccable, Make Interfaces Feel Better, Transitions.dev) — never rebuild them.
- v1 scope is fixed to three must-haves (brief §5). Anything else goes to `docs/backlog.md`.
- Cross-platform: must work on Windows, macOS and Linux.

## Stack (proposed, confirm in plan)

TypeScript (strict), Node 22, pnpm, Playwright, axe-core, Vitest, Biome. Next.js for fixture apps.

## Commands

To be filled in when the repo is scaffolded.

## Decisions

- ADR 0001: name is Craftgate (see `docs/adr/0001-name.md`).
