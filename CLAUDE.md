# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

An unofficial design system for the **redesigned** Here to Career web app (Junior Achievement's career-exploration platform, https://career.ja.org). It is a pure static site — three files, no build step, no package manager, no JS:

- `tokens.css` — all design tokens. **Two layers**: (1) `--ja-*` *primitives* are the raw "JA Primitives" palette copied verbatim from Figma — these are facts, never edit a hex; (2) `--h2c-*` *semantic* tokens map primitives to roles (`--h2c-primary`, `--h2c-danger`, `--h2c-text`, …) and are the only thing components consume. To reskin, change a semantic alias; never hardcode a brand hex in `h2c.css` or `index.html`.
- `h2c.css` — component library consuming the semantic tokens. Class names intentionally mirror Bootstrap 4 (`.btn-primary`, `.card`, `.alert-info`) because the source app is a customized Bootstrap 4 theme and the CSS must drop onto its markup unchanged. Don't rename these to "cleaner" names.
- `index.html` — the documentation site. Every component in `h2c.css` should have a section here with a live preview and a code snippet; when adding/changing a component, update both.

## Source of truth

The palette now comes from the **Figma "JA Primitives" library** (the visual designer's redesign), *not* the old live site's compiled CSS. The redesign promoted the old secondary teal to primary:

- `--h2c-primary` = `--ja-turquoise` (#00A0AF) — primary buttons, links, active state
- `--h2c-primary-dark` = `--ja-blue-black` (#22404D) — headings, dark button, footer/hero
- Status set: active=turquoise, pending=yellow, completed=forest, cancelled=gray-600, error=red

The primitive→semantic mapping in `tokens.css` was **inferred from the Figma component screenshots**, since only the primitive variables were provided. If the Figma file defines its own semantic/alias variables, reconcile the `--h2c-*` block against them — the `--ja-*` primitive values themselves are confirmed exact.

## Commands

```sh
python3 -m http.server 8000    # serve locally at http://localhost:8000
```

No build, lint, or test tooling exists (and node/npm are not installed on this machine). Verify changes by loading the page in a browser.

## Conventions

- Primitive hex values are facts from Figma — don't "improve" them. Express every design decision as a *semantic* token referencing a primitive.
- Shape language: pill radius (100px) on buttons/badges/chips, 10px on cards, .25rem on inputs. Inputs are deliberately *not* pills.
- Headings: Montserrat bold (700) in `--h2c-text-heading`. Body: regular (400) in `--h2c-text`.
- Shadows are blue-black-tinted (`rgba(34, 64, 77, …)`), never pure black.
- Base typography in `h2c.css` is scoped under the `.h2c` class (set on `<body>`) so the stylesheet can coexist with other CSS.
- Deployment is static-host drag-and-drop; nothing may introduce a build step.

## Deployment

Live on GitHub Pages from the public repo. To publish updates: commit, then `git push origin main && git push pages main` (`origin` = private `JA` repo, `pages` = public `here-to-career-design-system` repo). GitHub token is stored in the macOS keychain; the standalone `gh` binary is at `~/bin/gh`.
