# Craftgate

**A measured craft gate for front-end work.** Craftgate is an open-source Claude Code plugin and free CI check that renders your UI in a real browser, flags motion and accessibility craft defects, and backs every rule with a published precision and recall score from CraftBench — a benchmark of deliberately broken UIs.

> Status: pre-build. See [the product brief](docs/brief.md).

## Why

Craft skills tell an agent what good UI looks like. None of them prove their checks are accurate. Craftgate measures itself, works alongside the skills you already use (Impeccable, Make Interfaces Feel Better, Transitions.dev), and runs free on every pull request.

## Planned v1

- **CraftBench** — flawed fixture apps with ground-truth defect manifests and a public scorecard
- **Motion + accessibility review skill** — Playwright + axe-core in a real browser, reduced-motion and interruptibility probes, mechanical auto-fixes
- **CI gate + hook** — deterministic checks only, no API key, free GitHub Actions tier

## Licence

MIT
