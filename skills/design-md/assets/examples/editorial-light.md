---
version: alpha
name: Heritage Editorial
description: A warm, print-inspired editorial aesthetic for long-form reading and journalism.
colors:
  primary: "#1A1C1E"
  secondary: "#525B6B"
  tertiary: "#B8422E"
  neutral: "#F7F5F2"
  on-primary: "#F7F5F2"
  on-tertiary: "#FFFFFF"
typography:
  display:
    fontFamily: Public Sans
    fontSize: 3.5rem
    fontWeight: 600
    lineHeight: 1.05
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Public Sans
    fontSize: 2.25rem
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Public Sans
    fontSize: 1.5rem
    fontWeight: 600
    lineHeight: 1.25
  body-lg:
    fontFamily: Public Sans
    fontSize: 1.125rem
    fontWeight: 400
    lineHeight: 1.7
  body-md:
    fontFamily: Public Sans
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.6
  label-caps:
    fontFamily: Space Grotesk
    fontSize: 0.75rem
    fontWeight: 500
    lineHeight: 1
    letterSpacing: 0.12em
rounded:
  none: 0px
  sm: 4px
  md: 8px
  full: 9999px
spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 48px
  gutter: 32px
components:
  button-primary:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.on-tertiary}"
    typography: "{typography.label-caps}"
    rounded: "{rounded.sm}"
    padding: 14px
  button-primary-hover:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.sm}"
  button-secondary:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.primary}"
    typography: "{typography.label-caps}"
    rounded: "{rounded.sm}"
    padding: 14px
  card:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.primary}"
    rounded: "{rounded.sm}"
    padding: 32px
  input-text:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.primary}"
    rounded: "{rounded.sm}"
    padding: 12px
  meta-label:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.secondary}"
    typography: "{typography.label-caps}"
    padding: 4px
---

# Heritage Editorial

## Overview

Architectural minimalism meets journalistic gravitas. The interface evokes a premium matte finish — a high-end broadsheet or contemporary gallery rather than a software product. Aimed at readers who value substance over flourish: long-form publications, archives, museum collections, considered commerce.

The emotional register is calm and authoritative. Restraint over spectacle. The reader should feel they have arrived somewhere serious, where the content has been weighed.

## Colors

The palette is rooted in high-contrast neutrals with a single accent doing the work of interaction.

- **Primary (#1A1C1E)** — Deep ink for headlines and core text. Communicates permanence.
- **Secondary (#525B6B)** — Sophisticated slate for borders, captions, metadata. Quiet, utilitarian.
- **Tertiary (#B8422E)** — "Boston Clay". The sole driver of interaction, used exclusively for primary actions and critical highlights.
- **Neutral (#F7F5F2)** — Warm limestone foundation, softer than pure white. Establishes the matte-paper register.
- **On-primary (#F7F5F2)** — Limestone text on deep-ink surfaces.
- **On-tertiary (#FFFFFF)** — Pure white text on Boston Clay accents for maximum contrast.

## Typography

Two families in deliberate dialogue. Public Sans carries the narrative; Space Grotesk handles technical data.

- **Display & headlines** — Public Sans Semi-Bold. Tight tracking, generous line height. Establishes an institutional voice.
- **Body** — Public Sans Regular at 18px (lead) or 16px. Long-form readability is the priority.
- **Labels** — Space Grotesk for timestamps, bylines, metadata. Geometric construction evokes printer's marks. Always uppercase with generous letter spacing.

## Layout

Fluid grid on mobile; fixed-max-width grid (1200px) on desktop. A strict 8px scale (with a 4px micro-step) maintains rhythm. Generous internal padding on cards (24–32px) emphasises the considered, unhurried character of the brand. White space is structural, not decorative.

## Elevation & Depth

Depth is achieved through tonal layering, not shadows. The neutral limestone background recedes; primary content sits on the same surface but is delineated by typography weight, rule lines, and generous breathing room. Shadows would feel digital and undermine the matte register.

## Shapes

Architectural sharpness. All interactive elements use a minimal 4px corner radius. Modern enough to feel current, rigid enough to feel engineered. Pill shapes (`rounded.full`) appear only on metadata chips, never on primary actions.

## Components

- **Buttons** — Primary uses Boston Clay fill with white text; secondary uses limestone fill with deep-ink text. Both use the label-caps treatment.
- **Cards** — Limestone fill with deep-ink text. No shadow, no border. 32px internal padding.
- **Inputs** — Limestone fill, 4px radius, 12px padding. Border appears only on focus, in deep ink.
- **Meta labels** — Slate text on limestone, label-caps treatment. For datelines, bylines, section markers.

## Do's and Don'ts

- Do reserve Boston Clay for the single most important action per screen.
- Don't mix rounded and sharp corners in the same view.
- Do use the label-caps treatment for all metadata, regardless of size.
- Don't use shadows or elevation effects — depth is tonal here.
- Do maintain WCAG AA contrast (4.5:1) for all text.
- Don't use more than two font families on a single screen.
