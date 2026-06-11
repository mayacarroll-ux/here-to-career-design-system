# Here to Career — Design System

An unofficial design system extracted from [career.ja.org](https://career.ja.org) ("Here to Career", a Junior Achievement career-exploration platform). It documents the platform's visual language — deep-teal palette, Montserrat typography, pill buttons, soft-radius cards — as reusable design tokens and CSS components.

**Not affiliated with or endorsed by Junior Achievement.** Reference implementation only.

## What's here

| File | Purpose |
|---|---|
| `index.html` | The design-system documentation site (live component previews + code) |
| `tokens.css` | Design tokens only — colors, type scale, radii, shadows, spacing as `--h2c-*` custom properties |
| `h2c.css` | Component library built on the tokens (buttons, cards, badges, alerts, forms, navbar, hero) |

No build step, no JS, no framework dependency. Component class names mirror Bootstrap 4 (`.btn-primary`, `.card`, `.alert-info`, …) because the source site is a customized Bootstrap 4 theme — so `h2c.css` can restyle existing Bootstrap 4 markup directly.

## Develop

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

(Any static file server works; opening `index.html` directly also works.)

## Deploy

It's a pure static site — any static host:

- **Netlify**: drag the folder onto https://app.netlify.com/drop
- **GitHub Pages**: push to GitHub → repo Settings → Pages → deploy from `main` branch, root folder
- **Vercel / Cloudflare Pages**: import the repo, no build command, output dir `.`

## Using the tokens in another project

```html
<link rel="stylesheet" href="tokens.css">
<link rel="stylesheet" href="h2c.css">
<body class="h2c">
```

Or consume `tokens.css` alone and reference `var(--h2c-primary)` etc. from your own styles.
