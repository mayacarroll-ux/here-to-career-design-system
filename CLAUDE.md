# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

An unofficial design system for the **redesigned** Here to Career web app (Junior Achievement's career-exploration platform, https://career.ja.org). It is a pure static site — three files, no build step, no package manager, no JS:

- `tokens.css` — all design tokens. **Two layers**: (1) `--ja-*` *primitives* are the raw "JA Primitives" palette copied verbatim from Figma — these are facts, never edit a hex; (2) `--h2c-*` *semantic* tokens map primitives to roles (`--h2c-primary`, `--h2c-danger`, `--h2c-text`, …) and are the only thing components consume. To reskin, change a semantic alias; never hardcode a brand hex in `h2c.css` or `index.html`.
- `h2c.css` — component library consuming the semantic tokens. Class names intentionally mirror Bootstrap 4 (`.btn-primary`, `.card`, `.alert-info`) because the source app is a customized Bootstrap 4 theme and the CSS must drop onto its markup unchanged. Don't rename these to "cleaner" names.
- `index.html` — the documentation site. Every component in `h2c.css` should have a section here with a live preview and a code snippet; when adding/changing a component, update both.

## Source of truth

The design system is the visual designer's **Figma file** "H2C - JA - Design System" (page *JA - Design System*, frame `ja-design-system`), *not* the old live site's compiled CSS. The file has three variable collections, all now mirrored in `tokens.css`: **JA Primitives** (`--ja-*`), **JA Semantic** (`--h2c-*`, Light mode), and **JA Typography** (`--h2c-font-*`). The redesign promoted the old secondary teal to primary:

- `--h2c-primary` = `--ja-turquoise` (#00A0AF) — primary buttons, links, active state
- `--h2c-primary-dark` = `--ja-blue-black` (#22404D) — headings, dark button, footer/hero
- Status set (confirmed in the *Badges & Tags* section): active=turquoise, pending=yellow, completed=forest, cancelled=gray-600, error=red

Values confirmed directly against the Figma documentation frames:
- **Primitives** — exact hexes, named e.g. Immersive Blue-Black, Empowered Yellow, Leadership Lime, Sustainable Forest. The `colors` section documents primitives only.
- **JA Typography** — Display/H1 48·Bold, H2 36·SemiBold, H3 24·SemiBold, Body Large 18·Regular, Body 16·Regular, Caption 12·Medium. Encoded in the `--h2c-font-size-*` tokens; note H2/H3 are SemiBold, not Bold.
- **JA Spacing** — named scale xs .25 / sm .5 / md-sm .75 / md 1 / lg 1.5 / xl 2 / 2xl 3 rem (`--h2c-space-*`).
- **Buttons** ("CTA Button Base") — pill, 44px tall, states Default (turquoise) / Hover (outline) / Selected (dark), three sizes + icon variant.

The `--h2c-*` block in `tokens.css` is a verbatim mirror of the **JA Semantic** collection (read from Figma's Variables panel), grouped color/brand · status · text · background · border. Notable exact values: `text/primary` = **black**, `text/on-accent` = gray-900, `background/page` = gray-100, `background/subtle` = gray-50. The collection defines only `error`/`success` (+light) under status; the status-badge colors (active/pending/completed/cancelled) are *composed* from brand/status/text and live under the "Component-facing aliases" block, which also holds the Bootstrap-named feedback aliases (`info`→primary, `warning`→accent, `danger`→error). The one token not in JA Semantic is `--h2c-text-heading` (= brand/primary-dark) — there's no heading text variable, so this is an interpretation matching the rendered headings.

To re-read the Figma file: it's accessible via the Claude-in-Chrome extension. Open the file URL, then in the Layers panel expand `ja-design-system` and use Shift+2 (zoom to selection) on each section layer (`colors`, `typography`, `buttons`, `badges`, …).

## Commands

```sh
python3 -m http.server 8000    # serve locally at http://localhost:8000
```

No build, lint, or test tooling exists (and node/npm are not installed on this machine). Verify changes by loading the page in a browser.

## Conventions

- Primitive hex values are facts from Figma — don't "improve" them, and don't invent new ones in code; genuinely new color *ideas* belong in the Figma file (the designer's `JA Primitives` frame), not as new `--ja-*` tokens here.
- **Color hierarchy** (designer direction): darker surfaces recede (background), lighter surfaces come forward (foreground), and color comes mostly from photography/illustration/Lottie rather than flat fills. The `--h2c-surface-dark*` ramp (blue-black → midnight-ink → boundless) and the `--h2c-burst-*` accents (ocean-mint, pacific-blue, deep-harbor, lime, yellow) support this — burst tokens are for imagery/data-viz "pops", never UI chrome.
- Shape language: pill radius (100px) on buttons/badges/chips, 10px on cards, .25rem on inputs. Inputs are deliberately *not* pills.
- Headings: Montserrat bold (700) in `--h2c-text-heading`. Body: regular (400) in `--h2c-text`.
- Shadows are blue-black-tinted (`rgba(34, 64, 77, …)`), never pure black.
- Base typography in `h2c.css` is scoped under the `.h2c` class (set on `<body>`) so the stylesheet can coexist with other CSS.
- Deployment is static-host drag-and-drop; nothing may introduce a build step.

## Deployment

Live on GitHub Pages from the public repo. To publish updates: commit, then `git push origin main && git push pages main` (`origin` = private `JA` repo, `pages` = public `here-to-career-design-system` repo). GitHub token is stored in the macOS keychain; the standalone `gh` binary is at `~/bin/gh`.
