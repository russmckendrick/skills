---
name: design-md
description: Create new DESIGN.md files from a brand brief or aesthetic description, and update existing DESIGN.md files coherently. DESIGN.md is Google Labs' open format for describing a design system to AI coding agents — YAML token frontmatter plus markdown rationale. Use whenever the user mentions "DESIGN.md", a design system file, design tokens for a project, or asks to capture a project's visual identity in a portable format. Also trigger when the user wants to derive a design system from an existing brand (logo, website, brief) into a structured form an agent can read, or when they want to update the colors, typography, or components of a project's existing DESIGN.md. Do not trigger this skill for generating UI code itself (React, HTML, components) — that is a separate concern handled by frontend-design or similar skills. This skill produces and modifies the DESIGN.md file; other skills consume it.
---

# DESIGN.md

DESIGN.md is a self-contained, plain-text representation of a design system that AI coding agents read to generate consistent UIs. It pairs YAML frontmatter (machine-readable tokens) with a markdown body (human-readable rationale).

This skill covers two workflows:

1. **Generate** a new DESIGN.md from a brand brief, aesthetic description, or existing brand reference.
2. **Update** an existing DESIGN.md — add or change tokens, swap palettes, refactor sections — without breaking references or section ordering.

Treat the DESIGN.md file itself as the deliverable. This skill produces and modifies the file; it does not generate the UI code that consumes it.

## Reference material to read before generating

Before producing a non-trivial file, read:

- `references/spec.md` — the condensed format specification (token schema, section structure, type system).
- `references/linting-rules.md` — the 8 linting rules the file should satisfy.

For complete examples to anchor against, see `assets/examples/editorial-light.md` (warm, sophisticated, print-inspired) and `assets/examples/productivity-dark.md` (cool, technical, high-density). Each demonstrates a different end of the design space and shows the canonical structure populated coherently.

You don't need to memorise the spec — the goal is for the file you produce to pass `npx @google/design.md lint` cleanly. The reference files exist so you can check details rather than guess.

## Section order — non-negotiable

Sections appear in this exact order. Skip any that don't apply, but don't reorder what you do include:

1. Overview (alias: Brand & Style)
2. Colors
3. Typography
4. Layout (alias: Layout & Spacing)
5. Elevation & Depth (alias: Elevation)
6. Shapes
7. Components
8. Do's and Don'ts

The `section-order` linting rule fires if any recognised section appears after one it should precede. Unknown sections are *preserved* by the linter (not rejected), so it's fine to add domain-specific sections like `## Iconography` or `## Motion` when they help — place them where they fit naturally in the flow.

## Generating a new DESIGN.md

Coherence matters more than completeness. A short, internally-consistent file beats a long file that contradicts itself. Work in this order even when the brief is sparse:

### 1. Distill the brand intent first

Before writing any tokens, internalise the aesthetic. Ask: what's the personality (playful, austere, technical, editorial, brutalist, organic)? Who's the audience? What single emotional response should the UI evoke? This becomes 2–4 sentences of prose in the **Overview** section, and it anchors every downstream decision. Don't skip this — agents that consume the file rely on the Overview for everything the tokens don't pin down.

### 2. Derive the palette from the intent

Pick colour values that *follow from* the brand intent rather than reaching for a default palette. Always define a `primary` token — the `missing-primary` rule warns when colours exist but no `primary` is named, and downstream agents will auto-generate one (taking control away from you).

Common conventions for palette names:

- `primary` — the dominant brand colour
- `secondary` — supporting / utilitarian
- `tertiary` — single accent for interaction
- `neutral` or `surface` — background foundation
- `on-surface` — primary text on the surface
- `error` — destructive / validation

Use full 6-digit hex in sRGB, in quotes: `"#1A1C1E"`. Three-digit shorthand is not part of the spec.

Verify contrast as you write components. Every component pair that defines both `backgroundColor` and `textColor` must clear WCAG AA — 4.5:1 minimum. The `contrast-ratio` rule fires below this threshold.

### 3. Choose typography intentionally

Most production design systems define 9–15 typography levels. For a starter DESIGN.md, 4–8 levels is plenty: a couple of headline sizes, a body size or two, and a label. Each level needs at least `fontFamily` and `fontSize`; add `fontWeight`, `lineHeight`, `letterSpacing` where they affect the look meaningfully.

Prefer unitless `lineHeight` (e.g. `1.6`) — it's the recommended CSS practice and scales correctly across font sizes.

If you define `colors`, also define `typography`. The `missing-typography` rule warns when colours exist without typography tokens, because agents will fall back to default fonts otherwise — and default fonts almost never match the brand intent.

### 4. Build spacing, rounded, and components from the foundations

`spacing` and `rounded` are optional but recommended (the `missing-sections` rule notes their absence at info level). Pick a coherent scale — usually a base of 4 or 8 with named levels (`xs`, `sm`, `md`, `lg`, `xl`).

For `components`, define the atoms most likely to be needed: `button-primary`, `button-secondary`, `input-text`, `card`. Use **token references** — `"{colors.primary}"`, `"{rounded.md}"` — not literal values. References give the user a single point of change later.

Define hover / active / pressed states as separate sibling components with related key names: `button-primary`, `button-primary-hover`, `button-primary-active`.

Component property names are constrained. Only these are recognised: `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`. Other names trigger the `broken-ref` rule.

### 5. End with Do's and Don'ts

This is where the design system's opinions live — the rules an agent should follow when the tokens don't fully specify behaviour. 4–8 bullets, each one practical and falsifiable:

- "Do use the primary color sparingly, only for the most important action per screen" — good (specific, actionable)
- "Do use the brand colours" — bad (vague, unverifiable)

## Updating an existing DESIGN.md

Updates carry more risk than new files because token references can quietly break. Follow this:

1. **Read the current file completely** before editing. Note which tokens are referenced where, especially in `components`.
2. **Make changes consistent across the file**. If you swap `colors.primary` from `#2665fd` to `#0B7A75`:
   - Update the value in the `colors` block.
   - Leave `{colors.primary}` references alone — they resolve to the new value automatically.
   - Update any prose in the **Colors** section that names the old hex (e.g. "Primary (#2665fd): ...") so the prose matches the tokens.
3. **Renames cascade**. If you rename `colors.tertiary` to `colors.accent`, update every `{colors.tertiary}` reference to `{colors.accent}`, or `broken-ref` will fire.
4. **Preserve unknown sections** the user has added (e.g. `## Iconography`, `## Motion`). These are intentional extensions; never strip them.
5. **Preserve canonical section order**. If new content needs a new known section, place it in its correct slot per the canonical order.
6. **Don't introduce orphaned colours**. The `orphaned-tokens` rule warns when a colour is defined but no component references it. If you add a colour, reference it from at least one component — or, if it's intentionally palette-only, mention that in the prose.
7. **Return the full updated file**, not a diff. DESIGN.md is small enough that the whole-file form is clearer, and the linter operates on whole files anyway.

## Self-check before returning the file

Before handing the result to the user, mentally walk the eight linting rules. The file should satisfy all of them:

| Rule | What to check |
| --- | --- |
| `broken-ref` | Every `{path.to.token}` resolves to a defined token; every component property is one of the recognised names |
| `missing-primary` | `colors.primary` exists if any colours are defined |
| `contrast-ratio` | Every component with both `backgroundColor` and `textColor` clears 4.5:1 |
| `orphaned-tokens` | Every defined colour is referenced by at least one component (or intentionally palette-only) |
| `missing-typography` | Typography tokens exist if colours do |
| `section-order` | `##` sections appear in canonical order |
| `missing-sections` | `spacing` and `rounded` are present (info-level, recommended) |
| `token-summary` | Informational only — no action needed |

`references/linting-rules.md` has the full text of each rule, what triggers it, and how to resolve.

## Output format

Always write the DESIGN.md file to disk so the user can download or commit it directly:

- On Claude.ai (with file tools): write to `/mnt/user-data/outputs/DESIGN.md` and surface it via `present_files`.
- In environments without file tools: return the file as a single fenced markdown code block.

For updates, return the **complete updated file**, not a diff.

After producing the file, give the user a brief (2–4 sentence) summary of the design intent and any notable choices — but keep it short. The DESIGN.md itself is the artifact; the prose-around-the-prose should not compete with it.
