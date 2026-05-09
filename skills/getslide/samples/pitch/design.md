---
name: pitch
description: "Modern tech pitch deck — single grotesk + 3-variant rotation + full chrome ring + monochrome + geometric decoration"
version: 0.4.0
source: "Built from the YC / TechCrunch pitch-deck aesthetic; v0.4 migrated to v2.1 schema (descriptive nicknames + role names + Do's & Don'ts + Known Gaps)"
---

# Pitch Theme

> A YC / TechCrunch-style pitch-deck theme. Single typeface, single hue family, rhythmic variant rotation — every deck reads like a tightly cut short film.

## §1 Atmosphere

A pitch deck has the demo-day "next big thing" mood: tight, modern, confident. The theme's visual character comes from big screens, strong grids, restrained whitespace — every pixel earns its place; nothing is decoration. Audience attention on a screen is scarce, and the pitch theme uses **a single typeface family + a single color family + rotating background variants** to make every frame feel like one frame in a film storyboard — clear breath between pages, but no part of any single frame steals the show.

Type is the spine. **Cabinet Grotesk** carries display, body, and nav alike — even the logo mark uses the same family. No serif, no italic, no secondary face. Weight steps across normal / semibold / bold to pull hierarchy by mass, not by glyph style. The CJK fallback drops into PingFang / Microsoft YaHei; on Chinese the display auto-shrinks from 120px to 112px — 120 is too dominant in CJK.

The color logic is **monochrome** — `{colors.accent}` = `{colors.primary}` = `{colors.foreground}`, all the same hex. There is no "brand blue / red / orange"; emphasis comes solely from the M20 pill highlight stroke, never from a tinted fill. But the deck is anything but monotone: **lite / dark / forest** — three variants — rotate inside one deck (lite cream / dark pure black / forest deep green `#0d2a25`). Per-page `data-variant` switches give a 9-page deck a "rhythm band" — P1 lite opens, P2 dark states the problem, P3 forest delivers the solution… The temperature shift between variants substitutes for chromatic accents — providing rhythm without breaking purity.

Decoration is geometric, always geometric. **M18 ambient frames** (rounded-rect SVG nested in the cover / closing background layer) are the wordless eyebrow of this theme — no text, no gradients, pure stroke, pure geometry, only a spatial gestalt for the page. **M20 pill highlight** wraps a single keyword in a stroked capsule and is the only "tint" allowed on the entire page (the stroke color = the current variant's `{colors.accent}`). No shadow (`{shape.shadow.default}: none`), no images, no gradients, no drop-caps, no eyebrow (the chrome already supplies page identity) — the theme deliberately blacklists every "editorial-leaning" motif.

**Key Characteristics**:
- Single Cabinet Grotesk family (display + body + nav)
- 3-variant rotation: lite (cream) / dark (pure black) / forest (deep green `#0d2a25`)
- Full chrome ring at all 8 anchors (cover never omits)
- Monochrome: `{colors.accent}` = `{colors.primary}` = `{colors.foreground}`
- Geometric motif whitelist: M07 oversized-numeral / M08 hairline-rule / M18 ambient-frames / M20 pill-highlight-word
- 12×8 strict grid + `data-layout="flow"` single-flow dual-track
- No shadow, no gradient, no italic, no serif, no chromatic accent

## §2 Palette values

### Surface
- **Mist** (`{colors.background}` — `#f0f0f0`): page canvas (slide base color)
  - lite: `#f0f0f0`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`
- **Card** (`{colors.card}` — base): in-slide content container
  - lite: `#f0f0f0` (same as background; differentiated by border)
  - dark: `rgba(0, 0, 0, 0.30)` (overlay black: deeper on the dark base)
  - forest: `rgba(0, 0, 0, 0.24)`
- **Secondary surface** (`{colors.secondary}` — base): inset / secondary panel
  - lite: `#e6e6e6`
  - dark: `rgba(0, 0, 0, 0.30)`
  - forest: `rgba(0, 0, 0, 0.24)`

### Text & Content
- **Pitch Forest** (`{colors.foreground}` — `#0d2a25`): primary text
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- **Card text** (`{colors.card-foreground}` — base): text inside cards
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- **Secondary text** (`{colors.secondary-foreground}` — same as card text)
- **Muted surface** (`{colors.muted}` — base): muted-area background (rgba transparent, tied to foreground)
  - lite: `rgba(13, 42, 37, 0.06)`
  - dark: `rgba(255, 255, 255, 0.06)`
  - forest: `rgba(255, 255, 255, 0.06)`
- **Stone** (`{colors.muted-foreground}` — base): caption / muted text
  - lite: `rgba(13, 42, 37, 0.55)`
  - dark: `rgba(255, 255, 255, 0.55)`
  - forest: `rgba(255, 255, 255, 0.55)`

### Brand & Accent
- **Brand Forest** (`{colors.primary}` — `#0d2a25`): brand primary (under the monochrome rule = foreground)
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- **Cream** (`{colors.primary-foreground}` — `#f0f0f0`): text on a primary fill
  - lite: `#f0f0f0`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`
- **Accent** (`{colors.accent}` — same as primary): emphasis (monochrome locked)
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- **Accent text** (`{colors.accent-foreground}` — same as primary-foreground)

### Status
- **Destructive** (`{colors.destructive}` — base): visually equal to default under the monochrome rule
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- **Destructive text** (`{colors.destructive-foreground}` — base)
  - lite: `#ffffff`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`

### Hairlines & Borders
- **Hairline** (`{colors.border}` — base): visual separation
  - lite: `rgba(13, 42, 37, 0.20)`
  - dark: `rgba(255, 255, 255, 0.10)`
  - forest: `rgba(255, 255, 255, 0.12)`

### Chrome Runtime (invariant across variants)
- **Pearl Grey** (`{chrome.canvas-bg}` — `#dcdcdc`): outer viewport base
- **Mist** (`{chrome.bg}` — `#f0f0f0`): topbar / sidebar base
- `{chrome.fg}` — `#0d2a25`
- `{chrome.muted-fg}` — `rgba(13, 42, 37, 0.55)`
- `{chrome.border}` — `rgba(13, 42, 37, 0.20)`
- `{chrome.accent}` — `#0d2a25`
- `{chrome.accent-fg}` — `#f0f0f0`

## §3 Typography values

### Font Family
- **Primary** (display + body): `"Cabinet Grotesk", "Geist", "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`
- **Mono**: `ui-monospace, "JetBrains Mono", "SF Mono", Menlo, monospace`

### Type Scale

| Token | Role Name | Size | Weight | Leading | Tracking | Use Case |
|---|---|---|---|---|---|---|
| `{type.size.4xl}` | **Hero Display** | 120px | 700 | 1.05 | -0.02em | cover hero title (CJK auto 112px) |
| `{type.size.3xl}` | **Display** | 88px | 700 | 1.05 | -0.02em | M07 oversized numeral / stat |
| `{type.size.2xl}` | **Display Small** | 76px | 700 | 1.05 | -0.02em | feature title |
| `{type.size.xl}` | **H2 / Display LG** | 44px | 600 | 1.15 | -0.015em | secondary title / lead-in |
| `{type.size.lg}` | **H3 / Card Title** | 32px | 600 | 1.15 | -0.015em | section heading / card title |
| `{type.size.md}` | **Body** | 22px | 400 | 1.55 | 0 | body / paragraph copy |
| `{type.size.sm}` | **Body Small / Caption** | 18px | 500 | 1.4 | 0 | chrome anchor text / table copy |
| `{type.size.xs}` | **Fine Print / Label** | 16px | 400 | 1.4 | 0 | caption / annotation |

### Principles

- **Single family** — all hierarchy is pulled by weight + size; no serif / italic / secondary face
- **Tight leading on display** — `{type.size.2xl}` and up always pair with `{type.leading.tight}` (1.05) + `{type.tracking.tight}` (-0.02em); display stays compact
- **Auto CJK shrink** — `:root:lang(zh|ja|ko) { --text-4xl: 112px }`; 120 is too dominant in CJK
- **Tabular numerals** — page numbers, stats, table digits all set `font-variant-numeric: tabular-nums` to prevent alignment drift
- **`tracking-wide` only on chrome / eyebrow-like meta** — body copy never goes wide (pitch has no eyebrow, but the chrome anchor's ALL CAPS metadata uses it)

### Note on Font Substitutes

**Cabinet Grotesk → Inter**: Inter at weight 600 with `font-feature-settings: "ss03"` approximates Cabinet's geometric character. Tighten tracking by an extra `-0.005em` on display sizes (Cabinet runs slightly tighter than Inter's default). Drop weight 510 if Inter Variable isn't loaded — fall back to weight 500.

**Cabinet Grotesk → Geist**: nearly identical metrics; no tracking adjustment needed. Display weight 700 reads slightly heavier in Geist; consider dropping to 600 for the cover hero on Geist if the visual feels too dense.

If running on a system that has Cabinet Grotesk pre-installed (designer / brand workstation), no substitution is needed.

## §4 Shape values

### Spacing System (4px base)
- `{shape.space.1}`     `4px`
- `{shape.space.2}`     `8px`
- `{shape.space.3}`    `12px`
- `{shape.space.4}`    `16px`
- `{shape.space.6}`    `24px`
- `{shape.space.8}`    `32px`
- `{shape.space.12}`   `48px`
- `{shape.space.16}`   `64px`
- `{shape.space.24}`   `96px`
- `{shape.space.32}`  `128px`

### Grid & Container (12 × 8 strict grid)
- `{shape.grid.columns}` `12`
- `{shape.grid.rows}` `8`
- `{shape.grid.gutter}` `24px`
- `{shape.grid.row-gap}` `24px`
- `{shape.grid.margin-x}` `96px`
- `{shape.grid.margin-top}` `96px`
- `{shape.grid.margin-bottom}` `96px`
- `{shape.grid.track-eyebrow}` `1 / 4`
- `{shape.grid.track-title}` `1 / 9`
- `{shape.grid.track-body}` `1 / 9`
- `{shape.grid.track-hero}` `1 / 9`
- `{shape.grid.track-stat}` `7 / -1`
- `{shape.grid.track-aside}` `9 / -1`

### Border Radius Scale
- `{shape.radius.sm}` `16px` — small cards / button / pill inner
- `{shape.radius.default}` `20px` — default cards
- `{shape.radius.full}` `999px` — pill / chip / capsule button

### Elevation & Depth

| Layer | Treatment | Use |
|---|---|---|
| **Flat** | no shadow, no border | Default for slide content |
| **Hairline** | 1px `{colors.border}` | Card outlines, dividers, page-rule |
| **Soft shadow** | not used | — |
| **Lifted shadow** | not used | — |

```
{shape.shadow.sm}      none      (theme bans shadows)
{shape.shadow.default} none
```

Pitch is strictly flat; cards separate from background through borders, not lifts.

## §5 Variants

- **lite** — cream (`#f0f0f0`) base, deep green (`#0d2a25`) text. **Default / cover preferred** — clean opening. The lightest of the three variants visually; used on P1 cover / P4 feature-grid / P8 pricing / P9 closing
- **dark** — pure black (`#0a0a0a`) base, cream (`#faf9f5`) text. **Problem / strong-emotion pages** — P2 problem uses dark to amplify "weight of the pain"; P5 stat-split also uses dark to make numbers hit harder
- **forest** — deep green (`#0d2a25`) base, cream text. **Solution / data-grounding pages** — P3 solution / P6 compare / P7 chart all use forest, placing "answer / evidence" pages on a steady base

### Multi-variant design principles
- Target 9-page distribution: **4 lite · 2 dark · 3 forest**
- Reference sequence: `lite → dark → forest → lite → dark → forest → forest → lite → lite` (V1-tested; avoids visually dragging consecutive same-variant runs)
- The temperature shift between variants substitutes for chromatic accents — under pitch's monochrome rule, rhythm comes from variant changes, not hue changes

## §6 Decorative signature

### Chrome Ring (8 anchors)
- **What it is**: the deck's signature ring — top-left logo + brand / top-right copyright / chrome-rule (a single hairline `<hr>`) / bottom-left brand / bottom-right page number
- **Geometry**: 4 `<div>`s anchor 4 corners + 1 `<hr>` rule sits 96px above the bottom edge. The logo-mark is a 44×44 rounded-10 square containing a single uppercase letter (the deck's first letter)
- **Usage**: never omitted on the cover — pitch deliberately keeps the full chrome ring so the deck's "industrial" feel persists; if a theme wants a chrome-less cover, switch themes (use claude / apple)
- **HTML template** (inside each page-shell):

```html
<div class="chrome-top-left">
  <span class="logo-mark">P</span>
  <strong>Brand.</strong>
</div>
<div class="chrome-top-right">© Brand, Inc<br>All Rights Reserved</div>
<hr class="chrome-rule" />
<div class="chrome-bottom-left">Brand.</div>
<div class="chrome-bottom-right">01</div>
```

### M18 Ambient Frames
- **What it is**: nested rounded-rect SVG in the background layer of cover / closing pages, providing a spatial gestalt
- **Geometry**: 3–5 stroke-only rounded rectangles (no fill), each at a different size + stroke-width, stacked to feel "framed"
- **Usage**: cover (P1) + closing (P9) only; never on intermediate pages — they would conflict with grid content
- **HTML template**: see `samples/pitch/pitch.html` § COMPONENT CSS section (search for `from blocks/ambient-frames.md` comment)

### M20 Pill Highlight
- **What it is**: a stroke-only oval capsule wrapping one keyword inside the title
- **Geometry**: `border: 1.5px solid var(--accent)` + `border-radius: 999px` + `padding: 0 0.4em` + `display: inline-block`; in-line, no wrap
- **Usage**: **at most 1 pill per page**; multiple pills compete and emphasis fails
- **HTML template**: see `samples/pitch/pitch.html` § COMPONENT CSS section (search for `from components/pill-highlight.md` comment)

### Component-level bend: dark variant + pitch-card
- **What it is**: under the dark variant, `pitch-card` does NOT use the default `var(--card)` (rgba black overlay) — it uses a forest-green solid block (`#0d2a25`)
- **Reason**: on a black base, content blocks need to be solid, not semi-transparent overlays — the overlay reads as "floating", a solid block reads as "anchored"
- **Implementation**: a separate selector override in §8 Append CSS

## §7 Do's and Don'ts

### Do's (positive guidance)

- **Use weight 700 for hero emphasis** — pitch's display weight system (400/500/600/700) carries emphasis through mass, not glyph style; bold titles, never italic
- **Use M20 pill highlight for a keyword** — wrap the most actionable noun in `<mark class="pill-highlight">`; stroke-only, single pill per page
- **Use `{shape.radius.default}` (20px) or `{shape.radius.sm}` (16px) for all cards** — pitch's softness comes from rounded corners; sharp corners read as wireframe, off-tone
- **Keep all 8 chrome anchors filled** — including the cover; pitch's industrial signature depends on the persistent ring
- **Use only the motif whitelist** — M07 oversized-numeral / M08 hairline-rule / M18 ambient-frames / M20 pill-highlight; all other motifs are out
- **Use border to separate cards** — `1px solid {colors.border}` instead of any shadow; pitch is strictly flat
- **Variant for rhythm, not decoration** — let `data-variant` switches between lite / dark / forest carry the temperature; do not introduce hue accents

### Don'ts (red lines)

- **No italic / no serif / no chromatic accent** — `{colors.accent}` is locked to `{colors.foreground}`; emphasis comes from M20 pill, never from a tinted fill
- **Destructive folds back into the monochrome system** — avoid using `data-variant="destructive"` button / badge; if a true alert red is genuinely required, declare an inline override (e.g. `#ef4444`)
- **No card with corner radius < 16px** — sharp corners read as wireframe and clash with the theme
- **No more than one M20 pill per slide** — multiple pills compete, emphasis fails
- **Never omit chrome on any slide** — including the cover; if the deck wants a chrome-less cover, switch themes
- **Never use blacklisted motifs** (M01 eyebrow / M03 drop-cap / M04 italic-accent / M16 color-block / M19 image-scrim)
- **No shadows** — `{shape.shadow.default}: none`; cards separate via border, not lift

## §8 Append CSS

```css
/* ─────────── 1. Chrome 8-anchor positions ─────────── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-size: var(--text-sm);
  color: var(--muted-foreground);
  z-index: 1;
}

.chrome-top-left {
  top: var(--grid-margin-top);
  left: var(--grid-margin-x);
  display: inline-flex;
  align-items: center;
  gap: 18px;
  font-size: 30px;
  font-weight: var(--weight-semibold);
  color: var(--foreground);
  letter-spacing: var(--tracking-tight);
}

.logo-mark {
  width: 44px;
  height: 44px;
  border-radius: 10px;
  background: var(--foreground);
  color: var(--background);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: var(--weight-bold);
  font-size: 26px;
  flex-shrink: 0;
}

.chrome-top-right {
  top: var(--grid-margin-top);
  right: var(--grid-margin-x);
  text-align: right;
  line-height: var(--leading-snug);
}

.chrome-bottom-left {
  bottom: 56px;
  left: var(--grid-margin-x);
}

.chrome-bottom-right {
  bottom: 56px;
  right: var(--grid-margin-x);
  font-variant-numeric: tabular-nums;
}

.chrome-rule {
  position: absolute;
  left:  var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);
  height: 1px;
  margin: 0;
  border: 0;
  background: var(--border);
}

/* ─────────── 2. .page-shell padding ─────────── */

.page-shell {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: var(--background);
  color: var(--foreground);
  padding: var(--grid-margin-top) var(--grid-margin-x) var(--grid-margin-bottom);
  overflow: hidden;
}

/* ─────────── 3. .page-content container + dual grid mode ─────────── */

.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  grid-template-rows:    repeat(var(--grid-rows), 1fr);
  column-gap: var(--grid-gutter);
  row-gap:    var(--grid-row-gap);
  height: 100%;
}

/* flow mode: 12-column single row + top-aligned, elements stack from top.
   Switch via: <div class="page-content" data-layout="flow">
   Used for intermediate pages (P2-P8) where title + body + cards stack vertically.
   Elements anchor only by column (.h*-*), never by row; spacing via inline margin-top.
   page-content adds 96 padding top/bottom to leave buffer between content and chrome-top/bottom
   (combined with page-shell padding 96 = total 192 from edge). */
.page-content[data-layout="flow"] {
  grid-template-rows: none;
  grid-auto-rows: auto;
  align-content: start;
  padding-top:    var(--grid-margin-top);
  padding-bottom: var(--grid-margin-bottom);
  row-gap: 0;
}

/* ─────────── 4. Utility classes: 12×8 grid anchors ─────────── */

.h1-2  { grid-column: 1 / 2; }   .h1-3 { grid-column: 1 / 3; }
.h1-4  { grid-column: 1 / 4; }   .h1-5 { grid-column: 1 / 5; }
.h1-6  { grid-column: 1 / 6; }   .h1-7 { grid-column: 1 / 7; }
.h1-8  { grid-column: 1 / 8; }   .h1-9 { grid-column: 1 / 9; }
.h1-12 { grid-column: 1 / 13; }
.h7-12 { grid-column: 7 / 13; }
.h8-12 { grid-column: 8 / 13; }

.v1    { grid-row: 1; }
.v1-2  { grid-row: 1 / 3; }
.v1-3  { grid-row: 1 / 4; }
.v2-3  { grid-row: 2 / 4; }
.v3-4  { grid-row: 3 / 5; }
.v3-5  { grid-row: 3 / 6; }
.v3-8  { grid-row: 3 / 9; }
.v4-5  { grid-row: 4 / 6; }
.v5-7  { grid-row: 5 / 8; }
.v5-8  { grid-row: 5 / 9; }
.v6-7  { grid-row: 6 / 8; }
.v1-8  { grid-row: 1 / 9; }

/* anchored-row: a floating row pinned just above chrome-rule for the bottom half of
   multi-block pages (feature-grid / compare / chart / pricing) */
.anchored-row {
  position: absolute;
  left:  var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: 120px;   /* 96 chrome-rule + 24 breathing; tight grids use 128 */
}

/* ─────────── 5. Component-level overrides ─────────── */

/* P2 problem-numbered: under the dark variant, pitch-card uses a forest-green solid
   block instead of the default semi-transparent black overlay. Design intent:
   on a black base, content blocks must read as solid to feel anchored. */
[data-variant="dark"] .pitch-card {
  background: #0d2a25;
  border-color: transparent;
}

/* feature-grid pitch-cards force forest-green fill regardless of outer variant.
   Design intent: content blocks on a lite slide must read as substantial. */
.feature-grid .pitch-card {
  background: #0d2a25;
  color: #faf9f5;
  border-color: transparent;
}

/* ─────────── 6. CJK auto-shrink ─────────── */

:root:lang(zh) { --text-4xl: 112px; }
:root:lang(ja) { --text-4xl: 112px; }
:root:lang(ko) { --text-4xl: 112px; }
```

## Known Gaps

- **Data-table component shape**: not specified — slide authors write ad-hoc HTML
- **Animation tokens**: not specified — the deck is static
- **Mobile / portrait orientation**: not supported (1920×1080 only)
- **Print-specific theme overrides**: only the chrome runtime handles `@media print`; theme-level color/contrast not defined
- **True chromatic destructive variant**: visually folds back into monochrome by design; if an alert red is genuinely required, the page declares an inline override (e.g. `#ef4444`)
- **Photographic atmosphere**: pitch's geometric vocabulary leaves no room for image-scrim or photo overlays — if a deck needs photographic mood, switch themes (claude / apple)
