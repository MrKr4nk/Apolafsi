# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page website for **Απολαυση (Apolafsi)**, a fast-food restaurant in Amyntaio, Florina, Greece (est. 1999). No build tools, no framework, no package manager — everything lives in one file.

## Running the site

Open `index.html` directly in a browser. There is no dev server or build step. To preview quickly from the terminal:

```powershell
Start-Process index.html
```

## Architecture

Everything — CSS, HTML, JavaScript — is in **`index.html`**. The backup `index.v1.bak.html` is the previous design iteration; keep it for reference but do not edit it.

### Page sections (top to bottom)

| Section | ID | Notes |
|---|---|---|
| Navbar | `#navbar` | Fixed; becomes `.solid` (blurred bg) after 60 px scroll |
| Hero | `#hero` | Full-viewport; flying food emojis, pig mascot with parallax |
| Ticker | `.ticker` | Scrolling marquee bar (CSS animation only) |
| Menu | `#menu` | Dark chalkboard style; featured special card + 2-col category columns + sides grid |
| Story | `#story` | Red gradient background; mascot image |
| Find Us | `#find-us` | Phone, hours, embedded Google Map |
| Footer | `footer` | — |

### CSS design tokens (`:root`)

```
--red       #E22A2B   primary brand colour
--deep-red  #A31F1F   hover state
--yellow    #F2C84C   price highlights / accents
--charcoal  #232323
--dark      #1a1a1a   page background
--menu-bg   #161616   menu section background
--cream     #FFF8F0
--nav-h     68px      navbar height (used for offset calculations)
```

Fonts: **Oswald** (headings, nav, prices) and **Inter** (body copy), loaded from Google Fonts.

### i18n (bilingual GR / EN)

- All user-facing strings have `data-i18n="key"` attributes.
- The `T` object in the `<script>` block holds `{ gr: {…}, en: {…} }` translation maps.
- `setLang(l)` swaps `innerHTML` of every `[data-i18n]` element and persists choice to `localStorage`.
- **To add or change a string**: update the text in both `T.gr` and `T.en`, and set `data-i18n="key"` on the HTML element.

### JavaScript behaviour

All JS is vanilla, inline at the bottom of `<body>`:

- **Navbar**: `scroll` listener toggles `.solid` class at 60 px.
- **Mobile nav**: `toggleMobileNav()` / `closeMobileNav()` toggle `.open` on `#navLinks` and `#hamburger`.
- **Parallax**: `data-parallax="-0.14"` attribute on `.hero-right` drives the pig offset on scroll.
- **Scroll reveals**: `IntersectionObserver` adds `.vis` to `.reveal` / `.reveal-r` elements.
- **Active nav**: second `IntersectionObserver` (threshold 0.4) adds `.nav-active` to matching nav links.

### Animation patterns

- Flying emojis: each `.fly` / `.fly-e` uses CSS custom properties `--d` (duration) and `--del` (delay) for staggered float animation.
- Entrance animations on `.hero-enter` elements are triggered by JS adding `.show`.
- Price elements use `.price-pop` → `.vis` for a scale-in effect via IntersectionObserver.
- `@media (prefers-reduced-motion: reduce)` collapses all animations to near-zero.

## Key facts about the business (do not change without confirmation)

- Phone: **2386 023 155**
- Address: Αμύνταιο, Φλώρινα, Μακεδονία, Ελλάδα
- Founded: **1999**
- Hours: Mon–Sat 11:00 → late night; **Sunday closed**
- Delivery: phone orders only, windows 13:00–17:00 and 19:00+
- Mascot: pig (`ApolafsiPIGGY.png`) — appears in hero, story, and footer
- Logo image: `Apolafsi2.png` — fallback text "ΑΠΟΛΑΥΣΗ" if image fails to load
