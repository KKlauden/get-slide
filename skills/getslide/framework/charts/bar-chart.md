---
name: bar-chart
kind: pattern-contract
purpose: "Vertical bars chart — structure + data → pixel algorithm. Not a CSS template; visual decisions per design.md."
---

# Bar Chart Pattern

> **This is a structure + algorithm contract, NOT a CSS template.** Read this to learn: what structure to use / how to compute heights. Visual details (radius / type / color / width) **must** be hand-written from the current deck's `design.md` — copying directly causes theme mismatch (an archive-styled chart will visually clash with a CHASSAN-styled deck).

## Applies to

Each bar's height reflects **one data value** — real data visualization (e.g. muon survival rate by altitude / monthly sales / fault counts).

**Doesn't apply to**:
- progress / fill bars (every bar same height; percentage fill represents progress) → use `chart-gauge.md`
- "rhythm" decorative bars (the varying-width thin-bar row in archive P4) — write yourself, no algorithm needed

## Required structure

```text
<container.bar-chart>
  ├── chart title + sub (in the theme's voice)
  ├── <bars area>
  │    ├── bars container (flex horizontal / grid)
  │    │    └── N bar units
  │    │         ├── one bar (height per algorithm below)
  │    │         ├── value label (top, optional but recommended)
  │    │         └── x-axis label (bottom)
  │    ├── x-axis baseline hairline (1px var(--border))
  │    └── y-axis marker (optional: max / mid / 0 indicator)
  └── caption (data note / source / reading, in the theme's voice)
```

**Minimum**: bars area + x-axis labels + chart title or caption (**don't ship a bare row of N bars**).

## Data → pixel algorithm (hard rule)

```text
container_height = 280–360px (decided by available page space)
padding_top      ≥ 32px       ← room for value labels
padding_bottom   ≥ 36px       ← room for x-axis labels
drawing_height   = container_height - padding_top - padding_bottom

max_value        = max(data_values)

For each bar:
  bar_height = (data_value / max_value) × drawing_height
  if bar_height < 6px:
      bar_height = 6px           ← visual floor; otherwise the data point disappears
```

**Example**: data = `[1000, 560, 310, 175, 98, 55]`, container 320px, padding 36+36
→ drawing 248px, max 1000
→ heights = `[248, 138.88, 76.88, 43.4, 24.3, 13.64]` px

**Visual sanity check**: shortest / tallest = 13.64 / 248 = 5.5%, equal to 55/1000 = 5.5% ✓ The chart strictly reflects the data.

## Visuals must be decided per design.md (**don't copy default values from this doc**)

| Decision | Look up in design.md |
|---|---|
| bar **radius** | §4 Shape `{shape.radius.default}` / `{shape.radius.sm}` (**don't** default to `border-radius: 6px` — CHASSAN style may want 12–20, archive style wants 0) |
| bar **color** | §2 Palette `{colors.accent}` / `{colors.primary}` / `{colors.foreground}` per the theme's emphasis system |
| bar **width** | matches the x-axis label width (32–80px); 6 wide bars vs 12 narrow bars convey different visual weight |
| value label **type** | §3 Typography display family (same family as hero — serif / sans depending on theme) |
| value label **size** | 14–22px, harmonized with the page body |
| x-axis label **type** | §3 Typography system-label (archive style = mono uppercase; CHASSAN style = serif italic or sans tracking-wide) |
| **bar gap** | `flex justify-content: space-between` or `gap`, tuned for visual density |
| **baseline / grid** | 1px hairline + `{colors.border}`; dense charts can add horizontal dashed |

## Anti-patterns (**don't do this**)

❌ Copy `--bar-fill: 28%` percent var — that's the progress scenario; real data computes px independently per bar

❌ Hard-coded visual values
```css
.bar { border-radius: 6px; }              /* 6 is a vibe, not theme-anchored */
.bar-x { font-family: "Geist Mono"; }     /* archive contamination */
.bar { background: #2563eb; }             /* breaks the token system */
```

❌ `cp` another deck's `.bar-chart` CSS wholesale — that brings archive / pitch visual decisions; either start blank or `cp` then **rewrite line-by-line** with design.md tokens

❌ Omit value labels or x-axis labels — a real-data chart without labels = unreadable

❌ Skip the bar < 6px floor — the data point disappears

## Why

- Charts are **data visualization**; proportions must strictly reflect data — algorithm cannot be vibes-driven
- Charts **follow the theme** — a CHASSAN chart and an archive chart look completely different; the doc presets nothing
- Algorithms (height computation / floor) are easy to get wrong; the doc gets them right once
