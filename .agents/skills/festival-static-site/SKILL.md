---
name: festival-static-site
description: >-
  Use this skill when the user wants to create, scaffold, or customize a zero-build
  cultural or festival tribute static website (e.g., Diwali, Durga Puja, Holi, Navratri,
  Eid, Christmas, Onam, Pongal, Ganesh Chaturthi, Lunar New Year) complete with
  bespoke SVG artwork, interactive rituals, a downloadable 1200x800 Canvas PNG greeting
  card generator, Open Graph social preview assets (og-image.png + og-image.svg),
  GitHub Pages CI/CD, README.md, and GEMINI.md.
---

# Festival Static Website Generator Skill (`festival-static-site`)

This skill provides a complete, plug-and-play blueprint for building rich, zero-build, single-page cultural and festival tribute websites following the **ParbonStatic** architecture.

---

## 1. Required Inputs & Discovery

When triggered, identify the following parameters from the user's request (or infer sensible defaults using [Theme Presets](./references/theme-presets.md)):

1. **Festival Name & Subtitle**: e.g., *"Deepavali | Festival of Lights"*, *"Sharadotsav | Durga Puja Tribute"*, *"Rangotsav | Festival of Colors"*.
2. **Production Base URL**: e.g., `https://parbonstatic.github.io/<RepoName>/` (used for canonical and Open Graph absolute links).
3. **Color Palette (5 semantic tokens)**:
   - `primary` (e.g., `gold`), `secondary` (e.g., `marigold`), `accent` (e.g., `vermilion`), `surface` (e.g., `midnight`), `text` (e.g., `ivory`).
4. **Interactive Centerpiece Ritual**:
   - Examples: *Light a Virtual Diya*, *Offer Pushpanjali Flowers*, *Play Virtual Gulal/Colors*, *Hang a Festive Lantern*, *Ring the Temple Bell*.

---

## 2. Target Repository Structure

Always generate or maintain this exact zero-build repository layout:

```text
<FestivalRepo>/
├── .github/
│   └── workflows/
│       └── static.yml    # GitHub Actions workflow for GitHub Pages deployment
├── favicon.svg           # Custom 64×64 vector emblem matching the festival centerpiece
├── og-image.svg          # 1200×630 editable vector source for the Open Graph banner
├── og-image.png          # 1200×630 rasterized PNG banner (REQUIRED for social crawlers)
├── index.html            # Self-contained single-page app (HTML5 + Tailwind CDN + CSS + SVG + ES6 JS)
├── LICENSE               # The Unlicense (Public Domain dedication)
├── GEMINI.md             # AI assistant context, architecture, and OG link guide
└── README.md             # User-facing documentation, features, and OG customization guide
```

---

## 3. Step-by-Step Build Workflow

### Step 1: Configure `<head>`, Fonts, and Open Graph Meta Tags
In `index.html`, include:
- Semantic metadata (`title`, `description`, `keywords`, `theme-color`).
- Local SVG favicon (`./favicon.svg`) plus an inline `data:image/svg+xml` fallback icon.
- **Open Graph & Twitter Card Meta Block**: Copy the structure from [og-meta-template.html](./resources/og-meta-template.html).
  - **CRITICAL RULE 1**: Never use relative paths (`./og-image.png`) in `og:image`, `og:image:secure_url`, `og:url`, `twitter:image`, `twitter:url`, or `canonical`. Always use the full `https://` deployed origin.
  - **CRITICAL RULE 2**: Always point `og:image` and `twitter:image` to `og-image.png` (`1200×630`, `image/png`), because WhatsApp, Facebook, LinkedIn, and X/Twitter crawlers ignore `.svg` files.
- **Google Fonts & Tailwind CDN**: Configure semantic color tokens (`DEFAULT`, `light`, `bright`, `dark`, `deep`, `card`, `soft`) and font families (`display`, `accent`, `serif`, `sans`) inside `tailwind.config` and `:root` CSS variables.

### Step 2: Implement Core Visual & CSS Craftsmanship
Include these signature CSS utilities in `<style>`:
- **Foil / Glassmorphic Cards (`.gold-foil-card` or `.festive-foil-card`)**: Dark translucent surface (`backdrop-filter: blur(12px)`) with a composite CSS mask gradient border (`::before` using `mask-composite: exclude`) and warm hover elevation.
- **Metallic Gradient Text (`.text-gold-foil`)**: Multi-stop gradient clipped to text (`background-clip: text`).
- **Keyframes**:
  - Centerpiece animation (e.g., `flameFlicker`, `petalFloat`, `lanternSway`).
  - Halo pulse (`haloPulse`) and slow 360° background geometric mandala/motif rotation (`slowSpin`, `reverseSlowSpin`).
  - Scroll reveal transitions (`.reveal` and `.reveal.visible`).

### Step 3: Build the 8 Standard Application Sections in `index.html`
1. **Ambient Background Canvas (`#particles-canvas`)**:
   - Fixed full-viewport `<canvas>` rendering 35–45 gently drifting and pulsing thematic particles (golden sparks, flower petals, snow, or color motes).
2. **Sticky Navigation Header (`#navbar`)**:
   - Animated mini SVG emblem, desktop anchor links, primary interactive CTA button, and accessible mobile drawer (`#mobile-menu` with `aria-expanded`).
3. **Hero Section (`#home`)**:
   - Festive badge, display headline, poetic lead paragraph, dual CTAs, 3-column highlight strip (including a live session community counter like `1,08,421+`), and an intricate animated centerpiece SVG with rotating backdrop geometry.
4. **Spiritual / Cultural Significance & Origin (`#significance`)**:
   - 3 storytelling pillar cards with custom SVG icons, historical/mythological context, and bottom symbolism footers.
5. **Interactive Multi-Day / Chapter Stepper (`#five-days` or `#festival-days`)**:
   - Accessible tablist (`role="tablist"`, `role="tab"`, `aria-selected`) + Spotlight Detail Card (`#day-spotlight` showing badge, lunar/calendar date, title, description, revered figures/deities, key customs, core blessing, icon, and chant/quote) + Previous/Next cycle buttons + clickable overview cards (`[data-day-card]`).
6. **Timeless Rituals & Celebrations (`#rituals`)**:
   - 4-card responsive grid showcasing traditional crafts, threshold art, festive cuisine, and community celebrations.
7. **Interactive Ritual & Personalized Greeting Card Studio (`#virtual-diya` or `#interactive-offering`)**:
   - **Left Column**: Interactive SVG stage + particle burst `<canvas>` + customization form (Sender Name, Recipient Name, and 5 curated blessing themes).
   - **Right Column**: Live DOM Greeting Card preview (`#greeting-card-preview`) with 3 action buttons:
     - **Download Card (`#download-card-btn`)**: Renders a high-resolution `1200×800` PNG greeting card on an offscreen HTML5 2D Canvas (`downloadGreetingCardAsPNG()`) with gradient background, radial glow, geometric watermark, double ornate frame, corner motifs, drawn centerpiece vector illustration, theme pill, wrapped quote/sub-message (`drawWrappedText()`), and sender sign-off.
     - **Copy Greeting (`#copy-wish-btn`)**: Copies formatted plain text to clipboard (`navigator.clipboard.writeText` + `<textarea>` fallback).
     - **Share Wish (`#share-wish-btn`)**: Uses `navigator.share` with clipboard fallback.
8. **Sacred Chants / Literary Quotes & Quick-Copy Wishes (`#festive-wishes`) + Footer**:
   - Centerpiece traditional verse/mantra in original script, transliteration, and 3-part philosophical meaning breakdown, followed by 3 one-click `.quick-copy-btn` wish cards and a multi-column footer with day quick-jump links.

### Step 4: Generate Static Assets & CI/CD Files
1. **`favicon.svg`**: Create a `64×64` vector icon matching the festival centerpiece.
2. **`og-image.svg` & `og-image.png`**:
   - Create `og-image.svg` (`1200×630` viewBox) with the dark festive gradient, framed border, centerpiece vector illustration, festival title, and subtitle.
   - Rasterize `og-image.svg` into `og-image.png` at exactly `1200×630` pixels so social media previews work out of the box.
3. **`.github/workflows/static.yml`**: Copy from [static.yml](./resources/static.yml).
4. **`LICENSE`**: Copy from [LICENSE](./resources/LICENSE).
5. **`README.md` & `GEMINI.md`**: Document the project features, design tokens, architecture, and explicit instructions on how to update the Open Graph absolute URLs in `index.html`.

---

## 4. Verification Checklist

Run the verification script [verify-festival-site.ps1](./scripts/verify-festival-site.ps1) or manually verify:
- [ ] Opening `index.html` directly requires zero build tools and produces no console errors.
- [ ] All `<head>` Open Graph and Twitter URLs (`canonical`, `og:url`, `og:image`, `og:image:secure_url`, `twitter:url`, `twitter:image`) use full `https://` URLs and point to `og-image.png`.
- [ ] Both `og-image.svg` and `og-image.png` (`1200×630`) exist in the project root.
- [ ] All interactive modules work: mobile menu toggle, multi-day stepper tabs + prev/next buttons + footer jump links, interactive centerpiece + spark burst canvas, `1200×800` PNG greeting card download, and clipboard/share buttons.
- [ ] `README.md` and `GEMINI.md` clearly explain how to update the OG meta links for custom domains or forks.
