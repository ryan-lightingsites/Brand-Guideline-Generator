# Authoring a category template (`templates/<category>.md`)

This spec is for a fresh session whose job is: **look at one or more website screenshots of a category (Dance, SaaS, Beauty, etc.) and emit a single Markdown file** that the SAS Brand Guide Generator can load as a "Start from a template" option.

You (the spawned session) will NOT have any context from the main project. Everything you need is in this file.

---

## Your inputs

- One or more screenshot files (PNG/JPG) of representative websites in the category.
- A category slug from the user (e.g. `dance`, `restaurant`, `gym`). If not given, derive a lowercase-kebab slug from the user's category name.

## Your output

A single file at `templates/<slug>.md` (relative to the repo root). Filename = slug + `.md`. No other files.

The file is a strict superset of the project's `DESIGN.md` export format:
- YAML front matter with design tokens (colors, typography, spacing, rounded, components).
- A `template:` block in the YAML carrying UI-only fields needed by the template gallery.
- A markdown body documenting the design system in prose.

The runtime parser reads BOTH the spec-defined YAML and the `template:` block, so every field listed below must be present.

---

## Required YAML fields

```yaml
---
version: alpha
name: <Category label, Title Case>            # shown in the template card, e.g. "Dance"
description: <one-line vibe>                  # used as DESIGN.md description AND template tagline

# ── Design tokens (DESIGN.md spec) ──
colors:
  primary:   "#RRGGBB"   # dominant brand color
  accent:    "#RRGGBB"   # interactive / highlight color
  neutral:   "#RRGGBB"   # light surface (paper/cream/cloud)
  ink:       "#RRGGBB"   # darkest text color
  # add 1–2 more named tokens ONLY if the category genuinely needs them
  # (e.g. "warning", "secondary"). Keep the palette tight: 4–6 total.

typography:
  h1:
    fontFamily: <Google Font name>
    fontSize: 3rem            # use rem; pick from typeScale below
    fontWeight: 700
    lineHeight: 1.1
  h2:
    fontFamily: <Google Font name>
    fontSize: 2.25rem
    fontWeight: 600
    lineHeight: 1.2
  body-md:
    fontFamily: <Google Font name>
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.5
  label-caps:
    fontFamily: <Google Font name>
    fontSize: 0.75rem
    fontWeight: 500
    letterSpacing: 0.06em

rounded:
  sm: 4px
  md: 8px
  lg: 16px

spacing:
  sm: 8px
  md: 16px
  lg: 24px
  xl: 48px

components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor: "#FFFFFF"          # or #111111 — pick by contrast against accent
    typography: "{typography.label-caps}"
    rounded: "{rounded.md}"       # use rounded.lg for pill, rounded.sm for sharp
    padding: 12px 24px
  card:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.ink}"
    rounded: "{rounded.md}"
    padding: 24px

# ── Template-gallery metadata (UI-only, parsed back into TEMPLATES) ──
template:
  tagline: "Word · Word · Word"          # 3 adjective tagline, e.g. "Bold · Kinetic · Sensual"
  primaryFont: <Google Font>             # same as typography.h1.fontFamily
  secondaryFont: <Google Font>           # same as typography.body-md.fontFamily
  borderRadius: "8"                      # bare number string, matches rounded.md without "px"
  separationStyle: shadow                # "border" | "shadow"
  borderOpacity: 10                      # 0–100, only used when separationStyle=border
  buttonShape: rounded                   # "pill" | "rounded" | "sharp"
  buttonSize: medium                     # "small" | "medium" | "large"
  buttonWeight: bold                     # "light" | "bold"
  buttonGradient: false                  # boolean
  buttonGradientDir: "to right"          # only if buttonGradient: true
  # Color groupings used by the UI palette editor. Each entry references a key from `colors:` above.
  colorGroups:
    - { key: primary, group: primary, fixed: true }
    - { key: accent,  group: accent,  fixed: true }
    - { key: neutral, group: neutral, fixed: true }
    - { key: ink,     group: neutral, fixed: true }
  # Leave images empty — user uploads their own. Do NOT add Unsplash URLs.
  images:
    hero: ""
    content1: ""
    content2: ""
    content3: ""
    card1: ""
    card2: ""
    card3: ""
---
```

### Field rules

- **`colors:` keys must be lowercase tokens.** `primary` / `accent` / `neutral` / `ink` are the canonical four. Add more only if the screenshots clearly warrant it.
- **Hex values must be 6-digit uppercase with leading `#`** (e.g. `#1A1C1E`).
- **All fonts must be free Google Fonts** — they load via Google Fonts CDN at runtime. Verify the family exists on fonts.google.com before using it.
- **`template.borderRadius` must match `rounded.md`** numerically (without `px`). They are read by two different paths.
- **`template.colorGroups` must reference every key in `colors:`** — same order is fine. The `group` value is one of `primary`, `accent`, `neutral`. `fixed: true` means the swatch is locked from removal in the editor.
- **Don't invent fields.** If something isn't listed above, leave it out — extra keys are silently dropped.

---

## Required markdown body (after the closing `---`)

Sections must appear in this order with these exact headings:

```markdown
## Overview

<2–3 sentence prose describing the category's visual feel: what mood, what era, what
material vocabulary (matte/glossy/textured), why these tokens were chosen.>

## Colors

- **Primary (`#XXXXXX`)** — token `{colors.primary}`.
- **Accent (`#XXXXXX`)** — token `{colors.accent}`.
- **Neutral (`#XXXXXX`)** — token `{colors.neutral}`.
- **Ink (`#XXXXXX`)** — token `{colors.ink}`.

The palette is normative — apply these tokens directly rather than introducing new hex values.

## Typography

- **<Primary font>** is the display family — use for `{typography.h1}` and `{typography.h2}`.
- **<Secondary font>** is the text family — use for `{typography.body-md}` and `{typography.label-caps}`.
- Never introduce a third typeface.

## Layout

Spacing follows the `{spacing.*}` scale. Use `{spacing.lg}` (24px) as the default gap between adjacent components and `{spacing.xl}` (48px) for vertical section padding.

## Elevation & Depth

<One sentence. Pick one of:>
- "Use thin 1px borders to separate surfaces. Reserve drop shadows for overlays and modals only." (when template.separationStyle = border)
- "Use drop shadows to lift cards off the page. Reserve flat borders for structural dividers only." (when template.separationStyle = shadow)

## Shapes

Corner radii follow the `{rounded.*}` scale. Default to `{rounded.md}` for cards and inputs, `{rounded.sm}` for tight controls, `{rounded.lg}` for prominent or pill-like elements.

## Components

- `button-primary` — the only button style for primary calls to action. <Hover guidance: "Hover states should darken the background by ~8%.">
- `card` — the default container for grouped content. Apply <a 1px border at low opacity | a soft shadow> for separation, never both.

## Do's and Don'ts

- **Do:** <category-specific rule>
- **Do:** <category-specific rule>
- **Do:** <category-specific rule>
- **Don't:** <category-specific rule>
- **Don't:** <category-specific rule>
- **Don't:** <category-specific rule>
```

Rules:
- 3 Do's and 3 Don'ts. Each should be ONE sentence, actionable, and reference a token or a concrete visual decision (not vague advice like "be consistent").
- Token references in prose use curly braces: `{colors.primary}`, `{typography.h1}`, `{rounded.lg}`, etc.
- Don't add sections beyond the ones above. The runtime parser ignores them; extra sections add noise.

---

## How to derive each field from screenshots

| Field | Method |
|---|---|
| `colors.primary` | The most prominent brand surface color (header bar, logo background, dominant block). Eyedropper the largest non-neutral area. |
| `colors.accent` | The CTA/link/highlight color. Look at primary buttons and active states. |
| `colors.neutral` | The page background or card surface. Usually near-white or near-black. |
| `colors.ink` | Body text color. Usually near-black or very-dark variant of primary. |
| `typography.h1.fontFamily` | Identify the display face from hero headlines. If unsure, pick a close Google Fonts equivalent. |
| `typography.body-md.fontFamily` | Body copy font. Often a clean sans (Inter, DM Sans, Manrope). |
| `typeScale` (h1/h2/body/caption sizes) | Measure rough ratios; round to common values (3rem, 2.25rem, 1rem, 0.75rem). Don't be precise — these are starting points. |
| `template.buttonShape` | `pill` if buttons are fully rounded (≥999px), `sharp` if 0–2px, `rounded` otherwise. |
| `template.buttonSize` | Eyeball padding: thin = small, generous = large, default = medium. |
| `template.separationStyle` | If cards have visible drop shadows → `shadow`. If they have hairline borders → `border`. |
| `template.buttonGradient` | True only if primary buttons clearly use a color gradient (not a flat fill). |
| `rounded.md` | The dominant corner radius on cards/inputs. |
| Do's / Don'ts | Translate the visual decisions you made above into rules. E.g. if buttons are pill-shaped, "Do: keep all interactive elements pill-shaped." |

---

## Concrete example

A minimal valid file would look like [example below — see `templates/_example.md`].

Save your file as `templates/<slug>.md` and stop. Do not modify any other file in the repo.

## Sanity checks before you finish

1. Filename is lowercase-kebab and ends in `.md`.
2. YAML front matter is between two `---` lines and parses (test with `yq` or `python -c "import yaml; yaml.safe_load(open('templates/<slug>.md').read().split('---')[1])"`).
3. Every key in `colors:` appears in `template.colorGroups`.
4. `template.borderRadius` (string, no unit) equals `rounded.md` numerically.
5. All fonts are real Google Fonts families.
6. All `images:` values are empty strings.
7. Body has exactly the 8 H2 sections listed, in order.
