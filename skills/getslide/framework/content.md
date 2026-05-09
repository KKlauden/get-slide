---
name: content
role: framework-skeleton
description: Content structure contract — teaches the AI how to author a new content.md (one per deck)
---

# Content — Content Structure Contract

> One of the framework triplet: **runtime / design / content**. This is not a deck's content — it is the standard for "how to author a deck outline."
>
> Every new deck drops a `content.md` into its own project folder (it does **not** have to live under `samples/`; the AI puts the folder wherever the user picks). §1 schema, §3 three rules for notes, and §4 data-title are **required**; §2 rhythm and §6 templates are **guidance**.

## What this document does

The content layer is the deck-outline layer — what pages a deck has, what layout shape each page uses, what text each page carries, how the presenter speaks. It does **NOT** decide theme visuals (that's design.md), and does **NOT** decide how basic UI is written (that's the AI writing HTML+CSS directly against design.md tokens, with samples as paradigm reference).

Every deck's `content.md` is **an instruction manifest the AI uses to fill `framework/deck.html`**:
- frontmatter: deck metadata + theme pointer
- per-page H2 block: 4 sub-fields (`block` / `content` / `styling` / `notes`)

---

## §1 Schema (required)

### Frontmatter

```yaml
---
title:    <deck title>             # required
author:   <name>                   # required
date:     YYYY-MM-DD               # required, absolute date — never "today"
theme:    ./design.md              # required, points at the same-folder design.md
brand:    "<brand name>"           # optional, used by chrome anchors
brand-letter: "<single letter>"    # optional, used for logo-mark (e.g. "P")
copyright: "<copyright string>"    # optional, may contain <br>
---
```

### Per-page H2 + 4 sub-fields

Every page is a `## P<N> — <Title>` heading followed by 4 sub-fields (**bullet format, free-form**):

```markdown
## P1 — Cover

- **block**: <block name>(can chain + chrome anchor) [+ <sub-block>]
- **variant**: <variant name>         # corresponds to design.md §5 variant
- **grid mode**: strict | flow        # strict = use the 12×8 grid; flow = free abs pinning
- **content**:
  - title: `<text>`
  - lede:  `<text>`
  - <other fields, per the block's needs>
- **placement**:                      # only for strict mode / complex abs
  - <element>: `.h1-9 .v6-7` or `top:-300; right:-237; ...`
- **chrome**:
  - top-left:     `<filled content / SVG / multiline <br>>`
  - top-right:    `<...>`
  - bottom-left:  `<...>`
  - bottom-right: `<...>`
- **chrome-rule**: default | top | off    # top hairline position; omit = default
- **notes**: |
    [presenter verbatim, 150–300 chars (CN) / 150–200 words (EN), follows §3 three rules]
```

### Sub-field semantics

| Sub-field | Required | Notes |
|---|---|---|
| `block` | ✅ | Primary block name (per §5 index) + optional appended-block chain ("+ ambient-frames") |
| `variant` | ✅ | One of the variants declared in design.md §5 |
| `grid mode` | ✅ | strict (12×8 grid hitting tracks) / flow (free abs / regular flow) |
| `content` | ✅ | Text / data points needed by the block; field names match the block's slots |
| `placement` | ⚪ | strict mode: grid-track shorthand (`.h1-9 .v6-7`); flow mode: abs px notation |
| `chrome` | ✅ | The 8 anchors actually filled (write "empty" or omit the line if not used) |
| `chrome-rule` | ⚪ | one of default / top / off; omit = default |
| `styling` | ⚪ | Token overrides (`font-size: 64px` etc.); use design.md tokens, not hex |
| `notes` | ✅ | Presenter verbatim, 150–300 chars (CN) / 150–200 words (EN) |

### Field-value conventions

- **All text fields are wrapped in backticks**: `` ` `` `<br>` is part of the text
- **Paths / shorthand use code formatting**: ``\`top: 280, left, width 700\``
- **Grid-track shorthand**: `.h<a>-<b> .v<c>-<d>` (h = horizontal track, v = vertical)
- **Abs pinning**: integer px; multiple properties separated by `;`

---

## §2 Page count / narrative rhythm (guidance)

### 9-page pitch deck (standard)

```
1. Cover     —— topic hook + one-line lede
2. Contents  —— TOC (3–5 items)
3. About     —— who you are / what you do
4. Problem   —— problem statement + data points
5. Solution  —— solution overview
6. Solution sub —— solution details / data
7. Performance —— key metrics
8. Market    —— market / global data (can stack with dark gradient)
9. Closing   —— closing call to action
```

### 5-page short

```
1. Cover
2. Problem
3. Solution (with data)
4. Validation / Stats
5. CTA / Closing
```

### 3-page mini

```
1. Cover
2. Hero point (with data)
3. Closing
```

### Variant rhythm guidance

- **Use dark / accent variants for impact moments** — cover, closing, key data pages
- **Use the default variant for regular body** (lite / light)
- **Use a third variant for transitions / mood pages** (e.g. forest)
- Don't over-rotate — within 9 pages, no more than 3 dark pages, otherwise impact dilutes

---

## §3 Three rules for writing notes (required)

The `notes` sub-field on every page carries the presenter verbatim. **All three rules are non-negotiable.**

### Rule 1: prompts, not a script

❌ **Wrong** (full script — the presenter would read it aloud):
> Hello everyone, my name is Alex. Today I'd like to introduce FAULTLINE. FAULTLINE is a company that builds geological monitoring; our mission is to provide real-time risk intelligence for global critical infrastructure. We have already deployed over 14,000 sensors across 47 countries...

✅ **Right** (key terms + transition sentences — hooks for the presenter):
> Opening: today's intro is **FAULTLINE** — a **real-time geological risk intelligence network** for global critical infrastructure.
>
> First, an analogy: every large engineering project has sensors, but the readings are **siloed** — dams, bridges, subways are independent silos with no horizontal correlation.
>
> Next, three data points to show **scale**, then a product walkthrough, and we close with global deployment.

**Core characteristics**:
- **Bold** key nouns + numbers as eye-anchors for the presenter
- Transition sentences as standalone paragraphs ("Next..." / "First...")
- Don't write full sentences with filler — let the presenter complete the thought

### Rule 2: 150–300 chars (CN) / 150–200 words (EN) per page (≈ 2–3 minute pace)

Too short → the presenter loses the thread; too long → it becomes a script. Around 200 CN chars or 150 EN words is the sweet spot.

### Rule 3: spoken style, not written style

| ❌ Written | ✅ Spoken |
|---|---|
| Therefore | So |
| The aforementioned solution | This solution |
| In summary | All in all |
| In view of | Given |
| Aims to | Wants to |
| Should you require | If you need |

Spoken style also implies: short sentences first (< 25 chars / ≈ 15 words per sentence), clear subject-verb-object without circumlocution.

### Sample

```yaml
notes: |
  Closing page: wrap up with three escalating **orders of magnitude** — LOCAL 230 units, REGIONAL 14K units, PLANETARY **1.4M planetary grid**.

  Each order represents a different technical challenge: LOCAL solves "do they exist", REGIONAL solves "are they connected", PLANETARY solves "global unified observation".

  We sit at the middle of REGIONAL today. If FAULTLINE's work resonates, let's chat after. **Thank you.**
```

---

## §4 data-title is required

Every page-shell must carry `data-title="..."` — the O-key overview grid and the S-key presenter NEXT card depend on it.

The text after "— " in a content.md H2 (`## P1 — Cover`) **is** the value of `data-title`. The AI copies it verbatim when assembling the deck.

Examples:
- `## P1 — Cover` → `data-title="Cover"`
- `## P3 — About FAULTLINE` → `data-title="About FAULTLINE"`
- `## P6 — Sensor Performance` → `data-title="Sensor Performance"`

**Don't** use emoji, overly long titles (>30 characters), or special characters — the overview card has limited width.

---

## §5 What to write in the `block:` sub-field

v2.1 has **cut** the `components/` and `blocks/` library — basic UI (card / button / hero / numeral / pill / cards-row / feature-grid, etc.) is now written directly by the AI in HTML+CSS (consuming design.md tokens).

The `block:` sub-field describes **the page's "content shape"** — gives the AI a sense of what shape the page should hold; it does NOT bind to a specific class name:

| Content shape | Example `block:` syntax |
|---|---|
| Large hero + lede | `hero (centered title + lede)` or `hero-bottom-anchored (88px title)` |
| Multi-column compare | `2-up compare (left frame A vs right frame B)` or `4-up zig-zag compare` |
| Numbered list | `numbered-list (3 rows: prefix / label / desc, hairline divider)` |
| Mega stat | `mega-stat (200px num + caption)` or `3-up stat-cards` |
| Quote page | `pull-quote-only (centered serif italic, large)` |
| Chart | `chart-line (timeline + 2 series)` or `chart-gauge (73% with marker)` |
| Cover | `cover (large hero + ambient motif + chrome)` |
| Closing | `closing (recap 3 sentences + contact)` |

**Chart sub-field**: when using a chart, consult one of the 3 SVG patterns in `framework/charts/`. Other UI is written by the AI; consult `samples/<theme>/<theme>.html` to see how each theme implements similar shapes.

---

## §6 Common page templates (guidance)

### Cover
```markdown
- **block**: hero-block + ambient-frames
- **variant**: <cover variant>
- **grid mode**: strict
- **content**:
  - title: `<two-line hook>`
  - lede:  `<one-line tagline>`
- **placement**:
  - hero-block: `.h1-9 .v6-7`
- **chrome**:
  - top-left: `<logo + brand>`
  - top-right: `<copyright>`
  - bottom-right: `01`
- **notes**: |
  [Opening — introduce the theme + audience expectation]
```

### Contents / TOC
```markdown
- **block**: <toc / cards-row / custom>
- **variant**: lite
- **content**:
  - 3–5 TOC entries, each with num + label
- **chrome**: same as other pages
- **notes**: [a single transition sentence: "Today covers three things..."]
```

### Stat / Hero number
```markdown
- **block**: stat-split / numeral
- **variant**: <accent variant, dark/accent>
- **content**:
  - number: `92%`
  - label: `<one-line context>`
- **notes**: [a sentence of narrative behind the number]
```

### Chart narrative
```markdown
- **block**: chart-card + <chart-line-default etc.>
- **variant**: lite or accent
- **content**:
  - title: `<chart title>`
  - data: [12, 18, 27, 38, 51, 64, 76, 86]
  - annotation: `↗ 7× baseline`
- **notes**: [chart highlights + interpretation]
```

### Closing / CTA
```markdown
- **block**: hero-block + contact-list (optional)
- **variant**: lite or accent
- **content**:
  - title: `<closing hook>`
  - body:  `<next steps>`
- **notes**: [closing words — "Thanks" / "Let's chat after" / Q&A invitation]
```

---

## §7 content.md blank template

For a new deck, copy this skeleton verbatim into `<my-deck>/content.md` and start filling (`<my-deck>` is the folder you picked for this deck):

```markdown
---
title:        <deck title>
author:       <name>
date:         YYYY-MM-DD
theme:        ./design.md
brand:        "<brand>"
brand-letter: "<X>"
copyright:    "<copyright string>"
---

# <Deck Name>

[A narrative paragraph — what this deck is, who it's for, the variant rhythm]

**Variant rhythm** (N pages): v1 → v2 → v3 → ...
**Storyline**: cover → ... → closing

---

## P1 — Cover
- **block**: hero-block + ambient-frames
- **variant**: <v>
- **grid mode**: strict
- **content**:
  - title: `<...>`
  - lede:  `<...>`
- **placement**:
  - hero-block: `.h1-9 .v6-7`
- **chrome**:
  - top-left: `<...>`
  - top-right: `<...>`
  - bottom-right: `01`
- **notes**: |
    [Opening — 150–300 chars / 150–200 words]

## P2 — <Title>
...

## P9 — Closing
...
```

---

## Constraints (do not violate)

- **Every page has 4 sub-fields**: block / variant / content / chrome (chrome's items may be "empty", but each of the 8 anchors must be explicitly declared)
- **Every page has notes** — if not written, default to an empty string, but the H2 block must contain a `notes:` sub-field (even empty — it's a reminder to fill in later)
- **`theme:` points at `./design.md`** — never an absolute path, never a different theme file
- **`block:` cannot be empty** — every page needs a primary block name
- **Token overrides go in the `styling:` sub-field** — don't mix CSS into the `content` sub-field
- **H2 title = data-title** — copy 1:1 into the page-shell when assembling

## Why

- **4 sub-fields per page** force the AI to settle "what brick / what content / where / how to talk about it" up front — preventing half-baked pages
- **Notes are required** so presenter mode (S key) has data on day one; pages without verbatim may leave notes empty, but **the field must exist**
- **H2 = data-title** ensures the overview grid and presenter cards have content for all 9 pages
- **Frontmatter `theme:` pointer** lets one content.md re-skin — change the path and a different theme drives the deck
- **Page-count rhythm / variant rhythm** are guidance, not law — every deck owns its own narrative; this doc only ensures the AI doesn't produce a 0-cover deck or a 12-page all-dark mess
