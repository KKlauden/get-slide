# references/charts/

**Chart Pattern Contract Layer** — 3 chart patterns: bar / gauge / line.

All archive / pitch references in this folder are **negative examples illustrating theme-specificity**, NOT styles to copy.

## What this is

Not a "component library" and not "CSS templates" — these are **structure + algorithm contracts**:

- **Structure**: what parts a chart is composed of (bars area / value labels / x-axis labels / baseline / caption, etc.)
- **Algorithm**: precise conversion from data → pixels (proportional scaling on max / bar-height floor / marker geometry alignment)
- **Visuals decided per design.md** (radius / color / type / width — look up the design.md tokens; default values are NOT copied here)

## Why we kept these

The components / blocks libraries (card / button / hero / numeral, etc.) have been fully deleted — basic UI is written directly by modern LLMs without issue. But **charts are different**:

- Charts are **data visualization** — wrong proportions = bug (V3 testing tolerated 0.1px height drift, but proportions must match data, not vibes)
- Chart geometry (SVG viewBox / data → coordinate formulas / gauge marker alignment) is easy for the AI to get wrong
- Charts are easily **contaminated by old paradigms** (V3 testing found that when the AI read bar-chart.md sourced from archive's paradigm, it subconsciously brought archive's 6px radius / mono labels into a CHASSAN-style deck)

So these 3 patterns are rewritten as **"structure + algorithm only, no visuals"** — after reading, the AI learns how to compute and what parts to use; **visual decisions must be hand-written against the current deck's design.md**.

## The 3 patterns

| File | Use for | Don't use for |
|---|---|---|
| `bar-chart.md` | Real data visualization (each bar's height reflects an independent value) | progress / fill bars (use gauge) / decorative "rhythm" bars (write yourself) |
| `chart-gauge.md` | Single progress value ("X% coverage") + N discrete bar ticks + marker | Real data charts (use bar-chart) / circular / semicircular gauge (write SVG arc yourself) |
| `chart-line-default.md` | Time / sequential series (1–2 series + dot + area-fill + annotation) | Bar data (bar-chart) / scatter / radar / pie (write yourself) |

## How to use

1. **Read the pattern**: understand structure / algorithm / required-element checklist
2. **Compute the data**: apply the pattern's algorithm to convert data → height / coordinates
3. **Write HTML + CSS**: HTML structure matches the pattern; CSS is **hand-written from design.md** — look up §2 Palette / §3 Typography / §4 Shape tokens; do NOT copy any chart CSS from archive / pitch
4. **Verify**: bar-height ratio = data ratio? marker aligns with the cutover? colors are visually consistent with the deck?

## Not in this folder

- **Basic UI** (card / button / hero / numeral / pill / cards-row, etc.) — AI writes these
- **Chart CSS / SVG visual templates** — they bring theme contamination (V3 pain point); the AI must rewrite visuals per design.md
- **New chart types** (pie / radar / scatter, etc.) — see the "v2 transcription conventions for new charts" at the end of `chart-line-default.md` and write them yourself
