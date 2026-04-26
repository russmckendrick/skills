---
version: alpha
name: DevFocus Dark
description: A focused, technical dark interface for developer productivity tools. High information density, low visual noise.
colors:
  primary: "#5B8DEF"
  secondary: "#94A3B8"
  tertiary: "#22D3A3"
  surface: "#0B1326"
  surface-elevated: "#141C33"
  on-surface: "#E2E8F8"
  on-primary: "#0B1326"
  error: "#FF8A80"
typography:
  headline-lg:
    fontFamily: Inter
    fontSize: 1.75rem
    fontWeight: 600
    lineHeight: 1.2
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Inter
    fontSize: 1.25rem
    fontWeight: 600
    lineHeight: 1.3
  body-md:
    fontFamily: Inter
    fontSize: 0.9375rem
    fontWeight: 400
    lineHeight: 1.55
  body-sm:
    fontFamily: Inter
    fontSize: 0.8125rem
    fontWeight: 400
    lineHeight: 1.5
  label-md:
    fontFamily: Inter
    fontSize: 0.8125rem
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.01em
  code:
    fontFamily: JetBrains Mono
    fontSize: 0.875rem
    fontWeight: 400
    lineHeight: 1.5
rounded:
  sm: 4px
  md: 6px
  lg: 8px
  full: 9999px
spacing:
  xs: 4px
  sm: 8px
  md: 12px
  lg: 16px
  xl: 24px
  xxl: 32px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.md}"
    padding: 8px
  button-primary-hover:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.md}"
  button-secondary:
    backgroundColor: "{colors.surface-elevated}"
    textColor: "{colors.on-surface}"
    typography: "{typography.label-md}"
    rounded: "{rounded.md}"
    padding: 8px
  button-destructive:
    backgroundColor: "{colors.error}"
    textColor: "{colors.surface}"
    typography: "{typography.label-md}"
    rounded: "{rounded.md}"
    padding: 8px
  card:
    backgroundColor: "{colors.surface-elevated}"
    textColor: "{colors.on-surface}"
    rounded: "{rounded.lg}"
    padding: 16px
  input-text:
    backgroundColor: "{colors.surface-elevated}"
    textColor: "{colors.on-surface}"
    typography: "{typography.body-sm}"
    rounded: "{rounded.sm}"
    padding: 8px
  chip:
    backgroundColor: "{colors.surface-elevated}"
    textColor: "{colors.secondary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.full}"
    padding: 4px
  code-block:
    backgroundColor: "{colors.surface-elevated}"
    textColor: "{colors.on-surface}"
    typography: "{typography.code}"
    rounded: "{rounded.sm}"
    padding: 12px
---

# DevFocus Dark

## Overview

A focused, minimal dark interface for developer productivity tools — terminals, dashboards, observability surfaces, IDE companions. Optimised for long sessions and dense information. Clean lines, low chroma, high contrast where it matters.

The emotional register is calm and competent. The interface should feel like a well-tuned instrument: every pixel earns its place. No celebratory flourishes; the user's work is the protagonist.

## Colors

A cool dark palette grounded in deep navy, with a measured blue primary and a green success accent.

- **Primary (#5B8DEF)** — Calm interaction blue. CTAs, links, focus rings, active states.
- **Secondary (#94A3B8)** — Slate for muted text, captions, dividers, inactive controls.
- **Tertiary (#22D3A3)** — Mint green for success states and primary hover. Signals positive resolution.
- **Surface (#0B1326)** — Deep navy page background. Slightly warmer than pure black; reduces eye strain in long sessions.
- **Surface-elevated (#141C33)** — Cards, inputs, code blocks. Lifted by tone, not shadow.
- **On-surface (#E2E8F8)** — Primary text. High but not maximal contrast — easier on the eye than pure white at body sizes.
- **On-primary (#0B1326)** — Surface colour reused as text on primary fills.
- **Error (#FF8A80)** — Soft coral for destructive actions and validation. Visible without being alarming.

## Typography

Inter throughout for UI; JetBrains Mono for code. Single sans-serif family keeps the visual register tight; the monospace introduces information texture only where it semantically belongs.

- **Headlines** — Inter Semi-Bold. Tight tracking at larger sizes.
- **Body** — Inter Regular at 15px (default) or 13px (dense surfaces). Generous line height for scannability.
- **Labels** — Inter Medium at 13px. Slight positive letter spacing aids glanceability in chips and small UI controls.
- **Code** — JetBrains Mono at 14px. Used in code blocks, inline code, terminal output.

## Layout

A flexible grid based on a 4px unit. Information density is the priority — components sit closer together than in marketing-style interfaces. Sidebars are persistent. Toolbars are slim. Whitespace appears around groups of related controls, not around every individual element.

## Elevation & Depth

Depth is achieved through tonal layering only. The page sits on `surface`; cards, inputs, and panels sit on `surface-elevated`. No drop shadows, no glows. Hover states use a subtle background lift to the next tone, not a shadow expansion.

Focus rings are the exception: a 1px ring in `primary` blue on interactive elements meeting keyboard focus.

## Shapes

Soft-but-not-rounded. 6px is the default radius for buttons and inputs; 8px for cards. Pill shapes (`rounded.full`) are reserved for status chips and badges, where they signal distinct semantic units.

## Components

- **Buttons** — Primary uses brand blue fill with surface text; secondary uses elevated-surface fill; destructive uses soft coral.
- **Cards** — Elevated surface fill, 8px radius, 16px padding. No border, no shadow.
- **Inputs** — Elevated surface fill, 4px radius, body-sm typography. 1px primary-blue focus ring.
- **Chips** — Pill-shaped, elevated fill, muted text. For status, tags, filter pills.
- **Code blocks** — Elevated surface fill with JetBrains Mono. 4px radius, 12px padding.

## Do's and Don'ts

- Do use the primary blue sparingly — it should mark the single most important action per surface.
- Don't introduce shadows. Depth is tonal here; shadows would feel out of register.
- Do prefer 13–15px body text. Larger sizes break the dense-information character.
- Don't combine the mint accent with the soft coral in the same surface — they signal different emotional registers.
- Do use JetBrains Mono only for code, terminal output, or technical identifiers (not for prose).
- Do maintain WCAG AA contrast (4.5:1) for body text against any surface.
