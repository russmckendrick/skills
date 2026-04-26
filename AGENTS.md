# AGENTS.md

## Tooling
- Validate one skill locally: `npx -y skills-ref validate skills/<name>/`
- CI (`.github/workflows/validate.yml`) runs that same command across every `skills/*/` on push and PR — that is the ground truth for "is this skill valid".

## Non-obvious rules
- The folder name under `skills/` MUST equal the `name:` field in that skill's `SKILL.md` frontmatter. The agentskills.io spec enforces this and the validator fails otherwise.
- `template/` deliberately lives at the repo root, NOT under `skills/`. Anything inside `skills/` is treated as a real skill by discovery and validation, so a template placed there would fail the name-match rule above.
- The repo itself IS the plugin. `.claude-plugin/marketplace.json` uses `"source": "./"` and `.claude-plugin/plugin.json` declares the root as the plugin, with skills served from `skills/<name>/`. Do not introduce a `plugins/<x>/skills/<y>/` layout — it would duplicate skill content for no gain.

For the add-a-skill workflow see `CONTRIBUTING.md`; for the spec see <https://agentskills.io/specification>.
