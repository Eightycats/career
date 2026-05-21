# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running locally

Because `index.html` fetches `career.json` via `fetch()`, the page must be served over HTTP (not opened as a file):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Architecture

This is a zero-dependency static site with three files:

- **`career.json`** — all content. Edit this to add/update work history entries.
- **`index.html`** — structure + all JavaScript. The JS fetches `career.json`, clones a `<template>` for each project, and appends cards to `#timeline`.
- **`styles.css`** — all styling. Uses CSS custom properties for the color palette and decorative imagery.

## How content rendering works

`index.html` contains a `<template id="project-template">` element. For each project in `career.json`, the JS:
1. Clones the template
2. Calls `normalizeImages()` to unify `projectImages` array and legacy `heroImage` into a single format
3. Calls `imageShape()` to classify each image as `wide`, `portrait`, or `square` based on the `aspect` ratio string (e.g. `"1000 / 375"`)
4. Applies CSS classes (`project-gallery`, `gallery-wide-stack`, `no-hero`, `from-left`/`from-right`) that drive layout via CSS Grid
5. Uses `IntersectionObserver` for scroll-reveal animation (`.visible` class)

## Adding a project

Edit `career.json`. Each project supports:

```json
{
  "name": "Company Name",
  "period": "2022–2025",
  "companyLogo": "assets/logo.png",
  "url": "https://example.com",
  "description": "What you built.",
  "projectImages": [
    { "src": "assets/hero.png", "alt": "Alt text", "aspect": "1000 / 375" },
    { "src": "assets/thumb.png", "alt": "Alt text", "aspect": "250 / 385",
      "links": { "apple": "https://apps.apple.com/...", "android": "https://play.google.com/..." } }
  ]
}
```

- `projectImages[0]` is the primary/hero image; remaining images go in the thumbnail row
- `aspect` drives layout shape classification (`wide` ≥1.35 ratio, `portrait` ≤0.9, else `square`)
- `links.apple`/`links.android` override the project `url` for app store deep-links (device detected via `deviceStore()`)
- `heroImage` is a legacy fallback for a single image when `projectImages` is absent

## Decorative elements

The `.stone` markers (cycling through 5 decor PNGs via `nth-child(5n+N)`) and `.tree-N` background elements are purely decorative CSS. The winding dirt path is an inline SVG `<path>` with a fixed cubic bezier curve — to adjust the path shape, edit the `d` attribute on both `.dirt-path-shadow` and `.dirt-path` in `index.html`.
