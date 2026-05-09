---
name: design
role: framework-skeleton
description: Theme design contract — teaches the AI how to author a new design.md (one per deck)
---

# Design — Theme Design Contract

> One of the framework triplet: **runtime / design / content**. This document is not a specific theme — it is the standard for "how to author a theme."
>
> Every new deck drops one `design.md` into its own project folder, **strictly follows §1 – §7 of the schema below**, and starts with a `> Source: ...` line declaring the reference (image / URL / brand spec) the theme was built from.
>
> CSS is not hand-written inside `design.md`. When assembling the deck HTML, §8 substitutes the design.md values into `framework/design-tokens-template.css` and the result is inlined under the `§ TOKENS` block of `framework/deck.html`.

## What this document does

The design layer is the **theme primitives layer** in the 4-layer architecture. The visual character of a deck — what color, what typography, what shape, how the decorative system is organized — is settled once and for all by `design.md`.

Every deck's `design.md` (next to `content.md` and `<deck>.html`) contains:
- A short **Atmosphere** essay (so the AI can intuitively know the direction when authoring a new slide)
- **Value tables** (palette / type / shape — pure numbers and strings, no CSS)
- **Decorative signature** (rules describing theme-specific visual systems)
- **Do's and Don'ts** (positive + negative theme rules)
- **Known Gaps** (what this design.md does NOT cover)

## Files in this folder

`design.md` works alongside two adjacent files. They are **3 stages of one workflow**, never read in parallel:

| File | Read when | How |
|---|---|---|
| **`design.md`** (this file) | Learning the schema for authoring a new theme | Read once to understand §1 – §7 |
| **`design-blank-template.md`** | Starting a new theme | `cp framework/design-blank-template.md <my-deck>/design.md`, then fill section by section |
| **`design-tokens-template.css`** | Assembling deck HTML | Substitute the deck's design.md values into `{{...}}` placeholders, inline the rendered CSS as a `<style>` block under the `§ TOKENS` block of `framework/deck.html` |

The split exists so each file has a single content type (prose teaching / cp-ready scaffold / machine-readable CSS template). For an existing theme, you only ever touch the deck's own `design.md` — the framework triplet is reference.

## How to use

**Authoring a new theme**:
1. Read §1 – §7 below
2. Read the "Atmosphere writing guide" in §1
3. `cp framework/design-blank-template.md <my-deck>/design.md`
4. Fill section by section
5. Assemble the deck HTML: `cp framework/deck.html <my-deck>/<my-deck>.html`, then run §8 to substitute values into `framework/design-tokens-template.css` → replace the `§ TOKENS` block of `deck.html`

**Authoring a new slide**: read this deck's `design.md` §1 Atmosphere first — that decides what the page "looks like" — then pull values from §2 – §6.

## Token reference syntax

In prose throughout `design.md` (and other docs), reference tokens by **dotted namespace**:

| Namespace | Maps to | Example |
|---|---|---|
| `{colors.X}` | palette token `--X` | `{colors.primary}` → `var(--primary)` |
| `{type.size.X}` | size token `--text-X` | `{type.size.4xl}` → `var(--text-4xl)` |
| `{type.family.X}` | family token `--font-X` | `{type.family.body}` → `var(--font-body)` |
| `{shape.radius.X}` | radius token `--radius-X` | `{shape.radius.full}` → `var(--radius-full)` |
| `{shape.space.N}` | spacing token `--space-N` | `{shape.space.4}` → `var(--space-4)` |
| `{chrome.X}` | chrome runtime token `--chrome-X` | `{chrome.border}` → `var(--chrome-border)` |

**Why** `{...}` instead of `var(--...)` in prose: token paths survive renames via grep; `var(--accent)` decisions live in CSS muscle memory and don't surface in design intent. Prose using token paths makes the design searchable.

In the **§8 CSS template** (`framework/design-tokens-template.css`), the substitution syntax is **double-brace `{{...}}`** (Handlebars-style) — that is the generator's substitution marker. Don't confuse the two: `{}` for prose; `{{}}` for code template substitution.

---

## §1 Atmosphere

3-4 paragraphs of prose so the reader **feels** the mood of the theme — not just **reads** the parameters. Close with 5-10 bullets of Key Characteristics as a quick reference.

### The four required facets

| Paragraph | What to talk about |
|---|---|
| 1 | **Overall character** — imagery, metaphor, mood, period feel. Place the reader inside the design's "mood" |
| 2 | **Voice of typography** — what font family was picked and why; how the type works (weight / tracking / letterform width) |
| 3 | **Color logic** — monochrome / two-color / multi-color? Restrained or bold accent? Distribution of light and dark |
| 4 | **Decorative style** (optional) — decorative elements, spatial logic, special techniques (grid / shadow / glass / background grid pattern) |

End with **Key Characteristics**: 5-10 bullets distilling the prose's key points, **a quick reference for downstream AI**.

### Anti-patterns

❌ **Sprawl** — each paragraph wanders. Each paragraph stays on one topic; no cross-mixing.

❌ **Empty prose** — adjectives without anchors. Not "slightly blue", but **"barely-blue (`#08090a`), the blue is almost imperceptible"**.

❌ **Bullets-as-prose** — bullets ARE the closing summary, not the opener. Sentences first to convey the feel, then bullets to summarize.

❌ **Universal flattery** — "modern / clean / professional / elegant" works for any theme. Zero information.

### Example (excerpt from linear-app)

> Linear's website is a masterclass in dark-mode-first product design — a near-black canvas (`#08090a`) where content emerges from darkness like starlight. The overall impression is one of extreme precision engineering: every element exists in a carefully calibrated hierarchy of luminance, from barely-visible borders (`rgba(255,255,255,0.05)`) to soft, luminous text (`#f7f8f8`).
>
> The typography system is built entirely on Inter Variable with OpenType features `"cv01"` and `"ss03"` enabled globally, giving the typeface a cleaner, more geometric character. Inter is used at a remarkable range of weights — from 300 (light body) through 510 (medium, Linear's signature weight) to 590 (semibold emphasis). The 510 weight is particularly distinctive: it sits between regular and medium, creating a subtle emphasis that doesn't shout.

Why this passage works: **imagery ("starlight") + concrete values (`#08090a`) + design intent ("precision engineering") + implementation detail ("OpenType cv01")** all in the prose. The reader feels the character *and* gets exact numbers.

---

## §2 Palette values

List tokens in role-grouped sections. Every color gets a **descriptive nickname** alongside its token, with all variant hex values listed.

### Visual-color → token mapping SOP (required reading when the user provides a reference image / description)

The visual colors in a reference image cannot be mapped 1:1 to `{colors.background}` — **first decide what role each color plays in the visual**, then map it to the corresponding token. Common mistake: writing the "main color" into `{colors.background}` floods every slide with the main color (it should be `{colors.canvas-bg}` outside the chrome, with a neutral page background inside).

Use this decision tree:

| What you see in the reference image | What role it plays | Maps to token |
|---|---|---|
| **Largest area, outer frame, canvas base** | Visual environment / wrapper | `{colors.canvas-bg}` (chrome runtime, outer viewport) |
| **Each slide's base color** (where chrome's 4-corner text sits) | Page base | `{colors.background}` (page bg) |
| **Card / panel / info-block backgrounds** | Card layer | `{colors.card}` (in-slide cards) |
| **Secondary panel / inset region** | Secondary surface | `{colors.secondary}` (rarely used) |
| **Body copy / hero display text** | Primary text | `{colors.foreground}` |
| **Caption / secondary text / annotation** | Muted text | `{colors.muted-foreground}` |
| **Brand main color / logo / primary CTA bg** | Brand primary | `{colors.primary}` |
| **Small bright spots: pill / chip / accent dot / emphasis stroke** | Emphasis accent | `{colors.accent}` |
| **Error notice / warning color (≠ accent)** | Status alert | `{colors.destructive}` |
| **Divider / hairline / card outline** | Visual separation | `{colors.border}` |

**Hard rule**: the "main color" of a reference image is determined by **area** — the largest area is usually `{colors.canvas-bg}`, NOT `{colors.background}`. Only extremely minimal themes that genuinely flood the entire slide with the main color (e.g. archive's dark variant) should set `{colors.background}` to the main color.

### Example: CHASSAN reference image

Olive drab outer frame + cream slide base + lime emphasis dot.

```
What you see                            Wrong (V2 sub-agent did this)        Correct
──────────────────────────────────────────────────────────────────────────────────────
Olive drab (largest outer area)         {colors.background}: #7a8a2e ❌      {colors.canvas-bg}: #7a8a2e ✓
                                        (green floods the cream)              (green sits as outer chrome viewport)
Cream (slide base / chrome host)        {colors.card}: #efe7c8 ❌             {colors.background}: #efe7c8 ✓
Dark card (small label / pull box)      {colors.card}: #1a1f12 ✓              {colors.card}: #1a1f12 ✓
Bright lime (dot / pill / emphasis)     {colors.accent}: #cfe14a ✓            {colors.accent}: #cfe14a ✓
```

### The five required sections (with descriptive nickname pattern)

Format every entry as `**Nickname** (`{token}` — `<hex>`): use note`. List each variant's hex on an indented line.

```markdown
### Surface
- **Cream** (`{colors.background}` — `#efe7c8`): page canvas (slide base color)
  - lite:   `#efe7c8`
  - dark:   `#0a0a0a`
- **Olive Drab** (`{colors.canvas-bg}` — `#7a8a2e`): outer chrome viewport
- **Pearl** (`{colors.card}` — `#fafafc`): content container

### Text & Content
- **Ink** (`{colors.foreground}` — `#1d1d1f`): primary text
- **Stone** (`{colors.muted-foreground}` — `#7a7a7a`): caption / secondary text

### Brand & Accent
- **Action Blue** (`{colors.primary}` — `#0066cc`): brand identity
- **Lime** (`{colors.accent}` — `#cfe14a`): emphasis dot / pill / accent stroke

### Status
- **Alert** (`{colors.destructive}` — `#d33`): warning / error

### Hairlines & Borders
- **Hairline** (`{colors.border}` — `#e0e0e0`): visual separation
```

The 5 sections are **Surface / Text & Content / Brand & Accent / Status / Hairlines & Borders**. Token names align with shadcn (15 standard names locked):

```
--background  --foreground       --card      --card-foreground
--primary     --primary-foreground   --secondary --secondary-foreground
--muted       --muted-foreground   --accent    --accent-foreground
--destructive --destructive-foreground
--border
```

After all 15 tokens × N variants are filled, **the chrome runtime's 7 tokens go at the end of §2**:

```markdown
### Chrome Runtime (invariant across variants)
- `{colors.canvas-bg}` — `<hex>` (outer canvas, outside the sidebar)
- `{colors.chrome-bg}` — `<hex>` (sidebar / topbar base)
- `{colors.chrome-fg}` — `var(--foreground)` (points back to palette)
- `{colors.chrome-muted-fg}` — `var(--muted-foreground)`
- `{colors.chrome-border}` — `var(--border)`
- `{colors.chrome-accent}` — `var(--accent)`
- `{colors.chrome-accent-fg}` — `var(--accent-foreground)`
```

### Required to follow

- All 15 tokens × all variants must be listed; missing one means the §8 generator loses a row of CSS
- CSS functions (`rgba` / `color-mix`) are valid values
- Notes (parenthetical explanations) go after the value, not before

---

## §3 Typography values

### Font Family

```markdown
- **Primary** (display + body usually share one family): `Cabinet Grotesk`, `Geist`, `Inter`, `-apple-system`, `BlinkMacSystemFont`, `"Segoe UI"`, `"PingFang SC"`, `"Hiragino Sans GB"`, `"Microsoft YaHei"`, sans-serif
- **Mono** (code / technical labels): `ui-monospace`, `"JetBrains Mono"`, `"SF Mono"`, Menlo, monospace
- **OpenType features** (optional): `"cv01", "ss03"` enabled globally — alternate lowercase a + cleaner letterforms
```

The CJK fallback chain **must** include at least one of `"PingFang SC"` / `"Hiragino Sans GB"` / `"Microsoft YaHei"`.

### Type Scale

A unified ladder with 8 steps. Each row gives a **role name** (used in prose), a **token** (the technical handle), and 5 number columns. The values below are typical for a 1920×1080 slide canvas — each design.md may reset specific rows, but the ladder shape (8 sizes, 4xl largest) is fixed.

| Token | Role Name | Size | Weight | Leading | Tracking | Use Case |
|---|---|---|---|---|---|---|
| `{type.size.4xl}` | **Hero Display** | 120px | 700 | 1.05 | -0.02em | hero / cover only |
| `{type.size.3xl}` | **Display** | 88px | 600 | 1.05 | -0.02em | section opener |
| `{type.size.2xl}` | **Display Small** | 76px | 600 | 1.10 | -0.02em | medium display |
| `{type.size.xl}` | **H2 / Display LG** | 44px | 600 | 1.15 | -0.015em | h2 heads |
| `{type.size.lg}` | **H3 / Card Title** | 32px | 600 | 1.15 | -0.015em | h3 / card title |
| `{type.size.md}` | **Body** | 22px | 400 | 1.55 | 0 | body paragraph |
| `{type.size.sm}` | **Body Small / Caption** | 18px | 400 | 1.4 | 0 | body small |
| `{type.size.xs}` | **Fine Print / Label** | 16px | 500 | 1.4 | 0.05em | label / micro UI |

In prose throughout the deck and other docs: **reference role names** (`use Body for paragraph copy; Hero Display for cover`). Role names are how humans / AIs talk about hierarchy; the token is just the technical handle.

Each step **must include a use case** in your design.md — tells downstream AI which step belongs on which page.

### Principles — write 3-5

Talk about **how the scale is used**, not what the values are. Examples:

- **Compress at scale** — the larger the type, the tighter the tracking (`-0.02em` @ 120px, 0 @ 22px). Display uses tight letter-spacing to amplify the "engineering" feel
- **Single weight system** — most UI uses medium (500); emphasis uses semibold (600). Bold (700) is hero-only, to avoid "weight noise"
- **Leading inversely follows size** — display 1.05 (tight) → body 1.55 (open). Large type is already emphatic, so line-height tightens; small type needs air

### Note on Font Substitutes

Proprietary or non-system primary families need fallback advice for environments without the license / custom font. State which open-source family approximates yours and what tweaks recreate the feel.

Examples:

- **Cabinet Grotesk → Inter**: Inter at weight 600 with `font-feature-settings: "ss03"` approximates Cabinet Grotesk's geometric character. Tighten tracking by `-0.01em` on display sizes (Cabinet runs slightly tighter than Inter's default).
- **SF Pro → Inter**: Use `system-ui, -apple-system, BlinkMacSystemFont` first — on macOS / iOS this resolves to real SF Pro. For non-Apple platforms fall through to Inter; nudge `letter-spacing` down by `-0.01em` on display sizes; tighten body leading from `1.47` to `1.44`.
- **Geist → Inter**: nearly identical metrics; no tracking adjustment needed. Drop weight 510 if Inter Variable isn't loaded — fall back to weight 500.

If the primary IS already system / open-source (`system-ui`, Inter, etc.), this section can simply state "no substitution needed."

---

## §4 Shape values

### Spacing System

A 4px base scale. Used for inter-element gaps, padding, and margins.

```
{shape.space.1}    4px
{shape.space.2}    8px
{shape.space.3}   12px
{shape.space.4}   16px
{shape.space.6}   24px
{shape.space.8}   32px
{shape.space.12}  48px
{shape.space.16}  64px
{shape.space.24}  96px
{shape.space.32} 128px
```

Section gutters typically multiples of 24px or 48px. For the 1920×1080 canvas, edge margins land at 96px (`{shape.space.24}`).

### Grid & Container

If the theme uses a structural grid (some don't — flow-only themes can skip), declare:

```markdown
- columns: 12
- rows: 8
- margin x: 96px
- margin top: 96px
- margin bottom: 96px
- gutter: 24px
- row gap: 24px

**Track names** (used by `page-content data-layout="strict"`):
- `{shape.grid.track-eyebrow}`: 1 / 4
- `{shape.grid.track-title}`:   1 / 9
- `{shape.grid.track-body}`:    1 / 9
- `{shape.grid.track-hero}`:    1 / 9
- `{shape.grid.track-stat}`:    7 / -1
- `{shape.grid.track-aside}`:   9 / -1
```

### Border Radius Scale

```
{shape.radius.sm}      16px   tag / pill / small element
{shape.radius.default} 20px   default (card / panel)
{shape.radius.full}   999px   full pill
```

### Elevation & Depth

| Layer | Treatment | Use |
|---|---|---|
| **Flat** | no shadow, no border | Default for most slide content |
| **Hairline** | 1px `{colors.border}` | Card outlines, dividers |
| **Soft shadow** | `{shape.shadow.sm}` | Subtle lift if needed |
| **Lifted shadow** | `{shape.shadow.default}` | Stronger card / hero lift |

Most slide themes use **Flat or Hairline only**. Shadows are optional — themes that ban shadows should set both shadow tokens to `none`:

```
{shape.shadow.sm}      none      (theme bans shadows)
{shape.shadow.default} none

  or

{shape.shadow.sm}      0 1px 2px rgba(0, 0, 0, 0.06)
{shape.shadow.default} 0 4px 12px rgba(0, 0, 0, 0.10)
```

---

## §5 Variants

### Required

1. **Variant list** — which ones, and each one's semantic role (one sentence)
2. **When to use** — which variant for cover / body / closing / data pages
3. **Tokens that stay invariant across variants** — e.g. a fixed light-color chrome (even when slides go dark)

### Example

```markdown
- **lite** (default) — cream-white base, monochrome body; default for body pages
- **dark**           — true black base; cover / key data / closing for emphasis
- **forest**         — deep-green base; problem / market themed pages

**When to use**:
- cover P1: dark (strong impact)
- most body slides: lite (neutral default)
- problem P2: dark (emotional color)
- market sub-page P8: dark + bg-mode="market" (gradient overlay)
- closing P9: lite (return to calm)

**Invariant across variants**: chrome runtime is always light-chrome (`{chrome.bg}` = `#f0f0f0`) —
even when slides go dark, the sidebar stays light. Reason: editor-style consistency.
```

### Multi-variant design principles

- **Every palette token must be filled for every variant** — `[data-variant="dark"] { ... }` overrides all 15 tokens, not just `{colors.background}`
- **Typography / shape / grid stay invariant** — only the palette swaps; type sizes / radii / spacing / grid never change
- **Variants are not color themes** — they are **mood roles**. Each variant has its own use case, not just a "day / night" split

---

## §6 Decorative signature

Theme-specific visual language — **chrome ring / decorative layers / emphasis system / background grid pattern** etc. Every entry **gives rules, not screenshots**.

### Each entry must cover four facets

1. **What it is** — name + category (chrome ring / overlay / accent system / etc)
2. **Geometry parameters** — position, size, spacing, color
3. **Usage** — when to use, when not to use, how it shifts across variants
4. **HTML template** (optional) — if the spec dictates HTML structure rather than the chrome runtime

### Example

```markdown
### Chrome Ring (8 anchors)

- Every page has it, **not optional** — even covers can't skip it
- Anchor positions (chrome runtime hard constraint):
  - `chrome-top-left` — logo / brand mark
  - `chrome-top-right` — copyright / current page number
  - `chrome-bottom-left` — footer / brand secondary
  - `chrome-bottom-right` — page number / brand letter
  - `chrome-rule` — hairline `<hr>` at 96px from the top
- Visual: 1px hairline, color `{colors.chrome-border}`, auto-follows variants

### M18 Ambient Frames

- Rounded-rect SVG, background decoration
- Used only on cover and closing pages
- Size: 1208×920 + 1572×120 stacked
- Color: `{colors.foreground}` at 8% opacity — blends in without stealing focus
- HTML template: `<div class="ambient-frames" data-variant="...">…</div>`

### M20 Pill Highlight

- Emphasis system: pitch refuses `{colors.accent}` for tinting; emphasis lives on a pill background
- Geometry: `padding: 4px 12px`, `border-radius: {shape.radius.full}`
- Usage: numbers / keywords get pill-wrapped; titles do not
- Across variants: `background: {colors.secondary}`, auto-adapts
```

---

## §7 Do's and Don'ts

Theme-specific rules — **positive guidance + taboos**. Spell out the **reason** for each.

### Do's (positive guidance)

- **Use weight 600 for emphasis** — instead of italic (often banned), upgrade weight or pill-wrap the keyword
- **Use the chrome runtime's 8 anchors for all metadata** — page number / brand / copyright go in chrome, not in slide content
- **Use `{shape.space.X}` tokens for all gaps** — never inline pixel values like `padding: 18px`
- **Use `{colors.muted-foreground}` for any caption-tier text** — keep the foreground / muted-foreground rhythm consistent
- **Use a hairline border for "card hover" lift** — when the theme bans shadow, a 1px `{colors.border}` provides visual separation without breaking flatness

### Don'ts (red lines)

- **No italic** — Cabinet Grotesk has no italic glyphs, so a system fallback would break typeface consistency
- **No chromatic accent** — `{colors.accent}` is locked to `{colors.foreground}`; emphasis comes from pill highlight, not color
- **No shadow** — `{shape.shadow.default}: none`, flat aesthetic; if you want lift, use a hairline border
- **No rounded > 24px** — radius beyond 24px enters "soft" territory, but pitch follows grotesk principles — hardness is the aesthetic
- **No inline pixel values** — every spacing, size, color must reference a token; no `padding: 23px` or `color: #1d1d1f`

**Why pair Do's with Don'ts**: a future AI tempted to "add some italic emphasis" gets blocked by the Don't, and the matching Do tells it what to do instead. Removes the "I can't do X, so I do nothing" trap.

---

## §8 CSS generation procedure

### Source

The token block template lives at **`framework/design-tokens-template.css`** — a single canonical CSS template with `{{...}}` placeholders. design.md does not duplicate it.

When assembling `<deck>.html`:
1. Read this design.md's §2 – §5 values
2. Substitute every `{{...}}` placeholder in `framework/design-tokens-template.css` per the rules below
3. Inline the rendered CSS into the deck HTML, replacing the `§ TOKENS` block of `framework/deck.html`
4. Append the design.md's §8 Append CSS block (see below) verbatim below the rendered token block

### Substitution rules

- `{{palette.X.Y}}` → §2 token X in variant Y
- `{{type.size.X}}` → §3 size scale step X
- `{{type.family.X}}` → §3 family X (full fallback chain)
- `{{shape.radius.X}}` → §4 radius X
- `{{shape.space.N}}` → §4 space N
- `{{shape.grid.X}}` → §4 grid X
- `{{shape.shadow.X}}` → §4 shadow X
- `{{chrome.X}}` → §2 Chrome Runtime X
- `{{#each variants}}...{{/each}}` → repeat the block once per variant

### Append CSS (every design.md hand-writes this)

The token block is templated. Below it, every design.md adds **theme-specific layout primitives** by hand. Write all of it at the end of `design.md` inside **one** ` ```css ` fenced block; the assembler pastes it **verbatim** below the rendered token block.

Write in this order (6 fixed slots):

1. **Chrome 8-anchor position CSS** (if §6 chrome ring should be visible) — `chrome-top-left / -top-right / -bottom-left / -bottom-right` (four lines) + `chrome-rule` (one line). `framework/deck.html` already gives `chrome-rule` a `border: 0; margin: 0; height: 0` fallback; if you want the line drawn, override here
2. **`page-shell` padding / overflow** — `framework/deck.html` provides 1920×1080 + transform; add `padding / position / overflow` here
3. **`page-content` container + grid mode** (if §4 has a Grid) — 12×N strict grid + flow mode dual-track (see pitch sample); flexbox-flow themes can skip
4. **Utility classes** (optional) — e.g. `.h1-9` / `.v3-8` grid-column / row utilities (only if strict grid)
5. **Component-level overrides** (if §6 declares them) — theme-level component bends, e.g. `[data-variant="dark"] .pitch-card { background: #0d2a25; }`
6. **Decorative motif inline CSS** (if §6 uses motifs like M18 / M20) — e.g. `.ambient-frame` SVG container, `.pill-highlight` background

**Hard rule**: only this block contains raw CSS in `design.md`. The other sections (§1 – §7) hold values + prose only.

---

## Known Gaps

Each design.md should declare what it does **NOT** cover. This keeps the AI from inventing or assuming when authoring slides.

Categories to consider:
- Components or block patterns not specified (data-table, comparison-cards, etc.)
- Animation / transition tokens (durations, easings)
- Dark-mode counterparts for non-default variants
- Print-specific overrides beyond the chrome runtime's `@media print`
- Fonts-loaded vs. fallback divergence
- Reference-image atmospheres that couldn't be turned into tokens (photographic mood, etc.)

Example for a new theme:

```markdown
- **Data-table component shape**: not specified — slide authors write ad-hoc HTML
- **Animation tokens**: not specified — the deck is static
- **Mobile / portrait orientation**: not supported (1920×1080 only)
- **Print-specific theme overrides**: only the chrome runtime handles `@media print`; theme-level color/contrast not defined
- **Photographic atmosphere on cover**: encoded in the imagery itself, not a CSS gradient — no token covers it
```

---

## Constraints (do not violate)

- **`design.md` has only one place for CSS** — the §8 Append CSS block
- **Token naming aligns with shadcn** (15 standard names locked)
- **Every Atmosphere paragraph must be anchored to concrete values** (no floating prose)
- **Palette must list the full variant × token matrix** — missing one row loses one row of CSS
- **§8 Append CSS has 6 fixed slots in fixed order**

## Why

- **Prose + tables** lets the AI both intuitively imitate the theme (read §1) and pull values precisely (read §2 – §4)
- **CSS lives outside design.md** to prevent "value updated but CSS forgot" drift; §8 is the single transformation path
- **Atmosphere requires 4 paragraphs** so the AI can't half-ass it — the felt sense must be drawn out in full
- **Token reference syntax `{...}`** makes design intent searchable across docs (rename via grep)
- **Do's alongside Don'ts** removes the "I can't do X, so I do nothing" trap
- **Known Gaps** prevents AI from inventing what isn't documented
- **Descriptive nicknames** (`Olive Drab` / `Action Blue`) give humans + AIs a vocabulary; bare `{colors.canvas-bg}` doesn't survive 5 themes
