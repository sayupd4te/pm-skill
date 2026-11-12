# pm-skill

Six product-management skills for Claude Code, picked from [phuryn/pm-skills](https://github.com/phuryn/pm-skills) (72 skills) and cleaned up. The rest of that collection mostly restates frameworks the model already knows; these six add rules it doesn't follow by default.

| Skill | What it does |
|---|---|
| `create-prd` | Lean, decision-ready PRD (1–2 pages). Asks up to three questions before writing, never pads with "TBD", marks assumptions instead of inventing facts. Also reviews an existing PRD. |
| `strategy-red-team` | Attacks the load-bearing assumptions of a plan: steelman first, then "Fails if…", kill criterion and the cheapest test for each. |
| `pre-mortem` | Imagines the launch failed; sorts risks into Tigers, Paper Tigers and Elephants, with action plans for launch-blocking ones. |
| `interview-script` | Customer interview script following *The Mom Test*: past behavior, no pitching, with a note-taking template. |
| `survey-design` | Writes or reviews a survey: starts from the decision, asks about facts, pilots it, reports coverage and non-response. |
| `ab-test-analysis` | A/B test readout: sample size and power, SRM check, significance, guardrails, ship/extend/stop recommendation. |

## Install

```bash
claude plugin marketplace add sayupd4te/pm-skill
claude plugin install pm-skill@pm-skill
```

Skills load automatically when relevant. To force one: `/pm-skill:create-prd`.

To use a single skill without the plugin, copy its folder to `~/.claude/skills/` (or `.claude/skills/` in a project).

See also [PROMPTS.md](PROMPTS.md): a cheat sheet of copy-paste PM prompts (in Polish), including ones that call these skills.

## Changes from upstream

- Kept 6 of 72 skills; dropped redirects, duplicates and framework restatements.
- `ab-test-analysis`: fixed the sample-size formula. Upstream omitted the power term (Z₁₋β), which underestimates the required sample by about half while claiming to check 80% power. Added a concrete SRM test.
- Removed "Further Reading" links to paid courses, product promotions and references to skills not included here.
- Removed "Think step by step" boilerplate.

## License

MIT. Original skills © 2026 Paweł Huryn ([phuryn/pm-skills](https://github.com/phuryn/pm-skills)); see [LICENSE](LICENSE).
