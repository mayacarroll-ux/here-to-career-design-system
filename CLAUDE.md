# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

An unofficial design system extracted from https://career.ja.org ("Here to Career", Junior Achievement's career-exploration platform). It is a pure static site — three files, no build step, no package manager, no JS:

- `tokens.css` — all design tokens as `--h2c-*` CSS custom properties. Single source of truth for color, type scale, radii, shadows, spacing. **Every visual value in the system must come from here**; never hardcode a brand hex in `h2c.css` or `index.html`.
- `h2c.css` — component library consuming the tokens. Class names intentionally mirror Bootstrap 4 (`.btn-primary`, `.card`, `.alert-info`) because the source site is a customized Bootstrap 4 theme and the CSS must drop onto its markup unchanged. Don't rename these to "cleaner" names.
- `index.html` — the documentation site. Every component in `h2c.css` should have a section here with a live preview and a code snippet; when adding/changing a component, update both.

## Commands

```sh
python3 -m http.server 8000    # serve locally at http://localhost:8000
```

No build, lint, or test tooling exists (and node/npm are not installed on this machine). Verify changes by loading the page in a browser.

## Conventions

- Token values were extracted from the live site's compiled stylesheet — they are facts about the source brand, not aesthetic choices. Don't "improve" them (e.g. the pale-yellow `--h2c-warning: #f3f2b3` is a surface color paired with dark text, by design).
- Shape language: pill radius (100px) on buttons/badges, 10px on cards, .25rem on inputs. Inputs are deliberately *not* pills.
- Headings: Montserrat bold (700) in `--h2c-primary-dark`. Body: regular (400) in `--h2c-dark`.
- Shadows are teal-tinted (`rgba(35, 64, 77, …)`), never pure black.
- Base typography in `h2c.css` is scoped under the `.h2c` class (set on `<body>`) so the stylesheet can coexist with other CSS.
- Deployment is static-host drag-and-drop (Netlify/GitHub Pages); nothing may introduce a build step.
