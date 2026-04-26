# DESIGN.md — Format Specification (Condensed)

This is a working reference for producing DESIGN.md files. The full spec lives at <https://github.com/google-labs-code/design.md/blob/main/docs/spec.md>. This file is structured for fast lookup while authoring or editing.

## File structure

A DESIGN.md file has two layers:

1. **YAML frontmatter** — machine-readable design tokens, delimited by `---` fences at the top of the file.
2. **Markdown body** — human-readable design rationale organised into `##` sections.

The tokens are normative. The prose explains the *why*. Prose may use descriptive colour names (e.g. "Midnight Forest Green") that correspond to systematic token names (e.g. `primary`).

## Token schema

```
version: <string>          # optional, current: "alpha"
name: <string>
description: <string>      # optional
colors:
  <token-name>: <Color>
typography:
  <token-name>: <Typography>
rounded:
  <scale-level>: <Dimension>
spacing:
  <scale-level>: <Dimension | number>
components:
  <component-name>:
    <token-name>: <string | token reference>
```

`<scale-level>` is a named level in a sizing or spacing scale. Common: `xs`, `sm`, `md`, `lg`, `xl`, `full`. Any descriptive string key is valid.

## Token types

| Type | Format | Example |
| --- | --- | --- |
| Color | `#` + 6-digit hex (sRGB) | `"#1A1C1E"` |
| Dimension | number + unit (`px`, `em`, `rem`) | `48px`, `-0.02em` |
| Token Reference | `{path.to.token}` | `{colors.primary}` |
| Typography | composite object (see below) | — |

### Typography properties

| Property | Type | Notes |
| --- | --- | --- |
| `fontFamily` | string | Font family name |
| `fontSize` | Dimension | E.g. `48px`, `1rem` |
| `fontWeight` | number | Numeric weight (`400`, `700`). Bare or quoted in YAML — both equivalent |
| `lineHeight` | Dimension or number | Unitless number is a multiplier of fontSize and is the recommended form |
| `letterSpacing` | Dimension | E.g. `-0.02em` |
| `fontFeature` | string | Maps to `font-feature-settings` |
| `fontVariation` | string | Maps to `font-variation-settings` |

### Token references

A reference is wrapped in curly braces and contains a path to another value in the YAML tree. Example: `"{colors.primary}"`.

For most token groups, a reference must point to a **primitive** value (e.g. `{colors.primary-60}`), not a group (e.g. `{colors}`).

Within the `components` section, references to **composite** values are permitted (e.g. `"{typography.label-md}"` resolves to a whole typography object).

## Section structure

Sections use `##` headings. An optional `#` heading may appear for document titling but is not parsed as a section.

### Canonical section order

| # | Section | Aliases |
| --- | --- | --- |
| 1 | Overview | Brand & Style |
| 2 | Colors | — |
| 3 | Typography | — |
| 4 | Layout | Layout & Spacing |
| 5 | Elevation & Depth | Elevation |
| 6 | Shapes | — |
| 7 | Components | — |
| 8 | Do's and Don'ts | — |

Sections may be omitted, but those present must appear in this order. Unknown sections (e.g. `## Iconography`, `## Motion`) are preserved by consumers — they are not errors.

### Per-section purpose

**Overview** — Holistic description of look and feel. Brand personality, target audience, the emotional response the UI should evoke. Foundational context for cases where a specific rule isn't defined.

**Colors** — Defines the colour palettes. At least `primary` should exist. Convention: `primary`, `secondary`, `tertiary`, `neutral`. Tokens are `map<string, Color>`.

**Typography** — Defines typography levels (typically 9–15 in production systems; 4–8 is fine for a starter). Tokens are `map<string, Typography>`. Common naming: `headline-display`, `headline-lg`, `body-md`, `body-sm`, `label-md`, etc.

**Layout** — Layout strategy and spacing model. Grid system, containment principles, spacing scale rationale. Tokens go in `spacing`, which is `map<string, Dimension | number>`. Unitless numbers are valid for column counts or ratios.

**Elevation & Depth** — How visual hierarchy is conveyed. Shadow specs for elevated designs, or alternatives (tonal layers, borders) for flat designs.

**Shapes** — Shape language. Corner radii, edge treatments. Tokens go in `rounded`, which is `map<string, Dimension>`. Common: `none`, `sm`, `md`, `lg`, `xl`, `full`.

**Components** — Style guidance for component atoms. Common atoms: Buttons, Chips, Lists, Inputs, Checkboxes, Radio buttons, Tooltips. Tokens are `map<string, map<string, string>>`. Variants (hover, active, pressed) are separate sibling components with related keys.

**Do's and Don'ts** — Practical guidelines and pitfalls. Acts as guardrails for agents. 4–8 actionable, falsifiable bullets.

## Component property tokens

Recognised properties for entries in the `components` block:

| Property | Type |
| --- | --- |
| `backgroundColor` | Color |
| `textColor` | Color |
| `typography` | Typography (or reference to one) |
| `rounded` | Dimension |
| `padding` | Dimension |
| `size` | Dimension |
| `height` | Dimension |
| `width` | Dimension |

Other property names trigger the `broken-ref` rule.

## Recommended token names (non-normative)

Provided for consistency. Not required.

- **Colors**: `primary`, `secondary`, `tertiary`, `neutral`, `surface`, `on-surface`, `error`
- **Typography**: `headline-display`, `headline-lg`, `headline-md`, `body-lg`, `body-md`, `body-sm`, `label-lg`, `label-md`, `label-sm`
- **Rounded**: `none`, `sm`, `md`, `lg`, `xl`, `full`

## Consumer behaviour for unknown content

Designed to be extensible. Consumers handle unknown content as follows:

| Scenario | Behaviour |
| --- | --- |
| Unknown section heading | Preserve; don't error |
| Unknown colour token name | Accept if the value is a valid colour |
| Unknown typography token name | Accept as valid typography |
| Unknown spacing value | Accept; store as string if not a valid Dimension |
| Unknown component property | Accept with warning |
| Duplicate section heading | **Error** — file is rejected |

Duplicate headings are the one structural error. Everything else is preserved or accepted with a warning, in keeping with the spec's "foundation, not a prescription" stance.

## Minimal valid example

```markdown
---
name: DevFocus Dark
colors:
  primary: "#2665fd"
  surface: "#0b1326"
  on-surface: "#dae2fd"
typography:
  body-md:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: 400
rounded:
  md: 8px
---

## Overview
A focused, minimal dark interface for a developer productivity tool.

## Colors
- **Primary (#2665fd):** CTAs and active states
- **Surface (#0b1326):** Page backgrounds
- **On-surface (#dae2fd):** Primary text on dark backgrounds

## Typography
- **Body**: Inter, regular, 16px
```

This passes the linter (with info-level notes about missing `spacing` and `secondary`/`tertiary` colours) and is enough for an agent to start generating consistent UI.
