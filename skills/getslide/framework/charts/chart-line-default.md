---
name: chart-line-default
kind: pattern-contract
purpose: "Line chart — 1–2 series + dot + area-fill + annotation pill. SVG geometry contract + data → coordinate algorithm, NOT a visual template."
source: "@shadcn/chart-line-default adapted to SVG-only (v2 self-contained constraint)"
---

# Chart Line Default Pattern

> **This is an SVG geometry + algorithm contract, NOT a visual template.** Read this to learn: viewBox / coordinate system / data point position algorithm. Color / type / grid style / annotation pill shape **must** be hand-written from the current deck's `design.md` — copying numbers like `var(--foreground) fill-opacity 0.08` directly causes theme mismatch.

## Applies to

**Time / sequential series** (Q1–Q8 monthly trend / weekly data / experiment progression) line chart. 1–2 series, 1 key inflection point annotation.

**Doesn't apply to**:
- Bar data (each bar reflects an independent value) → `bar-chart.md`
- Single progress value → `chart-gauge.md`
- Scatter / radar / pie → write your own SVG, referencing this doc's algorithm and token usage

## Required structure (inside SVG)

```text
<svg viewBox="0 0 W H">
  ├── grid (N horizontal dashed lines)
  ├── y-axis labels (left, e.g. 0 / 25 / 50 / 75 / 100)
  ├── x-axis labels (bottom, time / sequential points)
  ├── area-fill path (below primary, semi-transparent area)   ← optional
  ├── secondary line path (if a 2nd series exists, draw first)
  ├── primary line path
  ├── data points (circles + background-color ring, drawn above the line)
  └── annotation pill (optional, 1 inflection point)
       ├── leader line (pill ↔ data point, dashed)
       ├── pill background + stroke
       └── pill text
```

## SVG geometry contract (recommended viewBox 1664 × 400)

```text
viewBox       = "0 0 1664 400"        ← chart-card container constraint (measured on archive / pitch)
                                          can change to 1280×320 / 2000×500 etc. per page space

x_axis_left   = 60                    ← room for y-axis labels
x_axis_right  = 1640                  ← right padding
plot_width    = 1640 - 60 = 1580

y_axis_top    = 24                    ← room for annotation pill
y_axis_bottom = 352                   ← room for x-axis labels
plot_height   = 352 - 24 = 328
```

## Data → coordinate algorithm

```text
N points (per series)              ← recommended 8 (pitch P7 measured: legible upper bound on a single page)

For each data_point i (0-indexed):
  x = x_axis_left + i × (plot_width / (N - 1))
  y = y_axis_bottom - (data_value / max_value) × plot_height

  if value < 0 (allow_negative):
      mid_y      = (y_axis_top + y_axis_bottom) / 2
      half_h     = plot_height / 2
      y          = mid_y - (data_value / max_abs_value) × half_h

Grid lines (recommended 5 evenly spaced):
  y = y_axis_top + i × plot_height / 4    (i = 0..4)
                                          → 24, 106, 188, 270, 352
```

**Example (V3 muon, if drawn as a line chart)**:
- max = 1000, 6 points (note: 8 is best for line; 6 is sparse)
- (10km / 1000) → x=60,    y=24
- (8km  / 560)  → x=376,   y=168
- (0km  / 55)   → x=1640,  y=334

## Visuals must be decided per design.md

| Decision | Look up in design.md |
|---|---|
| primary line **color** | §2 Palette `{colors.accent}` / `{colors.foreground}` |
| primary line **stroke width** | 2–3px (archive sharp style 2px; rounded themes 3–4px) |
| secondary line **color** | `{colors.muted-foreground}` |
| **area-fill** | `fill="var(--accent)" fill-opacity="?"` (**opacity is per theme; do NOT hard-code** 0.08 — archive dark needs 0.14, CHASSAN cream may need 0.05–0.08) |
| data point **radius** | 4–7px (tuned to line stroke width) |
| data point **ring** | `stroke="var(--background)" stroke-width="2"` (prevents background-color collision) |
| **grid lines** | dashed `stroke-dasharray="3 5"` + `{colors.border}` (archive style is dense; CHASSAN style may omit grid and keep only the hairline baseline) |
| **axis label type** | §3 Typography system-label |
| **axis label size** | 14–16px |
| **annotation pill shape** | rect rx matches §4 Shape `{shape.radius.full}` or `{shape.radius.default}` |
| **annotation pill text** | one phrase ≤ 6 words; tonality follows the theme — serif italic / sans uppercase / mono each fit different themes |

## Anti-patterns

❌ **Hard-coded colors**
```svg
<path stroke="#000" />              <!-- hard-coded hex -->
<rect fill="#f3f3f3" />             <!-- hard-coded grey -->
```

❌ **Copying `fill-opacity="0.08"` without thinking about the theme** — 0.08 is too weak on archive dark; CHASSAN cream may need 0.05–0.10; tune per theme

❌ **Squeezing more than 8 points** — 6/10/12 points are all OK (data-driven), but **don't** cram 12 points into a 1664 viewBox (overcrowded); widen the viewBox instead

❌ **Multiple annotation pills** — 1 is enough; multiple compete for attention

❌ **Omitting data points** — drawing the line without dots is the minimal-trend look (fits an extreme-minimal theme, but only when the theme calls for it); most decks need dots for readability

## Why

- 8 data points / 2 series is the upper bound for a single-page deck chart — more points = visually crowded
- v2 has no build tooling; data → coordinates is hand-computed and pasted as SVG — the doc gets the algorithm right once
- Chart visuals follow the theme: an archive line chart and a CHASSAN line chart look completely different; the doc presets nothing

## v2 transcription conventions for new charts (non-line)

When writing a new chart (pie / area / radar / scatter, etc.), follow these general rules:

1. **viewBox is 1664×400 or the same aspect ratio** — match the line-chart container
2. **`preserveAspectRatio="xMidYMid meet"`** + `aria-label="{title}"`
3. **All colors via `var(--xxx)`**: primary `var(--accent)` / secondary `var(--muted-foreground)` / grid `var(--border)` / data point ring `var(--background)`; **no hard-coded colors**
4. **Axis labels**: `font-size="14-16"` + `var(--muted-foreground)`; y `text-anchor="end"`, x `text-anchor="middle"`
5. **Data points** (line / area): `circle r="5"` + `fill="var(--accent)"` + `stroke="var(--background)" stroke-width="2"`
6. **No inline padding / border / legend** — those belong to the outer chart-card / page-shell wrapper
7. **Data hard-coded in SVG** — v2 has no build tools; coordinates are hand-computed (or generated once via Python and pasted)
