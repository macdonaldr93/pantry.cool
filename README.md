# pantry.cool

Marketing website for **Pantry** — the free Chrome extension that skips recipe-blog
filler and saves recipes to your Chrome bookmarks. (Extension source lives in the
sibling `recipe-box` repo, being renamed to `pantry`.)

## Stack

Plain static **HTML + CSS + vanilla JS** — no framework, no build step. Hand-written
CSS with design tokens (see the top of `css/styles.css`). Deploys straight to GitHub
Pages from the repo root.

- **Design language:** "warm recipe-card editorial" — cream paper, [Fraunces](https://fonts.google.com/specimen/Fraunces)
  display + [Hanken Grotesk](https://fonts.google.com/specimen/Hanken+Grotesk) body,
  the product's tomato red (`#fa3d3c`) as the action color, and a dashed-stitch motif
  borrowed from the extension icon's book binding.
- Fully readable with JavaScript disabled; JS only adds the mobile menu, scroll-in
  reveals, and the hero "skip the story" animation. Respects `prefers-reduced-motion`.

## Structure

```
index.html        Landing page
privacy.html      Privacy policy (required for the Chrome Web Store listing)
support.html      Support + "request a site" + FAQ
css/styles.css    Design tokens + all styles
js/main.js        Nav toggle, reveals, hero demo, footer year
assets/           Icons (copied from the extension's public/)
CNAME             Custom domain (pantry.cool)
sitemap.xml       Linked from robots.txt
.nojekyll         Serve files as-is (skip Jekyll processing)
```

## Develop locally

No build needed. Open `index.html` directly, or serve the folder so root-absolute
paths (`/css/...`, `/assets/...`) resolve:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Regenerate the OG image

`assets/og.png` (1200×630) is a screenshot of `assets/og-source.html`. To rebuild it
after editing the source, serve the repo and capture the page at scale 1:

```sh
python3 -m http.server 8000   # then, in another shell, with a headless browser:
# load http://localhost:8000/assets/og-source.html at 1200×630 and screenshot to assets/og.png
```

Any headless Chromium works (Playwright, Puppeteer, `chrome --headless --screenshot`).
Keep the output exactly 1200×630 so it matches the `og:image:width/height` tags.

## Deploy (GitHub Pages)

1. Push to the default branch.
2. Repo **Settings → Pages → Build and deployment → Deploy from a branch**, branch
   `main`, folder `/ (root)`.
3. The `CNAME` file points the site at **pantry.cool**. In your DNS, point the apex
   domain to GitHub Pages (A/AAAA records) or use a `www` CNAME, then enable
   **Enforce HTTPS** once the certificate is issued.

## Before launch — known placeholders

These are intentionally stubbed to the values the marketing docs specify; swap when ready:

- **Web Store link** points at the existing live listing
  (`…/detail/recipe-box/lcfhmcjffbgobdkdhnmmglogcbfkccpc`). Update every occurrence
  once the listing is renamed to "Pantry" / gets a new ID.
- **Source code is private.** The site intentionally exposes no public GitHub links;
  bug, feature, and site-coverage requests route to email instead.
- **Logo / favicon** reuse the existing red *recipe-book* icon from the extension. A
  Pantry-specific mark is a likely follow-up; replace the files in `assets/` and the
  `og:image` will update with them.
- **Social share image** is `assets/og.png` (1200×630), rendered from
  `assets/og-source.html`. Regenerate it after any brand/copy change (see below).
- **Fonts** load from the Google Fonts CDN. To match the extension's no-tracking
  ethos exactly, self-host the two families under `assets/fonts/` and swap the
  `<link>` for an `@font-face` block.
