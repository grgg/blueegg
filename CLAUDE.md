# CLAUDE.md — Blue Egg Site

Project context for Claude Code. This file is read at the start of every
session. Keep it updated as the project evolves.

---

## What this is

A personal artist website for **Blue Egg** — the electronic/techno music
project of **Grant Goddard**, a producer and musician from Los Angeles.

- **Live site:** https://bluee.gg (also grgg.github.io/blueegg)
- **Repo:** github.com/grgg/blueegg
- **Hosting:** GitHub Pages, deploys automatically from the `main` branch root.
- **Custom domain:** bluee.gg via Porkbun DNS. Managed by the `CNAME` file in
  the repo root — do not delete it.

The site is currently a **single hand-built `index.html`** with all CSS inline.
The near-term project (see "Migration plan" below) is to convert it to **Jekyll**
so that music releases become individual posts with their own shareable URLs and
an auto-generated RSS feed.

---

## Owner working style (important)

- The owner is comfortable editing HTML/CSS and is detail-oriented about design.
- **Do not make changes that weren't discussed.** When asked for a specific
  change, make exactly that change — don't "improve" copy, restructure, or
  retheme unprompted. Surface suggestions separately and let the owner decide.
- **No em dashes in body copy / site text.** The owner prefers commas or
  rephrasing. (Em dashes in code comments are fine.)
- Prefers consistency between light and dark mode — changes to one mode should
  generally be mirrored in the other so the two stay perceptually matched.
- Posts will be roughly monthly to twice-monthly.

---

## Design system (carry this over exactly in the Jekyll migration)

### Fonts (Google Fonts)
- **DM Mono** (weights 400, 500) — labels, nav, dates, metadata, captions.
- **EB Garamond** (400, 500, 400 italic) — serif body text, titles.
- The logo wordmark is NOT a font — it's baked-in vector paths (see Logo below).

### Color tokens — these are CSS variables; dark mode redefines them
Light mode `:root`:
```
--bg: #ffffff;             --surface: #f7f7f5;
--border: rgba(0,0,0,0.1); --text: #0f0f0f;
--muted: rgba(15,15,15,0.45);
--accent: #1a56db;         (the Blue Egg blue)
--logo-text: #0f0f0f;      --nav-bg: rgba(255,255,255,0.92);
--text-strong: rgba(15,15,15,0.82);
--text-faint: rgba(15,15,15,0.3);
--text-fainter: rgba(15,15,15,0.25);
--text-faintest: rgba(15,15,15,0.2);
--egg-divider: rgba(0,0,0,0.3);
--accent-underline: rgba(26,86,219,0.3);
--accent-hover-border: rgba(26,86,219,0.4);
--accent-hover-bg: rgba(26,86,219,0.04);
--watermark: rgba(15,15,15,0.08);
```
Dark mode `@media (prefers-color-scheme: dark)` overrides:
```
--bg: #0d0f14;             --surface: #16181f;
--border: rgba(255,255,255,0.12); --text: #e8e8ea;
--muted: rgba(232,232,234,0.5);
--accent: #5b8def;         (brightened blue — keeps SAME perceived color as
                            light mode against the dark bg; the logo egg uses
                            this same variable so accent + egg always match)
--logo-text: #e8e8ea;      --nav-bg: rgba(13,15,20,0.92);
--text-strong: rgba(232,232,234,0.85);
--text-faint: rgba(232,232,234,0.4);
--text-fainter: rgba(232,232,234,0.35);
--text-faintest: rgba(232,232,234,0.3);
--egg-divider: rgba(255,255,255,0.35);
--accent-underline: rgba(91,141,239,0.4);
--accent-hover-border: rgba(91,141,239,0.5);
--accent-hover-bg: rgba(91,141,239,0.08);
--watermark: rgba(232,232,234,0.08);
```
**Why the blue brightens in dark mode:** `#1a56db` only hits ~3:1 contrast on
the dark bg (hard to read); `#5b8def` reaches ~5.9:1. The accent and the logo
egg move together to the same value so there's never a mismatch.

### Layout
- Centered column, `max-width: 720px`, `padding: 0 24px 80px`.
- Sticky frosted nav (`backdrop-filter: blur(8px)`, uses `--nav-bg`).
- Base 16px, line-height 1.7.
- Mobile breakpoint at `max-width: 560px` (layout-only overrides, no color
  changes — colors all come from variables that adapt automatically).

### Logo (hero)
- An SVG, `viewBox="0 0 700 280"`, `role="img" aria-label="Blue Egg logo"`.
- Two parts: (1) the blue egg shape, and (2) the wordmark "BLUE EGG" as
  **baked-in vector paths** traced from Cinzel 900 (NOT a loaded font — this was
  deliberate to stop it falling back to a system serif).
- Egg path fill uses `var(--accent)`; wordmark group fill uses `var(--logo-text)`.
- **The wordmark is not editable as text** — it's vector outlines. If the logo
  ever needs to change, the paths must be regenerated from the Cinzel font.

### The egg motif (brand signature, appears at three scales)
- Full size in the hero logo.
- Tiny in the social-links divider (between the music row and social row).
- Barely-visible in the footer as the watermark easter egg (see below).
- The egg path (reused everywhere):
  `M12 22C16.4183 22 20 18.4183 20 14C20 9.58172 16.4183 2 12 2C7.58172 2 4 9.58172 4 14C4 18.4183 7.58172 22 12 22Z`

### Dividers — dark-mode-plugin-safe
All divider lines use `border-top: 1px solid var(--border)` (NOT
`background: var(--border)`). This was a deliberate fix: auto-dark-mode browser
plugins invert borders reliably but skip thin background-colored elements and
pseudo-elements. Keep all rules/lines as real elements with borders.

### Footer watermark easter egg
- A faint egg (`--watermark` color) sits in the footer center.
- On hover it reveals a custom tooltip styled as a **Pierce & Pierce business
  card** (cream `#f4f1ea`, centered uppercase serif, drop shadow) showing the
  American Psycho quote: "Look at that subtle off-white coloring. The tasteful
  thickness of it. Oh my God, it even has a watermark." (comma, no em dash).
- The card stays cream in BOTH modes intentionally (it's a physical object, not
  UI). On mobile the tooltip switches to `position: fixed` centered in the
  viewport so it doesn't clip.
- `aria-hidden="true"` so screen readers skip the gag.

---

## Accessibility (maintain in all new pages)
- `lang="en"` on `<html>`.
- All 8 social links have `aria-label` with the platform name.
- Hero SVG: `role="img"` + `aria-label`. Decorative SVGs: `aria-hidden="true"`.
- Nav `role="navigation"`, footer `role="contentinfo"`, news items
  `role="article"`.
- All images have meaningful `alt` text. Below-the-fold images use
  `loading="lazy"`.

---

## SEO / meta (already on index.html; replicate per-page in Jekyll)
- `<link rel="canonical">`, scheme-aware `theme-color` (white for light,
  `#0d0f14` for dark), meta description, Open Graph tags (incl.
  `og:image:width/height/alt`), Twitter `summary_large_image` card,
  and JSON-LD `MusicGroup` structured data with all social `sameAs` links.
- `robots.txt` and `sitemap.xml` exist at root. The owner wants the site found
  for: blue egg, electronic music, techno, grant goddard, los angeles.
- The bio body intentionally contains the word "techno" for search.

---

## Social / streaming URLs (the 8 links, in grid order)
Row 1 (music): Apple Music `music.apple.com/us/artist/blue-egg/1709237243`,
Bandcamp `blueegg.bandcamp.com`, Spotify
`open.spotify.com/artist/5YjeU3znfJ2FpbJ3e38LhX`, Tidal
`tidal.com/artist/78118208`.
Row 2 (social): SoundCloud `soundcloud.com/blueeggclub`, Instagram
`instagram.com/blueegg`, Twitch `twitch.tv/blueeggclub`, YouTube
`youtube.com/@blueeggclub`.

---

## Current content
- **Two releases** (currently hand-coded news boxes in index.html):
  - "Echoic" — May 2026, art `echoic-web.jpeg`, links to Apple/Bandcamp/SC/Spotify.
  - "Placid" — April 2026, art `placid.jpg`, links to Apple/Bandcamp/SC/Spotify.
- **Bio:** Grant Goddard, LA producer; modular synthesis + Ableton; hybrid
  modular/DJ techno on CDJs synced to modular synth and drum voices. Photo
  `grantone.jpg`, caption "Grant Goddard — Munich, 2024".
- **Tour dates** and **bio** are NOT posts — keep them as simple editable
  sections, not structured content.

---

## MIGRATION PLAN — single-page HTML → Jekyll

Goal: each release becomes a Markdown post with its own URL + an auto-generated
RSS feed, while the design above is preserved exactly. GitHub Pages runs Jekyll
natively, so **no GitHub Actions needed** — the required plugins (`jekyll-feed`,
`jekyll-sitemap`) are on the Pages allowlist.

### Target structure
```
/
├── _config.yml          # site config + plugins (jekyll-feed, jekyll-sitemap)
├── index.html           # homepage — becomes a template that lists posts
├── CNAME                # keep (domain)
├── favicon.ico + icons  # keep at root
├── og-image.png         # keep at root
├── _layouts/
│   ├── default.html     # shared shell: <head> meta, nav, footer, the egg,
│   │                    #   the watermark easter egg, and the dark-mode CSS
│   └── release.html     # per-release page layout
├── _posts/              # one Markdown file per release
│   ├── 2026-04-01-placid.md
│   └── 2026-05-01-echoic.md
├── assets/
│   ├── css/style.css    # all the CSS moved out of index.html into one file
│   └── images/
│       ├── releases/    # per-release art (echoic.jpeg, placid.jpg, ...)
│       └── grantone.jpg # bio photo
└── feed.xml + sitemap.xml  # AUTO-GENERATED by the plugins — don't hand-edit
```

### Post front-matter shape (decide final fields with owner)
Each `_posts/YYYY-MM-DD-slug.md` looks like:
```
---
layout: release
title: Echoic
date: 2026-05-01
art: echoic.jpeg
apple:    https://music.apple.com/...
bandcamp: https://blueegg.bandcamp.com/track/echoic
soundcloud: https://soundcloud.com/blueeggclub/echoic
spotify:  https://open.spotify.com/album/...
# Future optional fields the owner wants room for:
# credits: |
#   Written and produced by Grant Goddard
# gear: |
#   Modular synth, CDJs, drum voices
---

I didn't go TOO crazy with panning on this one.
```
Body text (and future credits/gear) renders through the `release` layout into
the same visual style as the current news boxes.

### Open decisions to confirm with the owner before/while migrating
1. **Post URL style** — `bluee.gg/echoic/` vs `bluee.gg/releases/echoic/` vs
   `bluee.gg/news/echoic/`. (Owner leaning unspecified — ask.)
2. **Credits + equipment fields** — build them into the layout now (rendered
   only when present) so future longer posts are ready.
3. **Homepage list length** — show all posts, or latest N with older ones still
   at their own URLs. Relevant within a year at monthly cadence.
4. **Move images into `/assets/images/`** (tidier, must update paths) vs keep at
   root (simpler). Owner open to moving for cleanliness.

### Migration guardrails
- The site must look **pixel-identical** after migration — same fonts, colors,
  dark mode, dividers, egg, watermark easter egg. Jekyll changes how pages are
  *generated*, not how they look.
- Move CSS to `assets/css/style.css` and link it from `default.html` so every
  page (home + releases) shares one stylesheet.
- Verify locally with `bundle exec jekyll serve` before pushing.
- Keep `robots.txt`; let `jekyll-sitemap` generate `sitemap.xml` and
  `jekyll-feed` generate `feed.xml`. Add the feed discovery link to `<head>`:
  `<link rel="alternate" type="application/rss+xml" title="Blue Egg" href="/feed.xml">`.

---

## Deploy workflow
- Edit locally → review diffs → commit → push to `main`.
- GitHub Pages auto-builds (Jekyll) and deploys to bluee.gg in ~1–2 min.
- After a new post: just add the `_posts/*.md` file + its art in
  `assets/images/releases/`. The homepage list, the release page, the RSS feed,
  and the sitemap all update automatically. No other files to touch.
