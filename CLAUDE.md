# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

"Bright & Well" is a static, single-page educational website about oral hygiene (brushing/flossing basics, gum health, tooth stability, fresh breath, and post-extraction care). There is no backend, no build step, and no package manager — just three files.

## Development

There is no build system, package manager, or test suite in this repo. To preview changes, open `index.html` directly in a browser or serve the directory with any static file server, e.g.:

```
python3 -m http.server 8000
```

There is no linting or automated testing configured. Verify changes visually in a browser, including the mobile nav toggle (`.nav-toggle`) at widths below 780px and dark mode (`prefers-color-scheme: dark`).

## Architecture

The site is three files with a strict separation:

- **`index.html`** — all content and structure. Sections are self-contained `<section class="section" id="...">` blocks (`basics`, `gum-health`, `tooth-stability`, `fresh-breath`, `extraction-care`, `faq`), each numbered with a `.section-tag`. Diagrams are hand-authored inline SVGs (no external images/icons) that reference CSS custom properties directly in their `fill`/`stroke` attributes (e.g. `fill="var(--teal-600)"`), so they automatically adapt to light/dark mode along with the rest of the page.
- **`styles.css`** — all styling, organized by section with comment headers (Header, Hero, Sections, Figures/diagrams, Cards, Two column, Checklist, Timeline, Callouts, Accordion/FAQ, Footer, Responsive nav). Theming is driven entirely by CSS custom properties defined once in `:root` and overridden in a `@media (prefers-color-scheme: dark)` block — there is no light/dark toggle in JS, it follows the OS/browser preference. The responsive breakpoint for the mobile nav is `780px`.
- **`script.js`** — two small, independent behaviors, no framework, no build step:
  1. Mobile nav toggle: shows/hides `#navList` and closes it when a link is clicked.
  2. Active-section highlighting: an `IntersectionObserver` over `main .section, .hero` toggles `.active` on the matching `.main-nav a` as the user scrolls.

### Conventions to follow

- Keep new sections consistent with the existing pattern: `<section class="section[ alt]" id="...">` containing a `.section-inner` with a `.section-tag` (two-digit number), an `<h2>`, and a `.section-lede`. Alternate `section`/`section alt` background striping down the page (see existing sections).
- If a new section is added to `<main>`, add a corresponding nav link in `#navList` (`index.html`) — the active-section-highlighting `IntersectionObserver` in `script.js` picks up any `.section`/`.hero` with an `id` automatically, no JS changes needed.
- New diagrams should follow the existing inline-SVG approach and use the `var(--...)` custom properties for colors rather than hardcoded hex values, so they respect dark mode. A few small accent colors (e.g. `#8b3a3a` for the blood clot illustration, `#c0524f` for bleeding-gum dots) are intentionally hardcoded rather than themed — match this pattern only for similar illustrative accents, not for primary UI colors.
- Reusable content patterns already in use: `.card-grid`/`.card` for grids of short tips, `.two-col` + `.checklist` for paired lists, `.timeline`/`.timeline-item` for chronological steps, `.callout` (and `.callout.warning`) for highlighted notices, `.accordion`/`.accordion-item` (native `<details>/<summary>`) for FAQs.
- The content is educational/medical in nature and ends with a `.disclaimer` noting it isn't a substitute for professional advice — preserve this disclaimer if editing the FAQ section.
