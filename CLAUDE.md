# SAS Brand Guide Generator

A zero-build, browser-only tool that turns a short brand-input form into a polished, exportable brand guide (PDF + Markdown). No package manager, no bundler, no backend — just three static HTML files served over any static server.

## Goal

Let a user enter brand details (name, tagline, logos, colors, typography, images, design-system rules), preview a real-looking brand guide live, tweak it inline, and export it as a PDF, a human-readable `<brand>-brand-guide.md`, or a machine-readable `<brand>-DESIGN.md` (a token/spec file usable as design-system source of truth).

## Files

- **[index.html](index.html)** — the input form. Sections: Template gallery, Brand Identity, Logo (SVG/PNG/dark/light/icon), Typography, Color Palette, Brand Images (7 slots), Design Style (separation, button fill/shape/size/weight, gradient direction), Design System Details. Validates `brandName`, stashes the form payload in `localStorage` under `brandGuideData`, then redirects to `preview.html`.
- **[preview.html](preview.html)** — the live preview + editor + exporter. Two tabs (Brand Guide, Design System), a left sidebar that mirrors the form for in-place editing, and a dark/light UI toggle. Reads/writes `brandGuideData` in `localStorage` and owns all three exports: `exportPDF()` (via `html2pdf.js`), `exportMarkdown()`, `exportDesignMd()`.
- **[brand-guide.html](brand-guide.html)** — a hand-built reference brand guide for "Logoipsum". Acts as the visual/structural template that `preview.html` is modeled on (cover, mockup, palette, typography, content samples, rules, applications). Not part of the runtime flow.
- **[sas-logo.svg](sas-logo.svg)** — the app's own header logo.

## Data flow

```
index.html  ──(collectData → localStorage.brandGuideData)──▶  preview.html
                                                                ├─ live edits write back to localStorage
                                                                ├─ Export PDF      (html2pdf.js)
                                                                ├─ Export .md      (brand guide)
                                                                └─ Export DESIGN.md (design tokens/spec)
```

There is no server-side state. Everything (including uploaded images, as data URLs) lives in `localStorage` — so storage quota is the main failure mode (`generate()` surfaces this).

## Templates

`TEMPLATES` in `index.html` (around line 2122) is the source of truth for starter presets (`architecture`, `beauty`, `saas`, …). Each preset defines fonts, color palette, button styling, spacing/rounded/typeScale scales, dos/don'ts, and Unsplash demo images. `applyTemplate(key)` preserves user-entered identity fields and only swaps style fields.

## External dependencies (CDN, runtime only)

- Google Fonts (DM Sans + per-template families)
- `@simonwep/pickr` — color picker
- `html2pdf.js` — PDF export (preview.html only)

## Running locally

Any static server works. The session currently has one on `http://localhost:3001/` (python3 `http.server`, background id `bfq9npass`). Entry point: `/index.html`.

## Conventions worth knowing

- CSS uses a small custom-property system: `--accent`, `--bg`, `--surface`, `--surface-2`, `--border`, `--ink`, `--ink-muted`, `--radius`. Light mode is a `.ui-light` class on `<body>` that overrides those.
- Inline `onclick=` handlers throughout — keep functions globally reachable when refactoring.
- Color swatches carry a `group` (`primary` / `accent` / `neutral`) and a `fixed` flag used by template application logic.
- `DESIGN.md` export follows the `google-labs-code/design.md` shape (see comment near line 3633 of preview.html).
