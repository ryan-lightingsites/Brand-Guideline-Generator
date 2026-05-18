---
version: alpha
name: Architecture
description: Editorial · Structural · Premium
colors:
  primary: "#1A1C1E"
  accent:  "#B8422E"
  neutral: "#F7F5F2"
  ink:     "#1A1C1E"
typography:
  h1:
    fontFamily: Playfair Display
    fontSize: 3.5rem
    fontWeight: 700
    lineHeight: 1.1
  h2:
    fontFamily: Playfair Display
    fontSize: 2.5rem
    fontWeight: 600
    lineHeight: 1.2
  body-md:
    fontFamily: Inter
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.5
  label-caps:
    fontFamily: Inter
    fontSize: 0.75rem
    fontWeight: 500
    letterSpacing: 0.06em
rounded:
  sm: 0px
  md: 0px
  lg: 2px
spacing:
  sm: 8px
  md: 16px
  lg: 32px
  xl: 64px
components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor: "#FFFFFF"
    typography: "{typography.label-caps}"
    rounded: "{rounded.sm}"
    padding: 12px 24px
  card:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.ink}"
    rounded: "{rounded.md}"
    padding: 32px
template:
  tagline: "Editorial · Structural · Premium"
  primaryFont: Playfair Display
  secondaryFont: Inter
  borderRadius: "0"
  separationStyle: border
  borderOpacity: 12
  buttonShape: sharp
  buttonSize: medium
  buttonWeight: bold
  buttonGradient: false
  colorGroups:
    - { key: primary, group: primary, fixed: true }
    - { key: accent,  group: accent,  fixed: true }
    - { key: neutral, group: neutral, fixed: true }
    - { key: ink,     group: neutral, fixed: true }
  images:
    hero: ""
    content1: ""
    content2: ""
    content3: ""
    card1: ""
    card2: ""
    card3: ""
---

## Overview

Architectural minimalism meets journalistic gravitas. The UI evokes a premium matte finish — a high-end broadsheet or contemporary gallery — with deliberate whitespace and a single warm accent.

## Colors

- **Primary (`#1A1C1E`)** — token `{colors.primary}`.
- **Accent (`#B8422E`)** — token `{colors.accent}`.
- **Neutral (`#F7F5F2`)** — token `{colors.neutral}`.
- **Ink (`#1A1C1E`)** — token `{colors.ink}`.

The palette is normative — apply these tokens directly rather than introducing new hex values.

## Typography

- **Playfair Display** is the display family — use for `{typography.h1}` and `{typography.h2}`.
- **Inter** is the text family — use for `{typography.body-md}` and `{typography.label-caps}`.
- Never introduce a third typeface.

## Layout

Spacing follows the `{spacing.*}` scale. Use `{spacing.lg}` (32px) as the default gap between adjacent components and `{spacing.xl}` (64px) for vertical section padding.

## Elevation & Depth

Use thin 1px borders to separate surfaces. Reserve drop shadows for overlays and modals only.

## Shapes

Corner radii follow the `{rounded.*}` scale. Default to `{rounded.md}` for cards and inputs, `{rounded.sm}` for tight controls, `{rounded.lg}` for prominent or pill-like elements.

## Components

- `button-primary` — the only button style for primary calls to action. Hover states should darken the background by ~8%.
- `card` — the default container for grouped content. Apply a 1px border at low opacity for separation, never both.

## Do's and Don'ts

- **Do:** Reserve `{colors.accent}` for primary calls to action only.
- **Do:** Use generous whitespace — at least 32px gaps between elements.
- **Do:** Pair Playfair Display headlines with Inter body copy.
- **Don't:** Use rounded corners — sharp edges only.
- **Don't:** Introduce a third typeface.
- **Don't:** Apply drop shadows — use 1px borders for separation.
