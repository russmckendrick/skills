# design-md

A Claude skill for authoring and updating [DESIGN.md](https://github.com/google-labs-code/design.md) files — the open format from Google Labs for describing a design system to AI coding agents.

## What it does

Two workflows, scoped tightly:

1. **Generate** a new `DESIGN.md` from a brand brief, an aesthetic description, or an existing brand reference (logo, website, vibe).
2. **Update** an existing `DESIGN.md` coherently — add or change tokens, swap palettes, refactor sections — without breaking token references or section ordering.

The deliverable is the `DESIGN.md` file itself. Generating UI code *from* a `DESIGN.md` is out of scope and belongs to other skills (e.g. `frontend-design`).

## What is DESIGN.md?

A `DESIGN.md` file is a self-contained, plain-text representation of a design system. It pairs **YAML frontmatter** (machine-readable design tokens — colours, typography, spacing, components) with a **markdown body** (human-readable rationale across eight canonical sections: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts).

Tokens are normative; prose explains *why*. Agents reading the file can generate UI that's consistent with the design system without rediscovering it on every run.

The format ships with an `npx @google/design.md` CLI for `lint`, `diff`, `export` (to Tailwind / W3C DTCG), and `spec`. See the [official spec](https://github.com/google-labs-code/design.md) for the full format reference.

## Installing the skill

This is a Claude skill in the standard format. To use it:

- **Claude.ai (web/mobile/desktop)** — install the `.skill` package via the skills UI.
- **Claude Code** — drop the `design-md/` directory into your skills location (`~/.claude/skills/` or your project's `.claude/skills/`).
- **Anywhere skills are supported** — Claude reads `SKILL.md` and loads the bundled references and examples on demand.

The skill triggers when you mention `DESIGN.md`, design systems, design tokens for a project, or ask Claude to capture a brand's visual identity in a portable format.

## Example usage

```
> Write a DESIGN.md for a brutalist photography portfolio — sharp,
> editorial, monochrome with one acid-yellow accent.

> I've added a DESIGN.md to my project but the linter is complaining
> about orphaned tokens. Can you clean it up and add components that
> use the unreferenced colours?

> Update DESIGN.md to swap the primary colour to teal and add a
> destructive button variant.
```

The skill produces lint-clean output by default — every file is mentally walked against the eight `@google/design.md` linting rules before being returned.

## Skill structure

```
design-md/
├── SKILL.md                              # Authoring & update guidance for Claude
├── README.md                             # This file
├── references/
│   ├── spec.md                           # Condensed format spec for fast lookup
│   └── linting-rules.md                  # The 8 rules + how to resolve each
└── assets/
    └── examples/
        ├── editorial-light.md            # Warm, print-inspired (lints clean)
        └── productivity-dark.md          # Cool, technical, dense (lints clean)
```

Both bundled examples pass `npx @google/design.md lint` with zero errors and zero warnings. They're full, realised design systems intended as anchor points for pattern-matching, not templates to be copied verbatim.

## How it works

A few design choices worth knowing:

- **Lint mentally when the CLI isn't reachable.** The skill walks all eight linting rules manually before returning the file. When `npx @google/design.md` *is* available (Claude Code on a developer machine), that's the canonical path — but the skill is deliberately not gated on it, so it works in sandboxed Claude.ai sessions too.
- **Examples are pattern anchors, not templates.** The skill directs Claude to read the bundled examples for *shape* and *coherence*, not to regurgitate them. A "playful coffee shop" brief should never produce something that looks like the editorial example — but it should produce something with the same internal consistency.
- **Updates are higher-stakes than creation.** There's a dedicated section in `SKILL.md` covering token-reference cascades (changing a referenced value vs. renaming a token), preserving user-added unknown sections, maintaining canonical section order, and returning the full updated file rather than a diff.

## Credits

- **DESIGN.md format** — [Google Labs](https://github.com/google-labs-code/design.md), Apache 2.0 licensed. This skill is an unaffiliated wrapper that helps Claude work with the format.
- **Skill format** — [Anthropic's Claude skill specification](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview).

## Status

The DESIGN.md format is at version `alpha` and under active development. This skill tracks the spec as of the dates linked above; expect periodic updates as the format matures.
