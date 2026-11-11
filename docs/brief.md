# Craftgate: product brief (Nov 2026)

**Owner:** Jathavedan (github.com/Jathubuilds), 10–15 hrs/week. **Status:** pre-build. **Constraint:** free to build and use (public repo, free CI, users bring their own Claude plan). **Name:** Craftgate (renamed from Atelier after the §10 name check).

## 1. Problem, with evidence

- Basic accessibility is getting worse. WebAIM Million 2026 found detectable WCAG failures on 95.9% of home pages and 56.1 errors per page (+10.1% year on year), and names AI-assisted development as a likely contributor ([source](https://prioritypixels.co.uk/insights/web-accessibility-is-getting-worse-according-to-the-2026-webaim-million-report/), 1 Apr 2026).
- Automated rules cover only part of the problem: axe catches about 57% of issues ([Deque](https://www.deque.com/blog/automated-testing-study-identifies-57-percent-of-digital-accessibility-issues), 2021). The rest needs judgment, which is where an LLM can add value.
- Motion is a blind spot: Chromatic does not yet check `prefers-reduced-motion` ([docs](https://www.chromatic.com/docs/accessibility)); Design Bug Bot lists no motion or accessibility coverage ([21st](https://21st.dev/blog/coderabbit-vs-design-bug-bot), Sep 2026).
- Nobody publishes accuracy numbers: Impeccable, Make Interfaces Feel Better, Transitions.dev, Vercel web-design-guidelines, AccessLint and ui-craft publish no catch-rate or false-positive figures; Tessl shows Transitions.dev with "No evaluations available" ([Tessl](https://tessl.io/registry/skills/github/Jakubantalik/transitions.dev/transitions-dev/evals)); Greptile's benchmark ignores false positives ([Greptile](https://www.greptile.com/benchmarks)).
- Employers ask for this: Anthropic's Design Engineer, Web posting asks for "tooling with real adoption, such as MCP servers, Claude skills", deliberate motion and performance budgets ([posting](https://job-boards.greenhouse.io/anthropic/jobs/5223916008)).

**Gap:** craft skills tell an agent what good looks like; none prove their checks are accurate.

## 2. Primary user

A design engineer or front-end developer shipping React/Next.js with Claude Code, who already uses a taste skill such as Impeccable, opens 5–20 UI PRs a week, and has no dedicated accessibility or motion reviewer.

## 3. Job to be done

"When Claude or I change UI, I want a check I can trust to catch motion, accessibility and craft regressions before merge, with evidence and a fix, so I ship polished UI without reviewing every animation by hand or drowning in false alarms."

## 4. Solution and signature moment

**One sentence:** an open-source Claude Code plugin plus a free CI gate that renders your UI in a real browser, flags motion and accessibility craft defects, and backs every rule with a published precision and recall score from a benchmark of 50+ deliberately broken UIs.

**Signature moment:** a PR comment — "Modal entrance ignores `prefers-reduced-motion` and can't be interrupted mid-animation. Rule `motion/interruptible`: 94% precision on CraftBench (n=54). Fix: [diff]." Locally, `/craftgate:review` gives the same finding and a one-keystroke fix. (Numbers illustrative.)

## 5. v1 scope (3 must-haves)

1. **CraftBench eval harness** — 50+ flawed fixture repos with ground-truth defect manifests plus clean controls; scores Atelier and alternatives (plain Claude, `npx impeccable detect`, Vercel web-design-guidelines); public scorecard in the README.
2. **Motion + accessibility review skill** — drives Playwright + axe-core in a real browser; motion probes emulate reduced motion, check transform/opacity vs layout properties, and interrupt animations mid-flight; explains axe findings and auto-fixes mechanical cases. Follows the Agent Skills spec.
3. **CI gate + hook** — GitHub Action running only deterministic checks (axe, motion probes, optional Lighthouse CI), no API key, free tier; local PostToolUse hook runs the same checks after UI edits.

**Cut from v1:** agentic-UX trust-pattern skill (v1.1, experimental, once gradable fixtures exist); separate performance skill (ship a Lighthouse CI template instead); multi-plugin marketplace; dashboards; Figma; Vue/Svelte; Claude calls in CI; auto-fix beyond mechanical a11y.

## 6. Metrics (all targets)

- **North star:** external repos running the gate in CI per week — 25 repos 90 days after launch.
- **Guardrails:** per-rule false-positive rate ≤10% (rules >15% ship off by default); recall on seeded motion/a11y defects ≥75%; local review <90 s, CI <3 min on a free runner; always-on context <1.5k tokens; zero undisclosed network calls; build time ≤15 hrs/week.

## 7. Riskiest assumption and cheapest test

**Assumption:** a browser-backed skill catches meaningfully more motion and accessibility defects than plain Claude Code or Impeccable's detector, with few false positives.

**Test (one weekend, $0):** 10 small Next.js fixtures, 30 seeded defects, 10 clean controls; three arms (plain Claude Code, `npx impeccable detect`, draft skill); record catches and false positives.

**Decision rule:** continue if the skill wins by ≥20 percentage points at ≤15% false positives; otherwise pivot to a pure benchmark project ("CraftBench for craft skills").

## 8. Launch channels

- Anthropic plugin directory (claude.ai/directory/manage; public repo, security scan, install stats) — [docs](https://claude.com/docs/plugins/submit)
- Own marketplace via `.claude-plugin/marketplace.json` — [reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md); install with `/plugin marketplace add Jathubuilds/<repo>`
- skills.sh, claudemarketplaces.com, Tessl registry (publish CraftBench results to its evals tab)
- Design-engineering X/Bluesky (share scores with other skill authors as help, not attack), r/ClaudeAI, Show HN ("We benchmarked 5 front-end AI skills"), A11y Slack, dev.to
- Dogfood on his own site builds

## 9. Prior art (accessed 11 Nov 2026)

| Tool | Coverage | Adoption | Evals? |
| --- | --- | --- | --- |
| [Impeccable](https://github.com/pbakaus/impeccable) | 23 commands incl. audit/animate/optimize; deterministic detector + LLM critique; `detect --github` CI annotations | ~107k stars | None published |
| [Make Interfaces Feel Better](https://github.com/jakubkrehel/make-interfaces-feel-better) | Animation, typography, shadows, hit areas | 3.9k stars, 76.2k installs | None |
| [Transitions.dev](https://tessl.io/registry/skills/github/Jakubantalik/transitions.dev) | Transitions and motion | n/a | None |
| [Vercel web-design-guidelines](https://mcpservers.org/agent-skills/vercel/web-design-guidelines) | Guidelines review, `file:line`; no motion | n/a | None |
| Anthropic frontend-design | Generation aesthetics | Official | None |
| [Hallmark](https://github.com/Nutlope/hallmark) | 50 themes, browser lint | ~49.6k stars | `eval` folder (unchecked) |
| [AccessLint](https://cdn.jsdelivr.net/gh/accesslint/claude-marketplace@main/README.md) | WCAG 2.2 scan/audit/fix | n/a | None stated |
| [ui-craft](https://claudeskills.info/skills/educlopez/ui-craft/audit/) | Static a11y/perf/responsive incl. reduced motion | 450 stars | None |
| Chromatic + Storybook a11y | axe with baselines | — | No reduced-motion checks |
| Lighthouse CI | Perf budgets in CI | Wide | n/a |
| CodeRabbit / Greptile / Design Bug Bot | General PR / design-system review, paid | Wide | Greptile ignores false positives |

Formats: [Agent Skills spec](https://agentskills.io/specification.md) — `name` 1–64 chars matching the folder, `description` 1–1024 chars, SKILL.md under 500 lines; marketplace via `.claude-plugin/marketplace.json`, validate with `claude plugin validate`.

## 10. Name check

"Atelier" is taken in this ecosystem: [martinffx/claude-code-atelier](https://cdn.jsdelivr.net/gh/martinffx/claude-code-atelier@main/README.md), [heyman333/atelier-ui](https://claudemarketplaces.com/plugins/heyman333-atelier-ui) (closest clash), [rachidchabane/atelier](https://claudemarket.ai/marketplaces/rachidchabane-atelier), an [Atelier pixel-art MCP server](https://glama.ai/mcp/servers/qus6zhx8f6), InterSystems Atelier. "Hallmark" and "Craft" are also taken. Options: **Craftgate** (no collisions found), **Loupe** (non-Claude namesakes), or a sub-brand **CraftBench** (check against WebCraftBench, UI-Bench). Light searches only — confirm on GitHub, npm and trademark registers.

## 11. Top risks

1. **Marginal lift** over plain Claude or Impeccable — mitigated by the §7 test.
2. **Distribution** against 50–100k-star incumbents — position as complement and benchmark, not rival.
3. **Eval circularity and capacity** — one person seeding and grading looks circular, and 50+ fixtures is a lot at 10–15 hrs/week; invite outside fixtures, keep a held-out set, ship 3 must-haves only.

## 12. This week

Build the 10-fixture, three-arm test (§7) and publish the raw numbers. It decides go/pivot and becomes the first launch post.
