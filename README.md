# readforward-website

The marketing site for **Read Forward** — [readforward.in](https://readforward.in) — served as a
static site from GitHub Pages.

## ⚠️ Deep-link critical files — do not touch

The Read Forward mobile app is deep-linked to this domain. Changing any of the files
below silently breaks `https://readforward.in/book/<id>` and `https://readforward.in/invite/<code>`
for every user who already has the app installed.

| File | Why it must not change |
|---|---|
| `.well-known/assetlinks.json` | Android App Links verification. Holds the **Play app-signing** SHA-256 fingerprint. If it changes, Android stops opening links in the app (they fall back to the browser). |
| `.well-known/apple-app-site-association` | iOS Universal Links. Declares `99DB6RLRL8.in.readforward.app` and the `/book/*` + `/invite/*` paths. Must be served as JSON with **no** `.json` extension. |
| `404.html` | The deep-link **store fallback**. GitHub Pages serves it for the dynamic `/book/<uuid>` and `/invite/<code>` paths, so reaching it means the app is not installed — it routes the visitor to the correct store by user agent. |
| `CNAME` | Points the custom domain at GitHub Pages. |
| `.nojekyll` | Disables Jekyll so `.well-known/` and `assets/` are served verbatim. |

Marketing changes belong in `index.html`, `privacy-policy.html` and `assets/` only.

Full reference: `docs/09-Deep-Linking-and-Website.md` in the app repo.

## Pages

| Page | URL | File |
|---|---|---|
| Landing | `https://readforward.in` | `index.html` |
| Privacy policy | `https://readforward.in/privacy-policy.html` | `privacy-policy.html` |
| Deep-link fallback | any unmatched path | `404.html` |

## Design

The site follows the app's **Futuristic (Aurora glass)** design system — frosted-glass
surfaces, an accent-coloured neon edge, an aurora backdrop and Space Grotesk type.
See `docs/16-Design-System-and-Theming.md` in the app repo.

Green (`#15803d`) is the core brand. Individual features carry their own accent so a
reader knows where they are without being told — Readings is terracotta (`#c2410d`),
Communities is indigo (`#4338ca`), LitCoins is purple (`#7c3aed`). On the web this is
expressed the same way it is in the app: as **data**, not code. Set `data-accent` on any
container and everything inside it re-tints, because nothing names a brand colour directly.

```html
<section data-accent="terracotta"> … </section>
```

Both pages are single self-contained HTML files (inline CSS, no build step). The only
external request is the Google Fonts stylesheet.

## Store links

| Platform | URL |
|---|---|
| App Store | `https://apps.apple.com/in/app/read-forward/id6767293889` |
| Google Play | `https://play.google.com/store/apps/details?id=in.readforward.app` |

## Updating

```bash
git clone https://github.com/achandak11-cloud/readforward-website.git
cd readforward-website
# edit index.html / privacy-policy.html
git add .
git commit -m "your message"
git push origin main
```

GitHub Pages redeploys within ~2 minutes. Verify the deep-link files still resolve afterwards:

```bash
curl https://readforward.in/.well-known/assetlinks.json
curl https://readforward.in/.well-known/apple-app-site-association
```
