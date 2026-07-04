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

The site is a **Jekyll site** (migrated in 2026 from a single hand-built
`index.html`). Each music release is a Markdown post in `_posts/` with its own
shareable URL (e.g. `bluee.gg/echoic/`), a custom Atom feed and `jekyll-sitemap`
auto-generate the RSS feed and sitemap. GitHub Pages builds it natively (no
Actions); the plugins are on the Pages allowlist.

---

## Local development

- Ruby is managed with **chruby + ruby-install** (Homebrew). Active Ruby:
  **3.3.6** at `~/.rubies/ruby-3.3.6` (never the macOS system Ruby). `~/.zshrc`
  auto-activates it; a brand-new terminal "just works."
- The `Gemfile` uses the **`github-pages` gem**, which pins **Jekyll 3.10** and
  the plugins to the exact versions GitHub Pages runs, so local == production.
- Workflow: `bundle install` once, then `bundle exec jekyll serve` →
  `http://localhost:4000`. Always verify locally before pushing.

---

## Owner working style (important)

- The owner is comfortable editing HTML/CSS and is detail-oriented about design.
- **Do not make changes that weren't discussed.** When asked for a specific
  change, make exactly that change — don't "improve" copy, restructure, or
  retheme unprompted. Surface suggestions separately and let the owner decide.
- **Do not invent content** (labels, genres, credits, etc.). Leave unknown
  fields as commented placeholders for the owner to fill.
- **No em dashes in body copy / site text.** The owner prefers commas or
  rephrasing. (Em dashes in code comments are fine.) Separators in UI use a
  middot `·` (e.g. the release pill "New Release · Single", the meta row).
- Prefers consistency between light and dark mode — changes to one mode should
  generally be mirrored in the other so the two stay perceptually matched.
- Posts will be roughly monthly to twice-monthly.

---

## Design system

### Fonts (Google Fonts)
- **DM Mono** (weights 400, 500) — labels, nav, dates, metadata, captions.
- **EB Garamond** (400, 500, 400 italic) — serif body text, titles.
- The logo wordmark is NOT a font — it's baked-in vector paths (see Logo below).

### Color tokens — these are CSS variables; dark mode redefines them
Defined once in `assets/css/style.css`. Light mode `:root`:
```
--bg: #ffffff;             --surface: #f7f7f5;
--border: rgba(0,0,0,0.1); --text: #0f0f0f;
--muted: rgba(15,15,15,0.6);
--accent: #1a56db;         (the Blue Egg blue)
--logo-text: #0f0f0f;      --nav-bg: rgba(255,255,255,0.92);
--text-strong: rgba(15,15,15,0.82);
--text-faint: rgba(15,15,15,0.62);
--text-fainter: rgba(15,15,15,0.6);
--text-faintest: rgba(15,15,15,0.6);
--egg-divider: rgba(0,0,0,0.42);
--accent-underline: rgba(26,86,219,0.3);
--accent-hover-border: rgba(26,86,219,0.4);
--accent-hover-bg: rgba(26,86,219,0.04);
--watermark: rgba(15,15,15,0.08);
```
Dark mode `@media (prefers-color-scheme: dark)` overrides:
```
--bg: #0d0f14;             --surface: #16181f;
--border: rgba(255,255,255,0.12); --text: #e8e8ea;
--muted: rgba(232,232,234,0.53);
--accent: #5b8def;         (brightened blue — keeps SAME perceived color as
                            light mode against the dark bg; the logo egg uses
                            this same variable so accent + egg always match)
--logo-text: #e8e8ea;      --nav-bg: rgba(13,15,20,0.92);
--text-strong: rgba(232,232,234,0.85);
--text-faint: rgba(232,232,234,0.55);
--text-fainter: rgba(232,232,234,0.53);
--text-faintest: rgba(232,232,234,0.53);
--egg-divider: rgba(255,255,255,0.45);
--accent-underline: rgba(91,141,239,0.4);
--accent-hover-border: rgba(91,141,239,0.5);
--accent-hover-bg: rgba(91,141,239,0.08);
--watermark: rgba(232,232,234,0.08);
```
**Why the blue brightens in dark mode:** `#1a56db` only hits ~3:1 contrast on
the dark bg (hard to read); `#5b8def` reaches ~5.9:1. The accent and the logo
egg move together to the same value so there's never a mismatch.

**Why `--muted` and the `--text-faint*` tokens look "dark for muted":** they're
tuned so EVERY text element clears WCAG AA (4.5:1) in both modes, measured on its
actual background (cards use `--surface`, which is slightly darker than the page,
so the faint tokens need a touch more weight than a pure-white calc suggests).
The light and dark values are picked to land at matching ratios (~4.9–5.3:1) so
the two modes stay perceptually paired. If you lighten any of these for looks,
re-check contrast first. The decorative tokens are intentionally exempt and stay
faint: `--egg-divider` (brand egg in the dividers), `--border` (hairlines), and
`--watermark` (the footer easter-egg egg, also `aria-hidden`).

### Layout
- Centered column, `max-width: 720px`, `padding: 0 24px 80px`.
- Sticky frosted nav (`backdrop-filter: blur(8px)`, uses `--nav-bg`).
- Base 16px, line-height 1.7. Everything is left-aligned (only the hero logo,
  and the homepage box actions *on mobile*, are centered).
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
- Tiny in the find-me-links divider (between the music row and social row).
- Barely-visible in the footer as the watermark easter egg (see below).
- The egg path (reused everywhere):
  `M12 22C16.4183 22 20 18.4183 20 14C20 9.58172 16.4183 2 12 2C7.58172 2 4 9.58172 4 14C4 18.4183 7.58172 22 12 22Z`

### Streaming-link components (Buy / Listen)
- **Icons** are monochrome inline SVG `<symbol>`s defined ONCE in
  `_includes/service-icons.svg` and referenced via `<use href="#ic-...">`. They
  inherit color from CSS (`fill: var(--muted)`, hover `var(--accent)`), so they
  adapt to dark mode automatically. Slugs: `ic-bandcamp, ic-apple, ic-spotify,
  ic-amazon, ic-youtube, ic-pandora, ic-soundcloud, ic-iheart, ic-deezer,
  ic-tidal`. Most are Simple Icons paths; **Amazon** is the "a"+smile mark
  (no clean square glyph exists), **iHeart** is the heart mark with the wordmark
  cropped out via viewBox.
- **Buy** = a labeled button "Buy on Bandcamp" using the same `.social-link`
  treatment as the icons (surface bg, border, hover→accent). It stands out via
  its text label + its own section, NOT via color (keeps the icon set
  consistent — earlier accent-filled versions were rejected).
- **Listen** = an icon-only row.
- **10 services, fixed order, Bandcamp first (artist-forward):** Bandcamp(Buy),
  Apple Music, Spotify, YouTube Music, SoundCloud, Tidal, Deezer, Amazon Music,
  iHeartRadio, Pandora. Render only when the post has that URL.
- **Homepage box** shows Buy + 5 services (Apple, Spotify, YouTube Music,
  SoundCloud, Amazon) inline on desktop, stacked + centered on mobile.
  **Release page** shows the full 10.

### Tracklist (release page)
- A CSS **grid** so key / Camelot / BPM / length line up in columns down a
  multi-track release; a single track reads cleanly in the same frame. Each
  `<li>` is a `grid-template-columns: subgrid` row spanning the shared columns,
  cells are pinned with explicit `grid-column`, and the divider border lives on
  the `<li>` (not the cells) so it stays one continuous line. `.track-meta` is
  `display: contents` so the spec spans become cells of the row grid; the middot
  `.track-sep`s are hidden (columns replace them).
- **Mobile** (`max-width: 560px`): the six columns can't fit, so the title takes
  its own line and the specs drop to a second line as their own aligned columns
  (a 1fr spacer pushes the length to the right edge). Requires subgrid (Baseline
  2023); fine for the current browser targets.
- key + BPM use `--text-strong` (DJ-critical), Camelot + length use `--muted`.
- **Camelot is auto-derived** from `key` via `_data/camelot.yml` (a fixed 24-key
  lookup with sharp+flat spellings, lowercased on match); a front-matter
  `camelot:` on a track overrides it. So posts only need to set `key`.

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
- `lang="en"` on `<html>` (set from `site.lang`).
- **Color contrast meets WCAG AA (4.5:1) for all text in both modes** — see the
  color-tokens note above. Don't lighten `--muted` / `--text-faint*` without
  re-checking.
- **Heading outline (don't skip levels):** the hero logo SVG is wrapped in the
  page `<h1>` (`.hero-h1`, needs `width:100%` or the logo shrinks). Section
  labels are `<h2 class="section-label">` (the class forces `font-weight:400` so
  DM Mono doesn't render synthetic-bold). Homepage release titles are
  `<h3>`-wrapped links; release pages use `<h1>` title → `<h2>` section labels.
- **Skip-link:** `<a class="skip-link" href="#main">` is the first element in
  `<body>` (in `default.html`); content is wrapped in `<main id="main"
  tabindex="-1">`. Hidden off-screen until focused. Keep it first and keep the
  `#main` target.
- Every find-me link and every Buy/Listen icon link has an `aria-label` with the
  platform name. Each `.listen-link` shows a tooltip of that label on hover AND
  on keyboard focus (`:focus-visible::after`), plus an accent focus ring.
- Hero SVG: `role="img"` + `aria-label`. Decorative SVGs: `aria-hidden="true"`
  (incl. the `_includes/service-icons.svg` symbol sheet).
- Nav `role="navigation"`, footer `role="contentinfo"`, release boxes
  `role="article"`, page content in `<main>`.
- All images have meaningful `alt` text. Below-the-fold images use
  `loading="lazy"`.

---

## SEO / meta
- The shared `<head>` lives in `_layouts/default.html` and is per-page aware:
  `<title>` ("Release · Blue Egg"), canonical, scheme-aware `theme-color`, meta
  description (from the post teaser/excerpt), Open Graph + Twitter card.
- **Per-release OG card:** each release page sets `og:image` to its **cover art**
  so shared links render the artwork; `og:type` `music.song`; JSON-LD
  `MusicRecording` with `byArtist` Blue Egg.
- **Homepage JSON-LD** is `MusicGroup` with the **`sameAs` ("also known as")
  links** — currently **14**: the 10 streaming artist pages + Last.fm + Instagram
  + Twitch + the YouTube video channel. This is the main Knowledge Graph signal;
  edit it in ONE place (`_layouts/default.html`).
- `robots.txt` stays at root. `sitemap.xml` is AUTO-generated by jekyll-sitemap
  (do not hand-edit). `feed.xml` is a **custom Atom template** (`/feed.xml` in the
  repo root) — each entry carries the cover image, teaser, streaming links, and a
  link to the release page; author is "Blue Egg". Edit `feed.xml` to change the
  feed (jekyll-feed is intentionally NOT enabled so the two don't both write it).
  The feed discovery link (`application/atom+xml`) is in `<head>`.
- Target search terms: blue egg, electronic music, techno, grant goddard, los
  angeles. The bio body intentionally contains "techno."

---

## Social / streaming URLs

### Hero "find me" grid (homepage, 8 links in grid order — artist profiles)
Row 1 (music): Apple Music `music.apple.com/us/artist/blue-egg/1709237243`,
Bandcamp `blueegg.bandcamp.com`, Spotify
`open.spotify.com/artist/5YjeU3znfJ2FpbJ3e38LhX`, Tidal `tidal.com/artist/78118208`.
Row 2 (social): SoundCloud `soundcloud.com/blueeggclub`, Instagram
`instagram.com/blueegg`, Twitch `twitch.tv/blueeggclub`, YouTube
`youtube.com/@blueeggclub`.

### Artist profiles for `sameAs` (all known, kept in `default.html`)
Bandcamp `blueegg.bandcamp.com/`, Apple Music `.../artist/blue-egg/1709237243`,
Spotify `.../artist/5YjeU3znfJ2FpbJ3e38LhX`, Amazon Music
`music.amazon.com/artists/B0GXL2RW5B/blue-egg`, YouTube Music
`music.youtube.com/channel/UCNNEL8FXW5NWnfWGqDTQoWg`, Pandora
`pandora.com/artist/blue-egg/AR27ccfZZ7Vvtk6`, SoundCloud `soundcloud.com/blueeggclub`,
iHeartRadio `iheart.com/artist/blue-egg-50443833/`, Deezer
`deezer.com/us/artist/385952371`, Tidal `tidal.com/artist/78118208`, Last.fm
`last.fm/music/blue+egg`, plus Instagram, Twitch, YouTube channel.

### Per-release streaming links
Live in each post's front matter (track/album URLs, not artist URLs). Render
only when present.

---

## Site structure
```
/
├── _config.yml            # site config, permalink /:title/, plugins, home_post_limit: 10
├── Gemfile / Gemfile.lock # github-pages gem (Jekyll 3.10, matches Pages)
├── index.html             # homepage template: hero, find-me, posts loop, tour, bio
├── CNAME, robots.txt      # stay at root
├── favicon.ico, icon-*.png, apple-touch-icon.png, og-image.png  # stay at root
├── _layouts/
│   ├── default.html       # shared shell: <head> meta+JSON-LD, nav, footer, watermark
│   └── release.html       # per-release page
├── _includes/
│   ├── service-icons.svg  # the 10 icon <symbol> defs (used once per page)
│   └── release-box.html   # homepage "Latest" box (takes post=...)
├── _data/
│   └── camelot.yml        # musical key → Camelot code lookup (tracklist)
├── _posts/
│   ├── 2026-04-01-placid.md
│   └── 2026-05-01-echoic.md
├── assets/
│   ├── css/style.css      # ALL styles (single stylesheet)
│   └── images/
│       ├── releases/      # per-release art (echoic-web.jpeg, placid.jpg)
│       └── grantone.jpg   # bio photo
├── feed.xml             # custom Atom feed template (rich per-release items)
└── sitemap.xml          # AUTO-generated by jekyll-sitemap — do not hand-edit
```

### Post front-matter (all fields; optional ones render only when present)
```yaml
---
layout: release
tag: New Release          # pill category (always); e.g. could be "Tour" later
type: Single              # Single | EP | Album (pill, releases only)
title: Echoic
date: 2026-05-01
art: echoic-web.jpeg      # file in assets/images/releases/
# genre: Techno           # meta row + JSON-LD
# label: Self-released    # meta row
bandcamp:   https://...   # Buy
apple: …  spotify: …  amazon: …  youtube: …  pandora: …
soundcloud: …  iheart: …  deezer: …  tidal: …   # Listen
# tracklist:              # optional; renders even for a single (one row)
#   - { title: "…", key: "F# minor", bpm: 128, duration: "4:12" }
#   # per-track, all optional. key + bpm + duration render as an aligned-column
#   # grid (values line up down an EP). Desktop = one row; mobile = title on top
#   # with the specs as columns beneath, length pinned right. key feeds the
#   # visible text, the schema.org musicalKey, AND the displayed Camelot code
#   # (auto-derived via _data/camelot.yml); bpm feeds text AND a JSON-LD Tempo
#   # PropertyValue. Set `camelot:` on a track only to override the derived code.
# production_notes: |      # optional Markdown (gear + technique). The "Liner
#   …                       #   notes →" homepage link shows when this OR
# credits: |               #   credits OR tracklist is present.
#   Written and produced by Grant Goddard
---
Body = the short teaser shown in the box and at the top of the release page.
```

The pill reads `tag · type`. The card meta row reads `genre · label · date`
(separators only between present items; date always shows).

---

## Deploy workflow
- Edit locally → `bundle exec jekyll serve` to verify → commit → push to `main`.
- GitHub Pages auto-builds (Jekyll) and deploys to bluee.gg in ~1–2 min.
- **Adding a release:** drop a `_posts/YYYY-MM-DD-slug.md` + its art in
  `assets/images/releases/`. The homepage list, the release page, the RSS feed,
  and the sitemap all update automatically. No other files to touch.
- **Cover art:** square, and keep files lean — the build does NOT resize. Export
  around 800–1000px (≈150–300KB). The art doubles as the per-release `og:image`
  (set to 800×800 in `default.html`). To shrink an oversized source with sips:
  `sips -Z 800 -s format jpeg -s formatOptions 68 file.jpg --out file.jpg`.

---

## Future ideas (owner interested, not doing now)
- **Email capture / mailing list** — own the audience directly instead of only
  routing fans out to streaming platforms. A single signup field (Buttondown,
  ConvertKit, etc.), likely in the footer. Most impactful growth lever for an
  independent artist; revisit when ready.
- **Privacy-friendly analytics** — e.g. Plausible or GoatCounter with outbound
  link tracking, to see which services fans click from the "Listen" menu.
  Deliberately no Google Analytics. Revisit when there's a reason to.
- The "Listen" disclosure is effectively a self-hosted smart link (own it, no
  third-party redirect/tracking). If a "click outside to close" is ever wanted,
  it needs a few lines of JS (would break the current no-JS purity).
