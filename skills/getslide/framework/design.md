---
name: design
role: framework-skeleton
description: Theme design contract — teaches the AI how to author a new design.md (one per deck)
---

# Design — Theme Design Contract

> One of the framework triplet: **runtime / design / content**. This document is not a specific theme — it is the standard for "how to author a theme."
>
> Every new deck drops one `design.md` into its own project folder (it does **not** have to live under `samples/`; the AI puts the folder wherever the user picks), and **strictly follows §1 – §7 of the schema below**. CSS is **not** hand-written inside `design.md` — when assembling the deck HTML, §8 below auto-generates the CSS and substitutes it into the `§ TOKENS` block of `framework/deck.html`.

## What this document does

The design layer is the **theme primitives layer** in the 4-layer architecture. The visual character of a deck — what color, what typeface, what shape, how the decorative system is organized — is settled once and for all by `design.md`.

Every deck's `design.md` (sitting next to `content.md` and `<deck>.html` in the same folder) contains:
- A short **Atmosphere** essay (so the AI can intuitively know which direction to take when authoring a new slide)
- **Value tables** (palette / type / shape, pure numbers and strings, no CSS)
- **Decorative signature** (rules describing the theme-specific visual systems)
- **Red lines** (theme-specific taboos)

## How to use

**Authoring a new theme**:
1. Read §1 – §7 below (the schema tells you what to fill into each section)
2. Read the "Atmosphere writing guide" at the bottom (learn how to write §1 well)
3. Copy the "Blank template" at the bottom into `<my-deck>/design.md` (under your new deck folder)
4. Fill section by section
5. Assemble the deck HTML: `cp framework/deck.html <my-deck>/<my-deck>.html`, then run §8 to substitute design.md values → replace the `§ TOKENS` block of `deck.html`

**Authoring a new slide**: read this deck's `design.md` §1 Atmosphere first — that decides what the page "looks like" — then pull values from §2 – §6.

---

## §1 Atmosphere

3-4 paragraphs of prose so the reader **feels** the mood of the theme — not just **reads** the parameters. Close with 5-10 bullets of Key Characteristics as a quick reference.

### The four required facets

| Paragraph | What to talk about |
|---|---|
| 1 | **Overall character** — imagery, metaphor, mood, period feel. Place the reader inside the design's "mood" |
| 2 | **Role of typography** — what font family was picked and why; how the type works (weight / tracking / letterform width) |
| 3 | **Color logic** — monochrome / two-color / multi-color? Is the accent restrained or bold? Distribution of light and dark |
| 4 | **Decorative style** (optional) — decorative elements, spatial logic, special techniques (grid / shadow / glass / background grid pattern) |

End with **Key Characteristics**: 5-10 bullets distilling the prose's key points, **a quick reference for downstream AI**.

### Three things to avoid when writing

- **Avoid sprawl** — each paragraph stays on one topic, no cross-mixing
- **Avoid emptiness** — must give concrete values. Not "slightly blue", but "barely-blue (`#08090a`), the blue is almost imperceptible"
- **Avoid bullets-as-prose** — bullets are a closing, not an opener. Use sentences first to convey the feel, then summarize with bullets

### Example (excerpt from linear-app)

> Linear's website is a masterclass in dark-mode-first product design — a near-black canvas (`#08090a`) where content emerges from darkness like starlight. The overall impression is one of extreme precision engineering: every element exists in a carefully calibrated hierarchy of luminance, from barely-visible borders (`rgba(255,255,255,0.05)`) to soft, luminous text (`#f7f8f8`).
>
> The typography system is built entirely on Inter Variable with OpenType features `"cv01"` and `"ss03"` enabled globally, giving the typeface a cleaner, more geometric character. Inter is used at a remarkable range of weights — from 300 (light body) through 510 (medium, Linear's signature weight) to 590 (semibold emphasis). The 510 weight is particularly distinctive: it sits between regular and medium, creating a subtle emphasis that doesn't shout.

Why this passage works: **imagery ("starlight") + concrete values (`#08090a`) + design intent ("precision engineering") + implementation detail ("OpenType cv01")** are all in the prose. The reader feels the character *and* gets exact numbers.

### Example (excerpt from apple)

> Apple's website embodies a refined, premium aesthetic that prioritizes content over chrome — a near-pure-white canvas (`rgb(251, 251, 253)`) with deep black text creating maximum contrast and readability. The design philosophy is one of "design is how it works" — invisible craftsmanship where every element serves the content.

Why this passage works: a single slogan ("design is how it works") becomes the spine, and the whole philosophy wraps around it. **No need to pile up adjectives** — picking the right anchor phrase beats stacking ten of them.

### Anti-patterns (don't write like this)

❌ **Bullets-only**:
> - Dark mode native
> - Inter font
> - Indigo accent

The reader has no idea why, when, or how dark.

❌ **Abstract prose**:
> The theme feels modern and clean, with a sense of professionalism and elegance.

"Modern / clean / professional / elegant" is empty — any theme could claim it. **Zero information.**

---

## §2 Palette values

List tokens in role-grouped sections. One token per line, with each variant's hex plus a one-line use note.

### Visual-color → token mapping SOP (required reading when the user provides a reference image / description)

The visual colors in the reference image cannot be mapped 1:1 to `--background` — **first decide what role each color plays in the visual**, then map it to the corresponding token. Common mistake: writing the "main color" into `--background`, which floods every slide with the main color (it should be `--canvas-bg` outside the chrome, with a neutral page background inside).

Use this decision tree:

| What you see in the reference image | What role it plays | Maps to token |
|---|---|---|
| **Largest area, outer frame, canvas base** | Visual environment / wrapper | `--canvas-bg` (chrome runtime token, outer viewport) |
| **Each slide's base color** (the surface where the chrome's 4-corner text sits) | Page base | `--background` (page bg) |
| **Card / panel / info-block backgrounds within a slide** | Card layer | `--card` (in-slide cards) |
| **Secondary panel / inset-region background** | Secondary surface | `--secondary` (rarely used) |
| **Body copy / hero display text** | Primary text | `--foreground` |
| **Caption / secondary text / annotation** | Muted text | `--muted-foreground` |
| **Brand main color / logo color / primary CTA bg** | Brand primary | `--primary` |
| **Small bright spots: pill / chip / accent dot / emphasis stroke** | Emphasis accent | `--accent` |
| **Error notice / warning color (≠ accent)** | Status alert | `--destructive` |
| **Divider / hairline / card outline** | Visual separation | `--border` |

### Example: the correct mapping for the CHASSAN reference image (olive green + cream + lime dot)

```
What you see                              Wrong (V2 sub-agent did this)        Correct
────────────────────────────────────────────────────────────────────────────────────────
Olive drab (largest outer area)         --background: #7a8a2e ❌              --canvas-bg: #7a8a2e ✓
                                          (result: green floods the cream)     (green sits as outer chrome viewport)
Cream (slide base / where chrome sits)    --card: #efe7c8 ❌                    --background: #efe7c8 ✓
Dark card (small label / pull box)        --card: #1a1f12 ✓                     --card: #1a1f12 ✓
Bright lime (dot / pill / emphasis blob)  --accent: #cfe14a ✓                   --accent: #cfe14a ✓
```

**Hard rule**: the "main color" of a reference image is determined by **area** — **the largest area is usually `--canvas-bg`, not `--background`**. Only extremely minimal themes that genuinely flood the entire slide with the main color (e.g. archive's dark variant) should set `--background` to the main color.

### The five required sections

1. **Background Surfaces** — page canvas / panel / card / elevated / overlay
2. **Text & Content** — primary / secondary (muted-foreground)
3. **Brand & Accent** — primary / accent / accent-foreground
4. **Status** — destructive / success / warning (only if used)
5. **Border** — default border / muted border

### Required format

Token naming **aligns with shadcn** — the 15 standard token names are locked:

```
--background  --foreground
--card        --card-foreground
--primary     --primary-foreground
--secondary   --secondary-foreground
--muted       --muted-foreground
--accent      --accent-foreground
--destructive --destructive-foreground
--border
```

Every token must list **the value for every variant**. Example:

```markdown
### Background Surfaces

- `--background` — page canvas (slide base color)
  - lite:   `#f0f0f0`
  - dark:   `#0a0a0a`
  - forest: `#0d2a25`
- `--card` — content container
  - lite:   `#f0f0f0`              (transparent over background)
  - dark:   `rgba(0, 0, 0, 0.30)`  (semi-transparent black, layers another shade onto a dark base)
  - forest: `rgba(0, 0, 0, 0.24)`
```

After all 5 sections × 15 tokens × N variants are filled in, **the chrome runtime's 7 default-value tokens go here too**:

```markdown
### Chrome Runtime (invariant across variants)

- `--canvas-bg`        — `#dcdcdc`            (outer canvas, outside the sidebar)
- `--chrome-bg`        — `#f0f0f0`            (sidebar / topbar base)
- `--chrome-fg`        — `var(--foreground)`  (points back to palette)
- `--chrome-muted-fg`  — `var(--muted-foreground)`
- `--chrome-border`    — `var(--border)`
- `--chrome-accent`    — `var(--accent)`
- `--chrome-accent-fg` — `var(--accent-foreground)`
```

### Required to follow

- All 15 tokens × all variants must be listed; if one is missing, the §8 generator loses a row of CSS
- CSS functions (`rgba` / `color-mix`) are valid values
- Notes (parenthetical explanations) go after the value

---

## §3 Typography values

### Required

#### Font Family

```markdown
- **Primary** (display + body usually share one family): `Cabinet Grotesk`, `Geist`, `Inter`, `-apple-system`, `BlinkMacSystemFont`, `"Segoe UI"`, `"PingFang SC"`, `"Hiragino Sans GB"`, `"Microsoft YaHei"`, sans-serif
- **Mono** (code / technical labels): `ui-monospace`, `"JetBrains Mono"`, `"SF Mono"`, Menlo, monospace
- **OpenType features** (optional): `"cv01", "ss03"` enabled globally — alternate lowercase a + cleaner letterforms
```

The CJK fallback chain **must** include at least one of `"PingFang SC"` / `"Hiragino Sans GB"` / `"Microsoft YaHei"`.

#### Size scale + use case

```markdown
- `--text-xs`    16px  — caption / label / micro UI
- `--text-sm`    18px  — body small
- `--text-md`    22px  — body
- `--text-lg`    32px  — h3 / card title
- `--text-xl`    44px  — h2
- `--text-2xl`   76px  — display small
- `--text-3xl`   88px  — display
- `--text-4xl`  120px  — hero / cover only
```

Each step **must include a use case** — tells downstream AI which step belongs on which page.

#### Weight scale

```markdown
- `--weight-normal`     400  — body default
- `--weight-medium`     500  — UI label / nav
- `--weight-semibold`   600  — title / strong emphasis
- `--weight-bold`       700  — display only
```

#### Leading + tracking scale

```markdown
**Leading** (line-height)
- `--leading-tight`   1.05  — display large type
- `--leading-snug`    1.15  — h2-h3
- `--leading-normal`  1.4   — UI text
- `--leading-body`    1.55  — body paragraph

**Tracking** (letter-spacing)
- `--tracking-tight`  -0.02em  — auto-tighten at large sizes
- `--tracking-snug`   -0.015em — slight tighten at mid sizes
- `--tracking-normal`  0       — default
- `--tracking-wide`    0.22em  — eyebrow / kicker
```

### Principles — write 3-5

Talk about **how this scale is used**, not what the values are. For example:

```markdown
- **compress at scale** — the larger the type, the tighter the tracking (-0.02em @ 120px, 0 @ 22px). Display uses tight letter-spacing to amplify the "engineering" feel
- **single weight system** — most UI uses medium (500); emphasis uses semibold (600). Bold (700) is hero-only, to avoid "weight noise"
- **leading inversely follows size** — display 1.05 (tight) → body 1.55 (open). Large type is already emphatic, so line-height tightens; small type needs air
```

---

## §4 Shape values (radius + spacing)

### Radius scale

```markdown
- `--radius-sm`     16px   — tag / pill / small element
- `--radius`        20px   — default (card / panel)
- `--radius-full`   999px  — full pill
```

### Space scale

4-base scale:

```markdown
- `--space-1`     4px
- `--space-2`     8px
- `--space-3`    12px
- `--space-4`    16px
- `--space-6`    24px
- `--space-8`    32px
- `--space-12`   48px
- `--space-16`   64px
- `--space-24`   96px
- `--space-32`  128px
```

### Grid (if the theme has a structural grid)

```markdown
- columns: 12
- rows: 8
- margin x: 96px
- margin top: 96px
- margin bottom: 96px
- gutter: 24px
- row gap: 24px

**Track names** (used by `page-content data-layout="strict"`):
- `--track-eyebrow`: 1 / 4
- `--track-title`:   1 / 9
- `--track-body`:    1 / 9
- `--track-hero`:    1 / 9
- `--track-stat`:    7 / -1
- `--track-aside`:   9 / -1
```

### Shadow

```markdown
- `--shadow-sm`: none           (theme bans shadows → set both to none)
- `--shadow`:    none
```

or

```markdown
- `--shadow-sm`: 0 1px 2px rgba(0, 0, 0, 0.06)
- `--shadow`:    0 4px 12px rgba(0, 0, 0, 0.10)
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

**Invariant across variants**: chrome runtime is always light-chrome (`--chrome-bg: #f0f0f0`) —
even when slides go dark, the sidebar stays light. Reason: editor-style consistency.
```

### Multi-variant design principles

- **Every palette token must be filled for every variant** — `[data-variant="dark"] { ... }` must override all 15 tokens, not just `--background`
- **Typography / shape / grid stay invariant across variants** — only the palette swaps; type sizes / radii / spacing / grid never change
- **Variants are not color themes** — they are **mood roles**. Each variant has its own "use case", not just a "day / night" split

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
- Visual: 1px hairline, color `var(--chrome-border)`, auto-follows variants

### M18 Ambient Frames

- Rounded-rect SVG, background decoration
- Used only on cover and closing pages
- Size: 1208×920 + 1572×120 stacked
- Color: `var(--foreground)` at 8% opacity — blends in without stealing focus
- HTML template: `<div class="ambient-frames" data-variant="...">…</div>`

### M20 Pill Highlight

- Emphasis system: pitch refuses `--accent` for tinting; emphasis lives on a pill background
- Geometry: `padding: 4px 12px`, `border-radius: --radius-full`
- Usage: numbers / keywords get pill-wrapped; titles do not
- Across variants: `background: var(--secondary)`, auto-adapts
```

---

## §7 Red Lines (taboos)

Theme-specific taboos — doing them breaks the character. Spell out the **reason** for each.

### Example

```markdown
- **No italic** — Cabinet Grotesk has no italic glyphs, so a system fallback would break typeface consistency
- **No chromatic accent** — `--accent` is locked to `--foreground`; emphasis comes from pill highlight, not color
- **No shadow** — `--shadow: none`, flat aesthetic; if the theme wants "card hover" lift, use a border, not a shadow
- **No rounded > 24px** — radius beyond 24px enters "soft" territory, but pitch follows grotesk principles — hardness is the aesthetic
```

Why write taboos: **a future AI tempted to "add some italic emphasis" gets blocked by §7** — main slide visuals stay consistent.

---

## §8 CSS generation procedure (provided by framework, not duplicated per design.md)

When assembling `sample.html`, plug the design.md §2 – §5 values into the template below to generate the inline `<style>` block of `:root` + variants.

### Template

```css
/* ============ Tokens (auto-generated from design.md) ============ */
:root {
  /* ─────────── tokens shared across variants ─────────── */
  --canvas-bg: {{chrome.canvas-bg}};
  --chrome-bg: {{chrome.chrome-bg}};

  /* ─────────── typography (scale) ─────────── */
  --font-display: {{type.family.display}};
  --font-body:    {{type.family.body}};
  --font-mono:    {{type.family.mono}};

  --text-xs:   {{type.size.xs}};
  --text-sm:   {{type.size.sm}};
  --text-md:   {{type.size.md}};
  --text-lg:   {{type.size.lg}};
  --text-xl:   {{type.size.xl}};
  --text-2xl:  {{type.size.2xl}};
  --text-3xl:  {{type.size.3xl}};
  --text-4xl:  {{type.size.4xl}};

  --weight-normal:    {{type.weight.normal}};
  --weight-medium:    {{type.weight.medium}};
  --weight-semibold:  {{type.weight.semibold}};
  --weight-bold:      {{type.weight.bold}};

  --leading-tight:   {{type.leading.tight}};
  --leading-snug:    {{type.leading.snug}};
  --leading-normal:  {{type.leading.normal}};
  --leading-body:    {{type.leading.body}};

  --tracking-tight:   {{type.tracking.tight}};
  --tracking-snug:    {{type.tracking.snug}};
  --tracking-normal:  {{type.tracking.normal}};
  --tracking-wide:    {{type.tracking.wide}};

  /* ─────────── shape (scale) ─────────── */
  --radius-sm:   {{shape.radius.sm}};
  --radius:      {{shape.radius.default}};
  --radius-full: {{shape.radius.full}};

  --space-1:    {{shape.space.1}};
  --space-2:    {{shape.space.2}};
  --space-3:    {{shape.space.3}};
  --space-4:    {{shape.space.4}};
  --space-6:    {{shape.space.6}};
  --space-8:    {{shape.space.8}};
  --space-12:   {{shape.space.12}};
  --space-16:   {{shape.space.16}};
  --space-24:   {{shape.space.24}};
  --space-32:   {{shape.space.32}};

  /* ─────────── grid (if the theme has one) ─────────── */
  --grid-columns:        {{shape.grid.columns}};
  --grid-rows:           {{shape.grid.rows}};
  --grid-gutter:         {{shape.grid.gutter}};
  --grid-row-gap:        {{shape.grid.row-gap}};
  --grid-margin-x:       {{shape.grid.margin-x}};
  --grid-margin-top:     {{shape.grid.margin-top}};
  --grid-margin-bottom:  {{shape.grid.margin-bottom}};
  /* + track-* as needed */

  /* ─────────── elevation ─────────── */
  --shadow-sm: {{shape.shadow.sm}};
  --shadow:    {{shape.shadow.default}};

  /* ─────────── default variant tokens (= the first variant's values) ─────────── */
  --background:             {{palette.background.<default-variant>}};
  --foreground:             {{palette.foreground.<default-variant>}};
  --muted:                  {{palette.muted.<default-variant>}};
  --muted-foreground:       {{palette.muted-foreground.<default-variant>}};
  --border:                 {{palette.border.<default-variant>}};
  --card:                   {{palette.card.<default-variant>}};
  --card-foreground:        {{palette.card-foreground.<default-variant>}};
  --primary:                {{palette.primary.<default-variant>}};
  --primary-foreground:     {{palette.primary-foreground.<default-variant>}};
  --secondary:              {{palette.secondary.<default-variant>}};
  --secondary-foreground:   {{palette.secondary-foreground.<default-variant>}};
  --accent:                 {{palette.accent.<default-variant>}};
  --accent-foreground:      {{palette.accent-foreground.<default-variant>}};
  --destructive:            {{palette.destructive.<default-variant>}};
  --destructive-foreground: {{palette.destructive-foreground.<default-variant>}};

  /* ─────────── chrome runtime ─────────── */
  --chrome-fg:        {{chrome.fg}};
  --chrome-muted-fg:  {{chrome.muted-fg}};
  --chrome-border:    {{chrome.border}};
  --chrome-accent:    {{chrome.accent}};
  --chrome-accent-fg: {{chrome.accent-fg}};
}

/* ─────────── explicit variant overrides ─────────── */
{{#each variants}}
[data-variant="{{name}}"] {
  --background:             {{palette.background.{{name}}}};
  --foreground:             {{palette.foreground.{{name}}}};
  --muted:                  {{palette.muted.{{name}}}};
  --muted-foreground:       {{palette.muted-foreground.{{name}}}};
  --border:                 {{palette.border.{{name}}}};
  --card:                   {{palette.card.{{name}}}};
  --card-foreground:        {{palette.card-foreground.{{name}}}};
  --primary:                {{palette.primary.{{name}}}};
  --primary-foreground:     {{palette.primary-foreground.{{name}}}};
  --secondary:              {{palette.secondary.{{name}}}};
  --secondary-foreground:   {{palette.secondary-foreground.{{name}}}};
  --accent:                 {{palette.accent.{{name}}}};
  --accent-foreground:      {{palette.accent-foreground.{{name}}}};
  --destructive:            {{palette.destructive.{{name}}}};
  --destructive-foreground: {{palette.destructive-foreground.{{name}}}};
}
{{/each}}

/* ─────────── global base ─────────── */
* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  font-family: var(--font-body);
  color: var(--foreground);
  background: var(--canvas-bg);
  -webkit-font-smoothing: antialiased;
}
```

### Substitution rules

- `{{palette.X.Y}}` → the value of token X in variant Y from §2
- `{{type.size.X}}` → the X step from §3 size scale
- `{{type.family.X}}` → the full family string from §3 (with the complete fallback chain)
- `{{shape.radius.X}}` → §4 radius X
- `{{shape.space.N}}` → §4 space N
- `{{shape.grid.X}}` → §4 grid X
- `{{shape.shadow.X}}` → §4 shadow X
- `{{chrome.X}}` → X from the §2 Chrome Runtime section
- `{{#each variants}}...{{/each}}` → repeat the block once per variant

### Append CSS (every design.md must append; written by hand, not templated)

The §8 template only generates tokens + a base reset. The layers below are **theme-level layout primitives** — every theme has its own positions / grid / decoration, and the framework can't abstract that for you. Write all of it at the end of `design.md` inside **one** ` ```css ` fenced block, and when assembling `sample.html` paste it **verbatim** below the generated `:root` block.

Write in this order:

1. **Chrome's 8-anchor position CSS** (if you want the chrome ring visible)
   `.chrome-top-left / -top-right / -bottom-left / -bottom-right` (four lines) + `.chrome-rule` (one line). Comes from the §6 Chrome Ring decision. `framework/deck.html` already gives `.chrome-rule` a fallback of `border: 0; margin: 0; height: 0`; if you want the line drawn, override it back here
2. **`.page-shell` padding / overflow**
   `framework/deck.html` already provides 1920×1080 + the centering transform; design.md only adds `padding / position / overflow` here
3. **`.page-content` container + grid mode** (if §4 has a Grid)
   12×N strict grid + flow mode dual-track (see pitch). Some themes use a single flexbox flow; in that case, skip
4. **Utility classes** (optional)
   e.g. `.h1-9` / `.v3-8` grid-column / row utilities — only needed when the theme uses a strict grid
5. **Component-level overrides** (if §6 Decorative signature declares them)
   Theme-level component bends, e.g. pitch's `[data-variant="dark"] .pitch-card { background: #0d2a25; }`
6. **Decorative motif inline CSS** (if §6 uses motifs like M18 / M20)
   e.g. `.ambient-frame` SVG container, `.pill-highlight` background, etc.

**Hard rule**: **only** this block contains raw CSS in `design.md`. The other sections (§1 – §7) cannot contain CSS — values are values, prose is prose.

---

## design.md blank template

For a new theme, copy this skeleton verbatim into `<my-deck>/design.md` and start filling (`<my-deck>` is the folder you picked for this deck):

````markdown
---
name: <slug>
description: "<one-sentence theme positioning>"
version: 0.1.0
---

# <Theme Name>

> [a one-line tagline, e.g. "YC / TechCrunch-style pitch deck theme"]

## §1 Atmosphere

[Paragraph 1: overall character — imagery, metaphor, mood, period feel]

[Paragraph 2: role of typography — font family, why, weight usage]

[Paragraph 3: color logic — monochrome / two-color / multi-color? Restrained or bold accent?]

[Paragraph 4 (optional): decorative style — decorative elements, spatial logic, special techniques]

**Key Characteristics**:
- [5-10 bullets]

## §2 Palette values

### Background Surfaces
- `--background` — page canvas
  - <variant1>: `<hex>`
  - <variant2>: `<hex>`
- ...

### Text & Content
- `--foreground` — primary text
  - ...
- `--muted-foreground` — secondary text
  - ...

### Brand & Accent
- `--primary` — ...
- `--accent` — ...

### Status
- `--destructive` — ...

### Border
- `--border` — ...

### Chrome Runtime (invariant across variants)
- `--canvas-bg` — `<hex>`
- `--chrome-bg` — `<hex>`
- `--chrome-fg` — `var(--foreground)`
- `--chrome-muted-fg` — `var(--muted-foreground)`
- `--chrome-border` — `var(--border)`
- `--chrome-accent` — `var(--accent)`
- `--chrome-accent-fg` — `var(--accent-foreground)`

## §3 Typography values

### Font Family
- **Primary**: `<family>`, [fallback chain including CJK fonts]
- **Mono**: `<family>`, [fallback chain]

### Size Scale
- `--text-xs`   `<value>`  — [use case]
- `--text-sm`   `<value>`  — [use case]
- `--text-md`   `<value>`  — [use case]
- `--text-lg`   `<value>`  — [use case]
- `--text-xl`   `<value>`  — [use case]
- `--text-2xl`  `<value>`  — [use case]
- `--text-3xl`  `<value>`  — [use case]
- `--text-4xl`  `<value>`  — [use case]

### Weight Scale
- `--weight-normal`    `<n>`
- `--weight-medium`    `<n>`
- `--weight-semibold`  `<n>`
- `--weight-bold`      `<n>`

### Leading + Tracking
- Leading: tight / snug / normal / body — `<v>` / ...
- Tracking: tight / snug / normal / wide — `<v>` / ...

### Principles
- [3-5 use principles]

## §4 Shape values

### Radius
- `--radius-sm`     `<v>`
- `--radius`        `<v>`
- `--radius-full`   `<v>`

### Space
- `--space-1`..`--space-32` — `<v>`...

### Grid (if the theme has one)
- columns / rows / margins / gutters / track-*

### Shadow
- `--shadow-sm` / `--shadow` — `<v>`

## §5 Variants
- **<variant1>** — [when to use]
- **<variant2>** — [when to use]
- **<variantN>** — [when to use]

## §6 Decorative signature

### <Name 1>
- What / geometry / usage / HTML template (if any)

### <Name N>
- ...

## §7 Red Lines
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

/* 6. Decorative motif inline CSS (if §6 uses motifs like M18 / M20) */
```
````

---

## Constraints (do not violate)

- **`design.md` has only one place for CSS** — the §8 Append CSS block. All other sections (§1 – §7) hold values + prose only
- **Token naming aligns with shadcn** (the 15 standard names are locked)
- **No component-level secondary aliases** (don't introduce `--card-bg / --card-padding-x`)
- **Every Atmosphere paragraph must be anchored to concrete values** (no floating prose like "modern and clean")
- **Palette must list the full variant × token matrix** — the chrome template has 14 tokens × N variants; missing one row loses one row of CSS
- **The 7 chrome runtime tokens must be filled at the end of §2** (including `--canvas-bg / --chrome-*`)
- **§8 Append CSS has 6 fixed slots in fixed order** (chrome anchors → page-shell → page-content → utility → component overrides → motif); fill what's needed, leave the rest as empty comments

## Why

- **Prose + tables** lets the AI both intuitively imitate the theme when authoring a new slide (read §1 Atmosphere) and pull values precisely (read §2 – §4)
- **CSS lives outside design.md** to prevent "value updated but CSS forgot" drift; §8 is the single transformation path
- **Atmosphere requires 4 paragraphs** so the AI can't half-ass it — the felt sense must be drawn out in full
- **Decorative signature gets its own section** — visual systems that used to scatter across motif/redlines are now systematized
- **The blank template** gives every new theme a consistent starting point, avoiding the "starting from scratch" format drift each time
