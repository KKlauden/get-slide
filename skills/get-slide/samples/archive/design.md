---
name: archive
description: "Industrial system pitch — file-archive aesthetic, black-and-white 2-variant + grotesk display + mono system-label chrome + sharp corners + abs pinning"
version: 0.3.0
source: "Built from an industrial file-archive aesthetic; v0.3 migrated to v2.1 schema (descriptive nicknames + role names + Do's & Don'ts + Known Gaps). Reference: 9-page FAULTLINE geological-monitoring fictional deck."
---

# Archive Theme

> An industrial-system pitch deck theme. File-archive aesthetic: pure black + pure white, serial numbers, mono labels, oversized hero type. Every page reads like a versioned engineering archive entry.

## §1 Atmosphere

The archive theme's visual character comes from an engineering archive room — black-and-white throughput, zero-radius corners, serial-numbered IDs, machine-printed type. It does not feel "modern"; it feels like an internal whitepaper carrying a BUILD number: cover is a title page (dark gradient + display hero + system labels), contents is an index (lite base + 9-item 3×3 serial-number grid + 280px mega title), the body pages are archive entries (lite base + abs-pinned numbers / lists / charts). Its "industrial" feel comes not from icons or textures but from **pure geometry + dramatic type-size deltas + serial-number order** — every element is encoded inside the mono-label system idiom.

Type is the bone. **Geist** carries display + body, with weight running up to 800 black; the largest hero is 280px (the contents super-title), with 88px / 112px as the standing display sizes. Display and mono coexist: display is for hero / titles / list labels, while mono (Geist Mono) is exclusively for chrome anchors / system labels / archive-tags — anywhere "metadata / IDs / system status" lives. The deliberate split creates two voices: "the system speaking vs. the designer speaking". The CJK fallback is PingFang; on Chinese the hero stays at 88–112px (mega 280 does not shrink, because CJK glyphs already carry enough density to support it).

The color logic is **strictly pure black or pure white** — `#000` / `#fafafa` are locked. All mid-greys are derived via `color-mix(in srgb, var(--foreground) NN%, transparent)` (35% for dividers, 55% for chrome-rule, 25% for muted big-numbers). The two variants take entirely different roles: **dark** is for cover / chapter dividers, paired with `linear-gradient(180deg, #000 0%, #2e2e2e 50%, #000 100%)` mid-page wash so the hero floats on the lit band; **lite** is for contents and every body page, pure `#fafafa` base, with hero / display / lists in solid `#0a0a0a`. Two exceptions only: destructive uses `#ef4444` (inline override on demand), and P8 market-page sets `data-bg-mode="market"` which switches to a bottom-rising blue gradient (`#2563eb` to `#0a0a0a`).

Decoration is geometry + serial numbers + system labels — **the chrome ring uses mono uppercase 18px throughout** (`PITCH DOCUMENT / VERSION: V1.0.0 / BUILD_0425 / INDEX_REF: FILE#0201`), 8 anchors always present but content per-page from `content.md` (filling allowed to be empty). **Chrome-rule supports 3 modes**: `data-chrome-rule="off"` (cover off) / default (at page-shell 720px, used by contents) / `="top"` (140px, used on P3 about-page). **`archive-ring-motif`** (cover-right SVG nested ovals + radial gradient simulating a metallic sheen) + **`archive-mega-title`** (180–280px super-display on contents) + **`archive-toc`** (3×3 grid serial 1.0–9.0) are archive's three signature motifs. All content elements are **abs-pinned directly to `.page-shell`** — not routed through `.page-content` grid — which is the biggest workflow difference between archive and pitch: each page's specific positioning relies on `archive-*` class absolute top/left/right/bottom anchors.

**Key Characteristics**:
- Single Geist family + Geist Mono dual track (display + system labels)
- 2-variant high contrast: dark (cover / divider with mid-page wash gradient) + lite (contents / body pages)
- 8-anchor chrome ring in mono uppercase 18px, 3-mode chrome-rule visibility
- Pure black + pure white locked; mid-greys derived via `color-mix`
- Sharp corners: `{shape.radius.default}: 0` (industrial feel comes from zero-radius)
- Abs-pinning: content elements absolute-positioned on page-shell, **NOT** routed through grid-cell
- Display hero ≥ 112px; contents super-title 180–280px
- Motif whitelist: M07 mono-numbered-toc / M08 hairline-rule / M21 metallic-render / M22 super-large-display

## §2 Palette values

### Surface
- **Pitch Black** (`{colors.background}` — `#000000`): page canvas (slide base color)
  - dark: `#000000`
  - lite: `#fafafa`
- **Snow** (lite background): `#fafafa`
- **Card** (`{colors.card}` — base): in-slide content container (very light overlay transparent)
  - dark: `rgba(255, 255, 255, 0.04)`
  - lite: `rgba(10, 10, 10, 0.02)`
- **Secondary surface** (`{colors.secondary}` — base): inset / secondary panel
  - dark: `rgba(255, 255, 255, 0.08)`
  - lite: `rgba(10, 10, 10, 0.06)`

### Text & Content
- **White** (dark foreground) / **Ink** (lite foreground): primary text
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- **Card text** (`{colors.card-foreground}` — same as foreground)
- **Secondary text** (`{colors.secondary-foreground}` — same as foreground)
- **Muted surface** (`{colors.muted}` — base): muted-area background
  - dark: `rgba(255, 255, 255, 0.06)`
  - lite: `rgba(10, 10, 10, 0.06)`
- **Stone** (`{colors.muted-foreground}` — base): caption / muted text / muted big-numbers
  - dark: `rgba(255, 255, 255, 0.55)`
  - lite: `rgba(10, 10, 10, 0.55)`

### Brand & Accent
- **Brand** (`{colors.primary}` — base): brand primary (under the black-and-white rule = foreground)
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- **Brand text** (`{colors.primary-foreground}` — base)
  - dark: `#000000`
  - lite: `#fafafa`
- **Accent** (`{colors.accent}` — same as primary): emphasis (locked to foreground)
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- **Accent text** (`{colors.accent-foreground}` — same as primary-foreground)

### Status
- **Alert Red** (`{colors.destructive}` — `#ef4444`): warning / error (the **only chromatic accent**, shared across both variants)
- **Alert text** (`{colors.destructive-foreground}` — `#ffffff`)

### Hairlines & Borders
- **Hairline** (`{colors.border}` — base)
  - dark: `rgba(255, 255, 255, 0.18)`
  - lite: `rgba(10, 10, 10, 0.18)`

### Chrome Runtime (invariant across variants)
- **Charcoal** (`{chrome.canvas-bg}` — `#1a1a1a`): outer viewport dark grey
- **Near-Black** (`{chrome.bg}` — `#0d0d0d`): topbar / sidebar
- `{chrome.fg}` — `#ffffff`
- `{chrome.muted-fg}` — `rgba(255, 255, 255, 0.55)`
- `{chrome.border}` — `rgba(255, 255, 255, 0.10)`
- `{chrome.accent}` — `#ffffff`
- `{chrome.accent-fg}` — `#000000`

## §3 Typography values

### Font Family
- **Primary** (display + body): `"Geist", "Inter", "Helvetica Neue", -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`
- **Mono**: `"Geist Mono", ui-monospace, "JetBrains Mono", "SF Mono", Menlo, monospace`

### Type Scale

Archive extends the standard 8-step ladder with two extra rungs (`--text-mega` and `--weight-black`) for the super-display motif. The role names in prose stay aligned with the standard ladder.

| Token | Role Name | Size | Weight | Leading | Tracking | Use Case |
|---|---|---|---|---|---|---|
| `--text-mega` | **Mega Display** | 280px | 700 | 0.85 | -0.04em | contents super-title / P8 mega-stat |
| `{type.size.4xl}` | **Hero Display** | 112px | 700 | 0.95 | -0.02em | cover hero / mega body number |
| `{type.size.3xl}` | **Display** | 72px | 700 | 0.95 | -0.02em | page-title secondary tier |
| `{type.size.2xl}` | **Display Small** | 48px | 700 | 1.0 | -0.02em | mid-tier hero / cross-page lead-in |
| `{type.size.xl}` | **H2 / Display LG** | 32px | 600 | 1.0 | -0.02em | contents toc / stat-card title / mid heading |
| `{type.size.lg}` | **H3 / Card Title** | 24px | 600 | 1.2 | 0 | caption / mid title |
| `{type.size.md}` | **Body** | 18px | 400 | 1.5 | 0 | body / chrome anchor / mono labels (**dominant size**) |
| `{type.size.sm}` | **Body Small / Caption** | 14px | 400 | 1.4 | 0 | table copy / micro caption |
| `{type.size.xs}` | **Fine Print / Label** | 11px | 400 | 1.4 | 0 | extreme-fine annotation (reserved; current sample doesn't use) |

### Weight Scale (with `--weight-black 800`)
- `--weight-normal` `400` — body / desc
- `--weight-medium` `500` — chrome anchor / mono labels (system label default)
- `--weight-semibold` `600` — toc items
- `--weight-bold` `700` — hero / page-title / list label / stat-num (**dominant weight**)
- `--weight-black` `800` — reserved for extra-heavy hero (current sample doesn't use)

### Leading + Tracking
- Leading: `tight 0.95` / `snug 1.0` / `normal 1.2` / `body 1.5` (note: archive's `tight` is tighter than pitch's `tight`)
- Tracking: `tighter -0.04em` / `tight -0.02em` / `normal 0` / `wide 0.04em` / `mono 0.08em`

### Principles

- **Display always tight tracking + tight leading** — display ≥ 88px requires `letter-spacing: tracking-tight (-0.02em)` + `line-height: 0.85` or `tight 0.95`; otherwise archive's display turns "soft"
- **Tabular numerals** — page numbers / stat-num / toc num all carry `font-variant-numeric: tabular-nums` to prevent alignment drift
- **Mono is reserved for system semantics** — chrome anchors / archive-tags / coverage-panel labels — anywhere "metadata / IDs / system status" lives; always mono uppercase + `tracking-mono (0.08em)`
- **Display and Mono never mix** — titles / hero are always display; mono is only labels / IDs
- **CJK does not shrink mega** — `--text-mega` stays at 280px in CJK; CJK glyphs carry enough visual density to support it

### Note on Font Substitutes

**Geist → Inter**: nearly identical metrics; no tracking adjustment needed. If display weight 700 reads slightly heavier on Inter, drop to weight 650 (Inter Variable) or 600 (static).

**Geist Mono → JetBrains Mono**: similar character widths; no adjustment needed for mono labels. If neither is available, fall through to `ui-monospace, SF Mono` — most modern systems carry one.

If the host system has Geist + Geist Mono pre-installed (designer / brand workstation), no substitution is needed.

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

### Grid & Container (page-shell padding only — archive does NOT use page-content grid)

- `{shape.grid.margin-x}`      `64px`  — tighter than pitch's 96
- `{shape.grid.margin-top}`    `56px`
- `{shape.grid.margin-bottom}` `56px`

> Archive abs-pins content elements directly to `.page-shell` and **does NOT** route through the 12-column `.page-content` grid. The grid token here is used only for `.page-shell` padding.

### Border Radius Scale
- `{shape.radius.sm}` `0` — sharp corners
- `{shape.radius.default}` `0` — sharp corners (archive's industrial feel comes from zero radius)
- `{shape.radius.lg}` `0` — sharp corners

### Elevation & Depth

| Layer | Treatment | Use |
|---|---|---|
| **Flat** | no shadow, no border | Default for slide content |
| **Hairline** | 1px `{colors.border}` (or `color-mix` derivative) | Card outlines, dividers, page-rule, panel borders |
| **Soft shadow** | not used | — |
| **Lifted shadow** | not used | — |

```
{shape.shadow.sm}      none
{shape.shadow.default} none
```

Archive is strictly flat; cards and panels separate via hairline borders, never via shadows.

## §5 Variants

- **dark** — pure black (`#000000`) base, pure white (`#ffffff`) text. **Cover / chapter divider / strong-emotion pages** — P1 cover uses dark + mid-page wash gradient (`linear-gradient(180deg, #000 0%, #2e2e2e 50%, #000 100%)`), placing the hero text on the lit band; P8 market-page also uses dark but layers `data-bg-mode="market"` for a bottom-blue gradient
- **lite** — cream (`#fafafa`) base, deep black (`#0a0a0a`) text. **Contents / all body pages (P2–P9 except P8)** — pure solid no gradient, lets display hero and the abs-pinned `archive-*` content elements present cleanly

### Multi-variant design principles
- Target 9-page distribution: **3 dark · 6 lite** (cover + chapter divider + market use dark for emotional rhythm; contents and body use lite for steady reading)
- Reference sequence: `dark → lite → lite → lite → lite → lite → lite → dark → lite` (the archive sample's actual rhythm; P1 cover dark / P8 market dark / others lite)
- No mid-grey variant allowed (e.g. `#1a1a1a` or `#888` base) — archive's visual lifeblood is black-and-white contrast; a transitional grey would soften the design

## §6 Decorative signature

### Chrome Ring (8 anchors) — Mono system labels
- **What it is**: every page's 8 anchor elements are always present (chrome runtime hard constraint), but content per-page comes from `content.md` and may be left as empty strings
- **Geometry**: 4 `<div>`s anchor 4 corners + 1 `<hr>` (chrome-rule). All 4 corner divs are mono uppercase 18px medium weight, color `{colors.foreground}`. Anchors achieve "visual omission" by carrying empty strings
- **3 chrome-rule modes**:
  - `data-chrome-rule="off"` — off (cover default)
  - default (no modifier) — at page-shell 720px (used by contents, hugging the mega-title's top)
  - `data-chrome-rule="top"` — at 140px (used on P3 about-page, a hairline below the chrome ring)
- **Example strings**:
  - cover: `PITCH DOCUMENT / FAULTLINE ©2025` (top-left), `VERSION: V1.0.0 / BUILD_0425` (top-right)
  - contents: `INDEX_REF: FILE#0201` (top-left), `SYS_MODE: ACTIVE` (top-right)
  - body pages: same pattern as contents + bottom-right serial number `02 / 04 / 09`
- **HTML template** (inside each page-shell):

```html
<div class="chrome-top-left">PITCH DOCUMENT<br>FAULTLINE ©2025</div>
<div class="chrome-top-right">VERSION: V1.0.0 / BUILD_0425</div>
<hr class="chrome-rule" />
<div class="chrome-bottom-left"></div>
<div class="chrome-bottom-right"></div>
```

### M21 Metallic Ring Motif (cover-only)
- **What it is**: nested SVG ovals + radial gradient floating on the right side of the cover, simulating a metallic sheen
- **Geometry**: 3–5 nested ovals, outer largest, inner smallest; each layer is `stroke-only` with a different stroke-width; the innermost layer adds a `radial-gradient` for the metallic reflection. Position spills past the page-shell edge (`right: -80px`) so it visually "embeds" into the slide edge
- **Usage**: cover (P1) only; never used elsewhere — this is archive's only "graphic" decoration on the cover
- **HTML template**: see archive sample P1 `.archive-ring-motif > svg`

### M22 Super-Large Display (contents super-title)
- **What it is**: 180–280px display heading anchored at the bottom of the contents page (e.g. "DESIGN ARCHIVE / FAULTLINE")
- **Geometry**: `font-size: 180px` or `280px`, `line-height: 0.85`, `letter-spacing: tighter (-0.04em)`, `text-transform: uppercase`, `font-weight: bold (700)`
- **Usage**: contents (P2) uses 180px; P8 market-page uses 280px (`archive-mega-stat`)

### M07 Mono Numbered TOC (contents serial-numbered grid)
- **What it is**: 9 TOC items in a 3×3 grid on the contents page, each prefixed by `1.0 / 2.0 / ... / 9.0` serial number + uppercase label
- **Geometry**: `grid-template-columns: repeat(3, 1fr)` + `grid-auto-flow: column` (HTML order 1–9 → visual 1/4/7 row 1), `gap: 64px 64px`; each item `display: flex; align-items: baseline; gap: 64px`; num `min-width: 60px` + tabular-nums

### Component-level bend: abs-pinning by default
- **What it is**: archive content elements are **all** abs-pinned to page-shell (NOT routed through the page-content grid). This is the largest difference from pitch
- **Reason**: every archive page is bespoke; a strict grid would constrain layouts. Abs + numerical coordinates is more direct
- **Implementation**: §8 Append CSS defines ~30 `archive-*` selectors, each declaring its own `position: absolute; top/left/right/bottom`

## §7 Do's and Don'ts

### Do's (positive guidance)

- **Use mono uppercase + 0.08em tracking for system labels** — chrome anchors / archive-tags / coverage-panel labels; the system-label voice is archive's identity
- **Use display weight 700 for hero and titles** — archive emphasizes by mass + size; never by italic / serif
- **Derive mid-greys via `color-mix(in srgb, var(--foreground) NN%, transparent)`** — 35% for dividers, 55% for chrome-rule, 25% for muted big-numbers
- **Abs-pin content elements directly to `.page-shell`** — every archive page is bespoke; don't try to route through page-content grid
- **Use `tracking-tight (-0.02em)` and `line-height 0.85` on display ≥ 88px** — archive's "large" feel collapses if you let display sit at default tracking / leading
- **Use `tabular-nums` on all numerals** — page-numbers / stat-num / toc-num must align column-true
- **Use the `archive-*` selector pattern** — every per-page block lives as its own selector in §8 Append CSS

### Don'ts (red lines)

- **No mid-grey backgrounds** — base is only `#000` or `#fafafa`; mid-greys must be `color-mix` derivatives, never standalone hex
- **No serif / no italic / no chromatic accent** — except destructive `#ef4444` and the P8 market blue gradient, color is locked to black-and-white
- **No rounded cards / blocks** — `{shape.radius.default}: 0`; rounded corners destroy the file-archive feel
- **No hero below 112px** — archive's "large" is the visual lifeblood; shrinking abandons the theme
- **No sans on chrome anchors** — system labels must be mono; sans-chrome loses the archive voice
- **Never use blacklisted motifs** (M03 drop-cap / M04 italic-accent / M16 color-block / M18 ambient-frames / M20 pill-highlight) — pitch motifs (ambient-frames / pill) are out
- **Never use `.page-content` grid** — archive content always abs-pins; if you want the 12-col grid, switch themes (use pitch)

## §8 Append CSS

```css
/* ─────────── 1. Chrome 8-anchor positions ─────────── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-family: var(--font-mono);
  font-size: var(--text-md);                    /* 18px — same tier as archive-tags; unifies system-label sizing */
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--foreground);
  line-height: 1.5;
  z-index: 3;
}
.chrome-top-left {
  top: var(--grid-margin-top);
  left: var(--grid-margin-x);
}
.chrome-top-right {
  top: var(--grid-margin-top);
  right: var(--grid-margin-x);
  text-align: right;
}
.chrome-bottom-left {
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
}
.chrome-bottom-right {
  bottom: var(--grid-margin-bottom);
  right: var(--grid-margin-x);
  text-align: right;
}

/* chrome-rule defaults at ~60% page height (used by contents); cover uses [data-chrome-rule="off"] to hide */
.chrome-rule {
  position: absolute;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  top: 720px;       /* default sits near the contents mega-title's top */
  height: 1px;
  margin: 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 55%, transparent);   /* adapts across dark/lite */
  z-index: 2;
}
.page-shell[data-chrome-rule="off"] .chrome-rule { display: none; }
.page-shell[data-chrome-rule="top"] .chrome-rule { top: 140px; }      /* P3 about-page: hairline below the chrome ring */

/* ─────────── 2. .page-shell padding + dark variant gradient ─────────── */

.page-shell {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: var(--background);
  color: var(--foreground);
  padding: var(--grid-margin-top) var(--grid-margin-x) var(--grid-margin-bottom);
  overflow: hidden;
}

/* dark variant mid-page wash gradient (vertical: hero band lit at #2e2e2e, fading up/down to pure black) */
.page-shell[data-variant="dark"] {
  background: linear-gradient(
    180deg,
    #000000 0%,
    #2e2e2e 50%,
    #000000 100%
  );
}

/* ─────────── 3. .page-content container (kept for compat; archive does not depend on grid) ─────────── */

.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(8, 1fr);
  column-gap: 24px;
  row-gap: 24px;
  height: 100%;
}

/* ─────────── 5. archive-specific blocks (abs-pinning is the main pattern) ─────────── */

/* P1 cover 3 inline system labels */
.archive-tags {
  position: absolute;
  top: 50%;                                 /* vertically centered, aligned with the gradient's mid */
  transform: translateY(-50%);
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: flex;
  gap: 200px;                               /* large gaps amplify the system feel */
  font-family: var(--font-mono);
  font-size: var(--text-md);                /* 18px — same tier as the chrome ring */
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P1 cover hero (bottom-anchored) */
.archive-hero {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 88px;                          /* matched to dribbble ref */
  font-weight: var(--weight-bold);          /* 700 — slightly lighter than black (800) */
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P1 cover right-side oval motif (overflows right: -80) */
.archive-ring-motif {
  position: absolute;
  top: 0;
  right: -80px;
  width: 700px;
  height: 100%;
  z-index: 0;                               /* hero / chrome cover the motif */
  pointer-events: none;
}
.archive-ring-motif svg {
  display: block;
  width: 100%;
  height: 100%;
}

/* P2 contents 9-item 3×3 grid */
.archive-toc {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, auto);
  grid-auto-flow: column;                   /* HTML order 1-9 → visual 1/4/7 row 1 */
  align-content: start;                     /* row height strictly per content */
  gap: 64px 64px;                           /* row + col gap both 64 */
  list-style: none;
  margin: 0;
  padding: 0;
  z-index: 2;
}
.archive-toc li {
  display: flex;
  align-items: baseline;
  gap: 64px;
  font-family: var(--font-display);
  font-size: var(--text-xl);                /* 32px */
  font-weight: var(--weight-semibold);      /* 600 */
  letter-spacing: var(--tracking-tight);
  line-height: 1;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-toc .num {
  font-variant-numeric: tabular-nums;
  min-width: 60px;
}

/* P2 contents super-title (bottom-anchored, 180px) */
.archive-mega-title {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 180px;                         /* two tiers smaller than --text-mega (280) */
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tighter);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P2 contents top-right dashed circle */
.archive-brand-mark {
  width: 36px;
  height: 36px;
  color: var(--foreground);
}

/* ─── P3 about-page (shared body-page page-title/body) ─── */

/* P3 chrome-top-right banner-style page-meta (variant of chrome-top-right + .archive-page-meta) */
.chrome-top-right.archive-page-meta {
  left: 1100px;                             /* spans the right ~40% of the slide */
  display: flex;
  align-items: center;
  justify-content: flex-end;                /* right-aligned packing; brand-mark hugs the right edge */
  gap: 100px;                               /* push left to avoid hugging the right edge */
  text-align: left;
}
.chrome-top-right.archive-page-meta > span:nth-child(2) {
  width: 64px;                              /* numeral column fixed-width */
  text-align: center;                       /* 02/04/09 all centered inside the 64px frame */
}

.archive-page-title {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  display: flex;
  gap: 200px;                               /* wide gap between ABOUT and FAULTLINE */
  font-family: var(--font-display);
  font-size: 88px;                          /* same tier as cover hero */
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  line-height: 1;
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

.archive-page-number {
  position: absolute;
  top: 280px;
  right: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 1;
  color: var(--foreground);
  z-index: 2;
}

.archive-body {
  position: absolute;
  top: 460px;
  left: var(--grid-margin-x);
  width: 700px;                             /* 18px size needs more width */
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  text-align: justify;
  color: var(--foreground);
  z-index: 2;
}

/* P3 archive-stat-list 3 rows with hairline dividers (KPI numeral list) */
.archive-stat-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 880px;                             /* leave room for image-frame (right 880) + 32 gap */
  z-index: 2;
}
.archive-stat-row {
  display: grid;
  grid-template-columns: 220px 240px 1fr;
  align-items: center;
  gap: 0 32px;
  padding: 32px 0;
  border-top: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-row:last-child {
  border-bottom: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-row .stat-num {
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 1;
  letter-spacing: var(--tracking-tight);
}
.archive-stat-row .stat-label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-semibold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
}
.archive-stat-row .stat-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--foreground);
}

/* P3 archive-image-frame grey placeholder frame */
.archive-image-frame {
  position: absolute;
  top: 280px;
  right: var(--grid-margin-x);
  width: 880px;
  bottom: var(--grid-margin-bottom);
  background: var(--muted);
  border: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: 18px;
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--muted-foreground);
  z-index: 1;
}

/* ─── P4 problem-page ─── */

.archive-numbered-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 800px;
  z-index: 2;
}
.archive-numbered-row {
  display: grid;
  grid-template-columns: 80px 1fr 1fr;      /* num prefix / label / desc */
  align-items: center;
  gap: 0 32px;
  padding: 28px 0;
  border-top: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-numbered-row:last-child {
  border-bottom: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-numbered-row .num {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-semibold);
  color: var(--foreground);
}
.archive-numbered-row .label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-numbered-row .desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}

.archive-metric-panel {
  position: absolute;
  top: 460px;
  right: var(--grid-margin-x);
  width: 800px;
  z-index: 2;
}
.archive-metric-panel .metric-title {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-metric-panel .metric-rule {
  height: 1px;
  margin: 16px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-metric-panel .metric-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--muted-foreground);
}
.archive-metric-panel .bar-chart {
  margin-top: 80px;                         /* top spacing inside metric-panel */
}

.archive-metric-bignum {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  right: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 240px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tighter);
  color: var(--foreground);
  z-index: 2;
}

/* ─── P5 solution-page (right-side layer-list) ─── */

.archive-layer-list {
  position: absolute;
  top: 480px;
  right: var(--grid-margin-x);
  width: 540px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  z-index: 2;
}
.archive-layer-item .layer-title {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
  text-align: right;                        /* right-align with layer-rule's right end (= chrome-rule right end) */
}
.archive-layer-item .layer-rule {
  height: 1px;
  margin: 12px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-layer-item .layer-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
  text-align: right;                        /* same alignment as layer-title */
}

/* ─── P6 performance-data (3 stat cards + dark coverage panel) ─── */

.archive-stat-cards {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  z-index: 2;
}
.archive-stat-card {
  background: color-mix(in srgb, var(--foreground) 4%, transparent);   /* light grey base */
  padding: 56px 56px 48px;
  display: flex;
  flex-direction: column;
  min-height: 360px;
}
.archive-stat-card .card-title {
  font-family: var(--font-display);
  font-size: 32px;                          /* same tier as P2 archive-toc */
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-stat-card .card-rule {
  height: 1px;
  margin: 24px 0 24px;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-card .card-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}
.archive-stat-card .card-foot {
  margin-top: auto;                         /* push num + cap to the bottom */
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 8px;
}
.archive-stat-card .card-num {
  font-family: var(--font-display);
  font-size: 88px;                          /* same tier as page-title */
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: var(--foreground);
  text-align: right;
}
.archive-stat-card .card-cap {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
  text-align: right;
}

.archive-coverage-panel {
  position: absolute;
  top: 660px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);        /* same 56 bottom gap as other pages */
  background: #0a0a0a;                      /* force dark; contrasts with the lite page */
  --foreground: #ffffff;                    /* inversion: child elements using var(--foreground) flip to white */
  --muted-foreground: rgba(255, 255, 255, 0.6);
  color: var(--foreground);
  padding: 56px;
  z-index: 2;
}
.archive-coverage-panel .coverage-title {
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: #ffffff;
}
.archive-coverage-panel .coverage-desc {
  margin-top: 12px;
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.6);
  max-width: 700px;
}
.archive-coverage-panel .coverage-bignum {
  position: absolute;
  top: 56px;
  left: calc(56px + 0.73 * (100% - 112px)); /* same x as gauge marker (73% of inner gauge width) */
  transform: translateX(-50%);
  font-family: var(--font-display);
  font-size: 56px;
  font-weight: var(--weight-bold);
  line-height: 1;
  color: #ffffff;
}
.archive-coverage-panel .chart-gauge {
  position: absolute;
  left: 56px;
  right: 56px;
  bottom: 56px;
}
.archive-coverage-panel .chart-gauge .gauge-labels {
  font-family: var(--font-mono);
  font-size: 18px;
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  color: rgba(255, 255, 255, 0.7);
}

/* ─── P7 system-performance ─── */

.archive-perf-list {
  position: absolute;
  top: 480px;
  left: 720px;
  right: var(--grid-margin-x);
  display: flex;
  flex-direction: column;
  gap: 64px;
  z-index: 2;
}
.archive-perf-row .perf-title {
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-perf-row .perf-rule {
  height: 1px;
  margin: 24px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-perf-row .perf-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 32px;
}
.archive-perf-row .perf-body {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--foreground);
}
.archive-perf-row .perf-stat {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 8px;
}
.archive-perf-row .perf-num {
  font-family: var(--font-display);
  font-size: 120px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: color-mix(in srgb, var(--foreground) 22%, transparent);    /* muted big-number */
}
.archive-perf-row .perf-cap {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  color: var(--muted-foreground);
  text-align: right;
}

/* ─── P8 global-impact (market-page; dark + bottom blue gradient) ─── */

.page-shell[data-bg-mode="market"] {
  background: linear-gradient(
    0deg,
    #2563eb 0%,                              /* base: vivid blue */
    #1e40af 7%,                              /* up 75px: deep blue */
    #15225a 14%,                             /* up 150px: dark navy */
    #0a0a0a 26%,                             /* up 280px: pure black */
    #0a0a0a 100%
  );
}
.page-shell[data-bg-mode="market"] .chrome-rule,
.page-shell[data-bg-mode="market"] .archive-impact-rule {
  background: color-mix(in srgb, var(--foreground) 30%, transparent);
}

.archive-mega-stat {
  position: absolute;
  top: 280px;                               /* top-aligned with page-title (top: 280) */
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 280px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tighter);
  line-height: 0.85;
  color: var(--foreground);
  z-index: 2;
}
.archive-mega-stat-label {
  position: absolute;
  top: 600px;
  left: 320px;                              /* indented to visually align with the "5" of $5.5 */
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}
.archive-impact-rule {
  position: absolute;
  top: 720px;
  left: 360px;                              /* left edge does not touch page edge; arrow occupies left 64-284 */
  right: var(--grid-margin-x);
  height: 1px;
  margin: 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 55%, transparent);
  z-index: 2;
}
.archive-arrow {
  position: absolute;
  top: 684px;                               /* center 36 (viewBox y=36) lands at 720 */
  left: var(--grid-margin-x);
  color: var(--foreground);
  z-index: 2;
}
.archive-impact-body {
  position: absolute;
  top: 760px;
  right: var(--grid-margin-x);
  width: 700px;
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--foreground);
  z-index: 2;
}
.archive-impact-body .muted {
  color: var(--muted-foreground);
}

/* ─── P9 global-reach (4 stage cols with bar-chart) ─── */

.archive-stage-grid {
  position: absolute;
  top: 280px;                               /* top-aligned with the left-side archive-page-title GLOBAL REACH */
  left: 480px;
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  z-index: 2;
}
.archive-stage-col {
  position: relative;
  border-left: 1px solid color-mix(in srgb, var(--foreground) 30%, transparent);
  padding: 0 32px;                          /* top padding removed */
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.archive-stage-col .stage-num {
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: color-mix(in srgb, var(--foreground) 25%, transparent);    /* muted big-number */
}
.archive-stage-col .stage-label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-stage-col .stage-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}
.archive-stage-col .bar-chart {
  position: absolute;
  left: 32px;
  right: 32px;
  bottom: 0;
  height: 480px;                            /* fixed bars zone height */
  --bar-w: 10px;                            /* unified 10px bar width inside the column */
}
/* tone variants: left → right reads blue → light grey → mid grey → black (depth ramp) */
.archive-stage-col[data-tone="blue"] .bar-chart .bar {
  background: #2563eb;
}
.archive-stage-col[data-tone="muted-light"] .bar-chart .bar {
  background: rgba(10, 10, 10, 0.18);
}
.archive-stage-col[data-tone="muted-mid"] .bar-chart .bar {
  background: rgba(10, 10, 10, 0.4);
}
/* dark stage uses the bar-chart default var(--bar-color, var(--foreground)) */
```

## Known Gaps

- **Data-table component shape**: not specified — slide authors write ad-hoc HTML
- **Animation tokens**: not specified — the deck is static
- **Mobile / portrait orientation**: not supported (1920×1080 only)
- **Print-specific theme overrides**: only the chrome runtime handles `@media print`; theme-level color/contrast not defined
- **True chromatic destructive scaling**: only `#ef4444` is whitelisted; no red-tone family for badge / chip variants
- **Grid-based body pages**: archive deliberately abs-pins everything; page-content grid is unused. If a deck needs the 12-col grid, switch to the pitch theme
- **Ambient / atmospheric photography**: archive's geometric vocabulary leaves no room for image-scrim or photo overlays — the engineering archive aesthetic is non-photographic by design
