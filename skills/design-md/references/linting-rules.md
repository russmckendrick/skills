# DESIGN.md — Linting Rules Reference

The `@google/design.md` linter runs eight rules against a parsed DESIGN.md file. Each rule produces findings at a fixed severity level: **error** (exit code 1), **warning**, or **info**.

A well-formed DESIGN.md should pass with zero errors and minimal warnings. Use this reference when generating or updating files to anticipate findings before the linter runs.

## broken-ref

**Severity:** error

Detects token references (`{path.to.token}`) that don't resolve to any defined token in the YAML frontmatter. Also flags unknown component sub-token property names.

**Triggers when:**

- A component references a token path that doesn't exist (e.g. `{colors.accent}` when no `accent` colour is defined).
- A component uses a property name not in the recognised set (`backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, `width`).

**Example finding:**

```
error  components.button-primary  Reference {colors.accent} does not resolve to any defined token.
```

**Resolution:** Define the missing token in the YAML frontmatter, or correct the reference path. For unknown properties, use one of the recognised component sub-token names.

## missing-primary

**Severity:** warning

Warns when colours are defined in the frontmatter but no token named `primary` exists. Without a `primary` colour, downstream agents auto-generate one, taking control away from the design system author.

**Triggers when:** The `colors` section has one or more entries, but none is named `primary`.

**Example finding:**

```
warning  colors  No 'primary' color defined. The agent will auto-generate key colors, reducing your control over the palette.
```

**Resolution:** Add a `primary` entry to the `colors` section.

## contrast-ratio

**Severity:** warning

Checks WCAG contrast ratios for component `backgroundColor` and `textColor` pairs. Warns when the ratio falls below the AA minimum of 4.5:1.

**Triggers when:** A component defines both `backgroundColor` and `textColor`, and the resolved colour values produce a contrast ratio below 4.5:1.

**Example finding:**

```
warning  components.card-dark  textColor (#999999) on backgroundColor (#333333) has contrast ratio 3.48:1, below WCAG AA minimum of 4.5:1.
```

**Resolution:** Adjust either the background or text colour to meet the 4.5:1 minimum. Resolved values are checked, so adjusting the underlying colour token cascades to all components that reference it.

## orphaned-tokens

**Severity:** warning

Identifies colour tokens that are defined but never referenced by any component. Orphaned tokens add noise and may indicate an incomplete design system.

**Triggers when:** A colour token exists in the `colors` section but is not referenced by any component's properties. Only fires when at least one component is defined.

**Example finding:**

```
warning  colors.tertiary  'tertiary' is defined but never referenced by any component.
```

**Resolution:** Reference the token in a component, or remove it if it's no longer needed. If you intentionally want a palette-only token, document the reason in the prose so future readers understand.

## missing-typography

**Severity:** warning

Warns when colours are defined but no typography tokens exist. Without typography tokens, agents fall back to default font choices, eroding the design system's typographic identity.

**Triggers when:** The `colors` section has entries but the `typography` section is empty.

**Example finding:**

```
warning  typography  No typography tokens defined. Agents will use default font choices, reducing your control over the design system's typographic identity.
```

**Resolution:** Add at least one typography token (e.g. `body-md`) to the frontmatter.

## section-order

**Severity:** warning

Warns when markdown sections appear out of the canonical order defined by the spec.

Expected sequence: **Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts.**

**Triggers when:** A recognised section heading appears before another recognised section that should precede it. Section aliases (e.g. "Elevation" for "Elevation & Depth") are resolved before checking.

**Example finding:**

```
warning  Section 'Components' appears before 'Typography', which is out of order.
```

**Resolution:** Reorder the sections in the markdown body to match the canonical sequence. Unknown sections may sit anywhere — only recognised sections are ordered.

## missing-sections

**Severity:** info

Notes when optional token sections (`spacing`, `rounded`) are absent from a file that already defines other tokens. Not required, but their absence means agents fall back to defaults.

**Triggers when:** The `colors` section has entries but `spacing` or `rounded` has no entries.

**Example finding:**

```
info  spacing  No 'spacing' section defined. Layout spacing will fall back to agent defaults.
```

**Resolution:** Add `spacing` or `rounded` sections to the frontmatter if you want explicit control over those values.

## token-summary

**Severity:** info

Emits a summary diagnostic reporting how many tokens are defined in each section. Purely informational — no resolution required.

**Example finding:**

```
info  Design system defines 4 colors, 3 typography scales, 2 rounding levels, 6 spacing tokens, 3 components.
```

## Quick reference

| Rule | Severity | Checks |
| --- | --- | --- |
| `broken-ref` | error | Token references that don't resolve; unknown component sub-tokens |
| `missing-primary` | warning | Colors defined but no `primary` exists |
| `contrast-ratio` | warning | Component `backgroundColor`/`textColor` below WCAG AA 4.5:1 |
| `orphaned-tokens` | warning | Colour tokens defined but never referenced by a component |
| `missing-typography` | warning | Colours defined but no typography tokens exist |
| `section-order` | warning | Sections out of canonical order |
| `missing-sections` | info | Optional sections (`spacing`, `rounded`) absent |
| `token-summary` | info | Count of tokens defined per section |

## Running the linter

If `npx` is available in the working environment:

```bash
npx @google/design.md lint DESIGN.md
```

Output is JSON by default. Exit code is `1` if errors are found, `0` otherwise.

If running the linter is not possible (e.g. sandboxed Claude.ai sessions), use this reference to manually verify each rule before returning the file to the user.
