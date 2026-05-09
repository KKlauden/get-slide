---
name: chart-gauge
kind: pattern-contract
purpose: "Linear gauge — N discrete bar ticks of progress + label range + triangle marker. Structure + geometry contract, not a CSS template."
---

# Chart Gauge Pattern

> **This is a structure + geometry contract, NOT a CSS template.** Read this to learn: what parts a gauge is composed of / how the marker geometry aligns. Visual details (color / marker shape / label type / radius) **must** be written by hand against the current deck's `design.md` tokens.

## Applies to

**Single progress value** visualization ("X% coverage" / "X% completion" / "X% adoption"). Uses N discrete bar ticks + a marker triangle to point at the current position.

**Doesn't apply to**:
- Real-data charts (each bar reflects an independent value) → use `bar-chart.md`
- Circular / semicircular gauge → SVG arc, write yourself

## Required structure

```text
<container.chart-gauge>
  ├── label range (top or sides label 0% / mid / 100%)
  ├── <gauge body>
  │    ├── N discrete bars (default 50; range 25–100)
  │    │    ├── bright bars × M     ← before current progress
  │    │    └── dim bars × (N-M)    ← after current progress
  │    └── marker (triangle / dot / vertical line, etc., points at marker_pos%)
  └── current value display (optional, pinned above marker_x)
```

## Geometry alignment hard rule

```text
total_bars       = 50 (recommended; ≥ 25 for granularity, ≤ 100 to stay sharp)
marker_pos_pct   = data_value / max_value × 100         e.g. 73 / 100 = 73%

bright_bars      = round(total_bars × marker_pos_pct / 100)   e.g. 73% × 50 = 36.5 → 37
dim_bars         = total_bars - bright_bars                   e.g. 50 - 37 = 13

marker_x_offset  = marker_pos_pct % of track_width
```

**Visual alignment hard rule**: the marker triangle must land on the bright/dim cutover **within ± 1 bar width**. If `bright_bars` computes to 37 but the marker visually lands at 33%, either the math is wrong or the CSS has drifted.

## Sizing algorithm

```text
gauge_track_height  = 48–72px (per available page space)
gap                 = 1–2px
bar_width           = (track_width - (total_bars - 1) × gap) / total_bars
                      ≈ 1–3px (tight enough for granular feel)
```

## Visuals must be decided per design.md

| Decision | Look up in design.md |
|---|---|
| bright bar **color** | §2 Palette `{colors.accent}` / `{colors.primary}` / `{colors.foreground}` |
| dim bar **color** | `{colors.muted}` or same hue as bright bars + `opacity: 0.3` |
| marker **color** | same token as the bright bars (visual anchor consistency) |
| marker **shape** | triangle (default CSS triangle) / dot / vertical line / pin — pick per theme's decorative system |
| label **type** | §3 Typography (archive = mono uppercase; CHASSAN = serif italic + sans mix) |
| label **layout** | flex `space-between` (left 0% / mid / right 100%), or stacked beside the gauge |
| bar **radius** | §4 Shape `{shape.radius.sm}` or 0 (archive sharp = 0; rounded themes 1–2px) |
| **value display** (if any) | §3 Typography hero size, pinned above marker_x |

## Anti-patterns

❌ The marker position **mathematically misaligns** with the bright/dim cutover — the gauge loses meaning

❌ Bar count < 20 — granularity too coarse, the gauge feel disappears

❌ Using a single continuous gradient bar instead of N discrete bars — that's a progress bar, not a gauge

❌ Copying archive's coverage-panel mono uppercase + white-on-black palette — that's an archive-deck-specific decision

## Why

- A gauge's visual force comes from the **tick count** — the audience can count the difference between 73 and 75
- Marker = the data's true position; bar ticks ≠ marker (ticks are a discrete approximation), so the two **must** align visually
- N=50 is the sweet spot measured on the archive sample (clear granularity + bars not too thin); adjustable
