---
name: <slug>
description: "<one-sentence theme positioning>"
version: 0.1.0
---

# <Theme Name>

> [a one-line tagline, e.g. "YC / TechCrunch-style pitch deck theme"]
>
> Source: [reference image URL / brand guideline / mood board / website that this theme is built from]

## §1 Atmosphere

[Paragraph 1: overall character — imagery, metaphor, mood, period feel]

[Paragraph 2: voice of typography — font family, why, weight usage]

[Paragraph 3: color logic — monochrome / two-color / multi-color? Restrained or bold accent?]

[Paragraph 4 (optional): decorative style — decorative elements, spatial logic, special techniques]

**Key Characteristics**:
- [5-10 bullets]

## §2 Palette values

### Surface
- **<Nickname>** (`{colors.background}` — `<hex>`): page canvas (slide base)
  - <variant1>: `<hex>`
  - <variant2>: `<hex>`
- **<Nickname>** (`{colors.card}` — `<hex>`): content container
  - ...

### Text & Content
- **<Nickname>** (`{colors.foreground}` — `<hex>`): primary text
- **<Nickname>** (`{colors.muted-foreground}` — `<hex>`): secondary text

### Brand & Accent
- **<Nickname>** (`{colors.primary}` — `<hex>`): brand identity
- **<Nickname>** (`{colors.accent}` — `<hex>`): emphasis dot / pill / accent stroke

### Status
- **<Nickname>** (`{colors.destructive}` — `<hex>`): warning / error

### Hairlines & Borders
- **<Nickname>** (`{colors.border}` — `<hex>`): visual separation

### Chrome Runtime (invariant across variants)
- `{colors.canvas-bg}` — `<hex>` (outer canvas, outside the sidebar)
- `{colors.chrome-bg}` — `<hex>` (sidebar / topbar base)
- `{colors.chrome-fg}` — `var(--foreground)`
- `{colors.chrome-muted-fg}` — `var(--muted-foreground)`
- `{colors.chrome-border}` — `var(--border)`
- `{colors.chrome-accent}` — `var(--accent)`
- `{colors.chrome-accent-fg}` — `var(--accent-foreground)`

## §3 Typography values

### Font Family
- **Primary**: `<family>`, [fallback chain including `"PingFang SC"` or `"Hiragino Sans GB"` or `"Microsoft YaHei"`]
- **Mono**: `<family>`, [fallback chain]
- **OpenType features** (optional): `"<feat1>", "<feat2>"`

### Type Scale

| Token | Role Name | Size | Weight | Leading | Tracking | Use Case |
|---|---|---|---|---|---|---|
| `{type.size.4xl}` | Hero Display | `<v>` | `<v>` | `<v>` | `<v>` | hero / cover only |
| `{type.size.3xl}` | Display | `<v>` | `<v>` | `<v>` | `<v>` | section opener |
| `{type.size.2xl}` | Display Small | `<v>` | `<v>` | `<v>` | `<v>` | medium display |
| `{type.size.xl}` | H2 / Display LG | `<v>` | `<v>` | `<v>` | `<v>` | h2 |
| `{type.size.lg}` | H3 / Card Title | `<v>` | `<v>` | `<v>` | `<v>` | h3 / card title |
| `{type.size.md}` | Body | `<v>` | `<v>` | `<v>` | `<v>` | body paragraph |
| `{type.size.sm}` | Body Small / Caption | `<v>` | `<v>` | `<v>` | `<v>` | body small |
| `{type.size.xs}` | Fine Print / Label | `<v>` | `<v>` | `<v>` | `<v>` | label / micro UI |

### Principles
- [3-5 use principles, e.g. "compress at scale", "single weight system", "leading inversely follows size"]

### Note on Font Substitutes
[If the primary family is proprietary or non-system, give substitution advice. Example for `Cabinet Grotesk → Inter`: "Use Inter at weight 600 with `font-feature-settings: 'ss03'`. Tighten tracking by `-0.01em` on display sizes."]

[If the primary IS already system / open-source, write: "No substitution needed."]

## §4 Shape values

### Spacing System (4px base)
- `{shape.space.1}` `<v>` ...
- `{shape.space.32}` `<v>`

### Grid & Container
- columns / rows / margins / gutters
- track names: `{shape.grid.track-eyebrow}` / `{shape.grid.track-title}` / etc

### Border Radius Scale
- `{shape.radius.sm}` `<v>`
- `{shape.radius.default}` `<v>`
- `{shape.radius.full}` `<v>`

### Elevation & Depth

| Layer | Treatment | Use |
|---|---|---|
| Flat | no shadow, no border | [when used] |
| Hairline | 1px `{colors.border}` | [when used] |
| Soft shadow | `{shape.shadow.sm}` | [when used] |
| Lifted shadow | `{shape.shadow.default}` | [when used] |

[Optional prose: layering philosophy — does this theme use shadows at all, or is it strictly flat?]

## §5 Variants
- **<variant1>** — [when to use]
- **<variant2>** — [when to use]
- **<variantN>** — [when to use]

**When to use**: [cover / body / closing / data — assign variants by purpose]

**Invariant across variants**: [tokens that don't change between variants, e.g. chrome bg]

## §6 Decorative signature

### <Name 1>
- What it is / geometry / usage / HTML template (if any)

### <Name N>
- ...

## §7 Do's and Don'ts

### Do's (positive guidance)
- [Use weight 600 for emphasis when italic is banned]
- [Use chrome runtime's 8 anchors for all metadata]
- [Use `{shape.space.X}` tokens for all gaps — never inline pixel values]
- [Use `{colors.muted-foreground}` for caption-tier text]

### Don'ts (red lines)
- **<taboo>** — [reason]

## §8 Append CSS

```css
/* 1. Chrome 8-anchor positions (if §6 chrome ring should be visible) */
.chrome-top-left    { /* top / left / font / color */ }
.chrome-top-right   { /* top / right / font / color */ }
.chrome-rule        { /* border-top / margin / height */ }
.chrome-bottom-left { /* bottom / left / font / color */ }
.chrome-bottom-right{ /* bottom / right / font / color */ }

/* 2. .page-shell padding / overflow */
.page-shell { /* position / padding / overflow */ }

/* 3. .page-content container + grid mode (if §4 has a Grid) */
.page-content { /* display: grid; columns/rows/gap */ }
.page-content[data-layout="flow"] { /* flow override */ }

/* 4. Utility classes (optional — only if using strict grid) */
/* .h1-9 { grid-column: 1 / 9; }
   .v3-8 { grid-row: 3 / 9; } */

/* 5. Component-level overrides (if §6 Decorative declares them) */

/* 6. Decorative motif inline CSS (if §6 uses motifs) */
```

## Known Gaps

[Declare what this design.md does NOT cover. Examples:]
- **Data-table / comparison-card components**: not specified — slide authors write ad-hoc HTML
- **Animation / transition tokens**: not specified — the deck is static
- **Dark-mode counterparts for all variants**: only `<variantX>` has the full dark token set
- **Print-specific theme overrides**: only the chrome runtime handles `@media print`; theme-level color/contrast not defined
- **Photographic atmosphere**: encoded in imagery, not as a CSS gradient — no token covers it
