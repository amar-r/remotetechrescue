# remotetechrescue.com

Single-page static marketing site for a remote computer/IT support side project
serving home users and small businesses.

## Build

There is no build step. Hand-written HTML and CSS, no framework, no npm, no
bundler, no preprocessor. Edit the files directly.

Preview locally with any static server run from `public/`, e.g.
`python3 -m http.server` — the site files are in `public/`, not the repo root.

## Hosting

A Cloudflare **Worker** named `remotetechrescue`, serving static assets only —
not Cloudflare Pages. Workers Builds deploys on every push to `main`.

`wrangler.jsonc` has no `main` key on purpose: with only an `assets` block and
no Worker script, this is an assets-only Worker and `main` is optional. Adding
one would mean writing request-handling code that isn't needed here.

Build settings in the Cloudflare dashboard (Worker → Settings → Build):

| Field | Value |
| --- | --- |
| Build command | **empty** — there is nothing to build, and `npm run build` will fail |
| Deploy command | `npx wrangler deploy` |
| Root directory | `/` |
| Production branch | `main` |

`privacy.html` and `terms.html` are linked as `/privacy` and `/terms`. That works
because `html_handling` defaults to `auto-trailing-slash`, which serves
`foo.html` at `/foo`. Don't change that default without fixing the footer links.

DNS and the Worker's custom domain are managed by the owner outside this repo.

## Files

Site files live in `public/` — that directory is the deploy surface, so anything
placed there is publicly fetchable. `CLAUDE.md`, `.gitignore`, and
`wrangler.jsonc` sit at the repo root and are deliberately not served.

- `public/index.html` — the whole marketing page
- `public/privacy.html`, `public/terms.html` — short plain-language legal pages
- `public/styles.css` — shared by all three pages; do not inline styles per page
- `wrangler.jsonc` — Worker name and assets directory

## Design tokens

Defined once in `:root` in `styles.css`. Use the variables, not raw hex.

| Token | Value | Use |
| --- | --- | --- |
| `--ink` | `#14181C` | body text |
| `--paper` | `#E9E9E4` | warm-oat page background |
| `--petrol` | `#0F3A44` | hero screen panel, trust band |
| `--petrol-lift` | `#17505E` | title bar of the hero panel |
| `--glass` | `#7FB8A6` | sea-glass accent (CTA, markers, highlight) |
| `--signal` | `#E8C547` | mustard, used sparingly |
| `--muted` | `#5C6670` | secondary text |

Type: Bricolage Grotesque 700/800 for display, Public Sans 400/500/600 for body,
JetBrains Mono for labels, prices, and eyebrows. Loaded from Google Fonts.

Mustard is deliberately rare — currently exactly two places: the "no fix, no
charge" chip and the fake second cursor in the hero. Adding a third should be a
conscious decision.

## Conventions

- Vertical rhythm comes from `.band` only; horizontal from `.wrap` only. Don't
  add section padding via element selectors — it fights the existing spacing.
- Focus rings are global via `:focus-visible`, recoloured inside `.band--dark`.
- The animated second cursor is decorative: `aria-hidden`, absolutely positioned
  so it can't cause layout shift, and hidden below 760px and under
  `prefers-reduced-motion`.
- The FAQ uses native `<details>`/`<summary>` with CSS `+`/`–` markers. No JS.
- The only JavaScript on the site sets the footer copyright year.

## Contact

Email only — `help@remotetechrescue.com`, a placeholder to be swapped later.
**There is intentionally no phone number anywhere on the site for now.** Don't
add one, and don't add a phone field or "call hours" copy. The trust band
explicitly says the owner never cold calls, so a phone number would undercut it.
