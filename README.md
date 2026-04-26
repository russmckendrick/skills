# russ-skills

Russ McKendrick's collection of [Agent Skills](https://agentskills.io) — reusable, portable instructions and resources that any skills-compatible AI agent (Claude, Claude Code, Cursor, GitHub Copilot, Gemini CLI, OpenCode, and more) can load on demand.

Each skill lives in its own folder under [`skills/`](./skills) and is a plain `SKILL.md` file with YAML frontmatter, following the open [agentskills.io specification](https://agentskills.io/specification).

## Install

There are three ways to consume this repo. Pick whichever matches your agent.

### 1. skills.sh CLI (works with most agents)

Install all skills, or just one, via the cross-vendor [`skills` CLI](https://skills.sh):

```sh
# all skills
npx skills add russmckendrick/skills

# a single skill
npx skills add russmckendrick/skills/design-md
```

### 2. GitHub CLI (`gh skill`)

```sh
gh skill install russmckendrick/skills/design-md
gh skill list
```

`gh skill` writes provenance (repo, ref, tree SHA) into the installed `SKILL.md` frontmatter so the install can be traced and updated later.

### 3. Claude Code plugin marketplace

Inside a Claude Code session:

```
/plugin marketplace add russmckendrick/skills
/plugin install russ-skills@russ-skills
```

Then ask Claude to run any skill from the table below.

## Skills

| Skill | Description |
| --- | --- |
| [`design-md`](./skills/design-md) | Authors and updates [DESIGN.md](https://github.com/google-labs-code/design.md) files — Google Labs' open format for describing a design system (YAML tokens + markdown rationale) to AI coding agents. Generates a new `DESIGN.md` from a brand brief, or updates an existing one without breaking token references or section order. |

More on the way.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). The short version: copy [`template/`](./template) into `skills/<your-skill-name>/`, edit `SKILL.md`, validate locally with `npx skills-ref validate skills/<your-skill-name>`, open a PR.

## Licence

[MIT](./LICENSE) © 2026 Russ McKendrick.
