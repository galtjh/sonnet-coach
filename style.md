# My Frontend UI Style Guide

> Paste this into Claude Chat at the start of any UI conversation to get production-quality, distinctive frontend output.

---

## How to Use This File

When I ask you to build any UI — a component, page, dashboard, app, or layout — follow every rule in this guide without me having to repeat them.

---

## Core Mandate

Build **production-grade**, **visually distinctive** interfaces. Never produce generic, template-looking, or "AI-default" UI. Every output should look like it was designed by a senior product designer with strong opinions.

---

## Typography

- **Always** import fonts from Google Fonts or similar — never use system fonts (no Arial, Roboto, Inter, or sans-serif fallbacks as primary fonts)
- Pair a **display/heading font** (characterful, editorial) with a **body font** (readable, refined)
- Font size scale: use `clamp()` for fluid sizing where possible
- Line height: `1.4–1.6` for body; `1.1–1.2` for headings
- Letter spacing: tight on large headings (`-0.02em`), normal on body

**Good font pairings to draw from:**
- `Fraunces` + `DM Sans`
- `Playfair Display` + `Lato`
- `Space Mono` + `Nunito`
- `Cormorant Garamond` + `Jost`
- `Syne` + `Epilogue`

---

## Color & Theme

- Use **CSS custom properties** (`--color-primary`, `--bg`, etc.) for every color — no hardcoded hex values in component styles
- Commit to a **dominant palette** of 2–3 colors max, with 1 sharp accent
- Default to **dark themes** unless context strongly suggests light
- Avoid: purple-on-white, generic blue CTA buttons, grey-everything dashboards
- Add **subtle color contrast** between sections — never flat same-color stacking

**Palette approach:**
```css
:root {
  --bg: #0f0f0f;
  --surface: #1a1a1a;
  --text: #f0ede6;
  --accent: #e8c547;
  --muted: #666;
}
```

---

## Light Mode & Dark Mode

**Always build with both modes supported.** Every UI must include a toggle button and respect `prefers-color-scheme` as the default on load.

### Implementation Pattern

```css
/* Default: respect system preference */
:root {
  color-scheme: light dark;
}

/* Dark mode tokens */
[data-theme="dark"] {
  --bg:      #0f0f0f;
  --surface: #1a1a1a;
  --border:  #2e2e2e;
  --text:    #f0ede6;
  --muted:   #888;
  --accent:  #e8c547;
}

/* Light mode tokens */
[data-theme="light"] {
  --bg:      #f5f2ec;
  --surface: #ffffff;
  --border:  #e0dbd0;
  --text:    #1a1a1a;
  --muted:   #666;
  --accent:  #c47a1e;
}
```

```js
// Toggle logic (vanilla JS)
const toggle = document.getElementById('theme-toggle');
const root = document.documentElement;

// Respect system default on load
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
root.setAttribute('data-theme', prefersDark ? 'dark' : 'light');

toggle.addEventListener('click', () => {
  const current = root.getAttribute('data-theme');
  root.setAttribute('data-theme', current === 'dark' ? 'light' : 'dark');
});
```

### Toggle Button Rules
- Place it in the **top-right corner** of the navbar or header — always visible
- Use an icon, not text: `☀️` / `🌙`, or an SVG sun/moon icon
- Animate the icon swap with a `rotate` or `scale` transition — never a bare swap
- Button must have an accessible `aria-label`: `"Switch to light mode"` / `"Switch to dark mode"`

### Per-Mode Design Rules

| Property | Dark Mode | Light Mode |
|---|---|---|
| Background | Deep neutral (`#0f0f0f` – `#1c1c1c`) | Warm off-white (`#f5f2ec` – `#ffffff`) |
| Surface cards | Slightly lighter than bg | Pure white with soft shadow |
| Text | Warm white (`#f0ede6`) | Near-black (`#1a1a1a`) |
| Accent | Bright, saturated | Slightly darker/richer tone |
| Shadows | Near-invisible (dark on dark) | Soft and warm (`rgba(0,0,0,0.08)`) |
| Border | Subtle (`#2e2e2e`) | Light grey (`#e0dbd0`) |

### What to NEVER Do with Themes
- ❌ Pure `#000000` black backgrounds in dark mode — use deep neutrals
- ❌ Pure `#ffffff` white backgrounds in light mode — use warm off-whites
- ❌ Same accent color in both modes without adjustment — saturation shifts needed
- ❌ Hardcoded colors anywhere — every color must reference a CSS variable

---

## Layout & Spacing

- Use **CSS Grid** for page layout; Flexbox for component internals
- Embrace **asymmetry** — not everything needs to be centered
- Use generous whitespace — padding should feel luxurious, not cramped
- Spacing scale: `4px` base unit → `8, 16, 24, 32, 48, 64, 96px`
- Mobile-first: always include responsive breakpoints (`768px`, `1024px`)
- Avoid 12-column Bootstrap-style grids unless the content genuinely needs them

---

## Motion & Interaction

- Add **at least one entrance animation** — staggered fade-in is the minimum
- Hover states must be visible and intentional — no bare `:hover { opacity: 0.8 }`
- Use `transition: all 0.2s ease` sparingly — be specific (`color`, `transform`, `box-shadow`)
- For page-level animation, use CSS `@keyframes` with `animation-delay` for stagger
- Avoid heavy JS animation libraries unless the effect truly justifies it

```css
/* Preferred stagger pattern */
.card:nth-child(1) { animation-delay: 0s; }
.card:nth-child(2) { animation-delay: 0.1s; }
.card:nth-child(3) { animation-delay: 0.2s; }
```

---

## Components

### Buttons
- Always have a clear **primary** and **ghost** variant
- Rounded: `border-radius: 6px` (default) or `9999px` (pill) — pick one and be consistent
- Never use plain blue `#0000ff` or plain grey defaults

### Cards
- Use `box-shadow` layering, not just borders
- Add subtle background texture or gradient to surface cards

### Forms
- Floating labels or clearly separated label + input — never unlabelled inputs
- Focus states must be visible (`outline` or `box-shadow` on focus)

### Navigation
- Must work on mobile — either a hamburger menu or a scrollable tab bar

---

## Code Quality Rules

- **Single-file output** unless I specifically ask for separate files
- All CSS goes in a `<style>` block or a `<style jsx>` block — no inline styles except for dynamic values
- No external CSS frameworks (no Bootstrap, no Tailwind) unless I ask
- Code must be **self-contained and runnable** — I should be able to paste it and it works
- Add comments only for non-obvious decisions, not line-by-line narration

---

## What to NEVER Do

- ❌ Generic purple-gradient hero sections
- ❌ Stock-photo placeholder boxes with grey backgrounds
- ❌ Cookie-cutter card grids with no visual hierarchy
- ❌ Inter or Roboto as the primary font
- ❌ `color: blue; text-decoration: underline` links
- ❌ Submit buttons that say "Submit"
- ❌ Layouts that look identical on desktop and mobile

---

## Tone by Project Type

| Project Type | Aesthetic Direction |
|---|---|
| Dashboard / Data | Dark, structured, data-dense, monospace accents |
| Landing Page | Bold typography, high contrast, editorial feel |
| Educational App | Warm, clear, friendly — but not childish |
| Portfolio | Minimal, precise, personality-forward |
| Admin Panel | Clean, efficient, low distraction |

If the project type is unclear, ask before building.
