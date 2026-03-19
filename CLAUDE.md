# CLAUDE.md — Padang Culinary Map

> Briefing file for Claude Code. Read this before touching any file.

---

## Project Overview

A **static vanilla web app** that displays a curated culinary destination map of **Padang, West Sumatra**. No frameworks, no build tools, no package managers — just files that work in a browser and deploy to any static host (GitHub Pages, Netlify, etc.).

This lives at: `lab.fariz.id/padang-culinary-map/`

---

## Tech Stack (Hard Constraints)

| Layer         | Tool                               | Notes                                       |
| ------------- | ---------------------------------- | ------------------------------------------- |
| Map           | Leaflet.js via CDN                 | `https://unpkg.com/leaflet/dist/leaflet.js` |
| Map tiles     | CartoDB Voyager or Stadia Outdoors | Warm, colorful — no API key needed          |
| Data          | `data.js` (plain JS array)         | Converted from spreadsheet manually         |
| Styling       | Vanilla CSS with CSS variables     | No Tailwind, no utility libs                |
| Interactivity | Vanilla JS                         | No jQuery, no Alpine                        |
| Fonts         | Google Fonts via CDN               | Bold display + readable body                |

**Never introduce:** npm, webpack, React, Vue, TypeScript, or any runtime dependency that needs installing.

---

## File Structure

```
padang-culinary-map/
├── index.html      ← single page, imports everything inline or via CDN
├── data.js         ← destination array, sourced from sheet
└── CLAUDE.md       ← this file
```

Keep it flat. No `/src`, no `/dist`, no `/components`.

---

## Layout

**Split-screen, desktop-first:**

```
┌─────────────────────┬──────────────────────┐
│                     │  [HEADER / TITLE]    │
│                     ├──────────────────────┤
│    LEAFLET MAP      │                      │
│   (sticky, full     │   CARD LIST          │
│    viewport height) │   (scrollable)       │
│                     │                      │
│   Custom markers    │   One card per       │
│   on each spot      │   destination        │
│                     │                      │
└─────────────────────┴──────────────────────┘
```

- Map panel: `50vw`, sticky, `100vh`
- List panel: `50vw`, independently scrollable
- On mobile: stack vertically — map on top (40vh), cards below

---

## Visual Design Direction

**Aesthetic:** Bold & colorful — energetic, fun, food-forward. West Sumatran cultural palette.

### Color Palette (CSS variables)

```css
--color-chili: #c0392b; /* deep red — primary accent */
--color-turmeric: #e67e22; /* warm orange-gold — secondary */
--color-green: #27ae60; /* forest green — tertiary */
--color-cream: #fdf6ec; /* warm off-white — background */
--color-ink: #1a1a2e; /* near-black — text */
```

### Typography

- **Display / Title:** Something with personality — bold, wide. Suggested: `Playfair Display` or `Syne`
- **Body / Cards:** Clean, legible. Suggested: `DM Sans` or `Plus Jakarta Sans`
- Title should feel punchy — large, high-contrast, culturally resonant

### Map Markers

- Colored circle markers (Leaflet `circleMarker` or custom `DivIcon`)
- Use chili red with a white number or food emoji inside
- On hover/click: pulse animation or scale-up

### Cards

- Rounded corners (`border-radius: 12px`)
- Bold left-border color accent (`border-left: 4px solid var(--color-chili)`)
- Hover: subtle lift (`transform: translateY(-2px)`, soft shadow)
- Active/selected state: background shifts to `--color-turmeric` tint

---

## Interactions

Two-way sync between map and list:

1. **Click a card** → map `flyTo()` that marker, open its popup, card scrolls into view
2. **Click a map marker** → highlight the corresponding card in the list (scroll it into view)
3. **Popup content** — show name + short description only (keep it tight)

No filter UI needed. No search. Keep interactions focused.

---

## Data Format (`data.js`)

```js
const destinations = [
  {
    id: 1,
    name: "Restoran Sederhana",
    description: "Warung Nasi Padang legendaris, berdiri sejak 1972.",
    lat: -0.9471,
    lng: 100.4172,
    // extendable later:
    // category: "Nasi Padang",
    // price: "$$",
    // hours: "07.00 - 22.00",
    // image: "https://..."
  },
  // ...
];
```

- Coordinates from Google Sheets (lat/lng columns)
- Description in Bahasa Indonesia
- Fields are **addable later** — code should handle optional fields gracefully (don't crash if `image` is undefined)

---

## Coding Rules

- **No inline styles** — all styling via CSS classes or CSS variables
- **No `var` declarations** — use `const` / `let`
- **Progressive enhancement** — the map should load even if JS partially fails
- **Comments** in code where the logic is non-obvious
- **Accessible** — map container has `aria-label`, cards have semantic HTML
- Keep `index.html` readable — don't minify or compress anything

---

## Build Sequence (do in order)

1. Scaffold `index.html` with split layout skeleton + Leaflet CDN links
2. Stub `data.js` with 5 sample Padang destinations
3. Initialize Leaflet map centered on Padang (`[-0.9471, 100.4172]`, zoom `14`)
4. Render markers from `data.js`
5. Render card list from same `data.js`
6. Wire click interactions (card ↔ marker two-way sync)
7. Style: apply color palette, typography, card design
8. Polish: hover states, map popup styling, mobile responsive

---

## Context

- Part of **lab.fariz.id** — a personal portfolio/lab of static vanilla JS projects
- Consistent stack across projects: no build tools, CDN-only dependencies
- Related live project for reference: `lab.fariz.id/jkt-temp-heatmap/` (Leaflet-based)
- Data will eventually be served from `web-data-store` GitHub repo via jsDelivr CDN — for now, keep it inline in `data.js`
