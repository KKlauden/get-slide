---
title: Pitch deck
author: kklauden
date: 2026-05-08
theme: ./design.md
brand: "Pitch."
brand-letter: "P"
copyright: "© Brand, Inc<br>All Rights Reserved"
---

# Pitch Deck

A complete 9-page V2 pitch sample — exercises the full chrome ring + hero-block + ambient-frames + contact-list + cards-row + pitch-card + solution-staggered + pill-highlight + feature-grid + stat-split + compare-stagger + chart-card + chart-line-default + pricing-3up.

**Variant rhythm**: lite → dark → forest → lite → dark → forest → forest → lite → lite

**Storyline**: cover (Take Control) → problem (broken) → solution (5 pillars) → features (5 things) → stats (4 hrs / 92%) → compare (4 dimensions) → chart (7× baseline) → pricing (3 plans) → closing (contact)

---

## P1 — Cover

- **block**: hero-block (`.h1-9 .v6-7`) + ambient-frames (cover variant)
- **variant**: lite
- **grid mode**: strict 12×8
- **content**:
  - title: `Take Control of<br>Your Productivity!`
  - lede: `Work Smarter, Not Harder`
- **placement**:
  - hero-block: `.h1-9 .v6-7` (lower-left third) + default `align-self: end`
  - ambient-frames: container `top:-300; right:-237; width:1100; height:1380` (the letter P spills past the slide's top edge)
- **chrome**:
  - top-left: `<span class="logo-mark">P</span><strong>Pitch.</strong>`
  - top-right: `© Brand, Inc<br>All Rights Reserved`
  - bottom-left: `Pitch`
  - bottom-right: `01`
- **notes**: |
    Opening: Hi everyone. In the next 8 minutes, I'd like to introduce a
    **productivity tool** we've been building for two years — and it's
    different from every productivity tool you've used.

    First, a counter-intuitive observation: every productivity tool on the
    market today is essentially **training you to be distracted** —
    push notifications, activity dashboards, the chat + doc + kanban
    multi-window switching. The longer we stare at these screens,
    **the less we actually finish**.

    Over the next 9 pages I'll cover, in order: first, **what the problem really is**;
    second, **our approach**; third, **the data validating it**; and finally
    pricing and how to start.

## P2 — Problem

- **block**: cards-row (3 columns) + pitch-card × 3 (num + label, no icon/body)
- **variant**: dark (forest-green cards on black bg, via design.md §8 selector override)
- **grid mode**: flow
- **content**:
  - title: `Productivity is broken.<br>Here's why.` (font-size 64px, line-height 1.2, h1-12)
  - body: `The tools we trust to make us focused now fight for our attention. The dashboards we rely on to measure progress reward activity instead. The result: more tabs, less done.` (font-size 22px, h1-8, margin-top 32)
  - 3 cards (num + label):
    - 01: `Notifications train us to ignore signal.`
    - 02: `Dashboards measure motion, not progress.`
    - 03: `Context-switching kills focus before it starts.`
- **placement**:
  - title `.h1-12`, body `.h1-8 margin-top: 32`, cards-row `.h1-12 margin-top: 64` (V1 flow spacing)
- **chrome**: `02` page-number

## P3 — Solution

- **block**: solution-staggered (5 cards in a tower 480/340/220/340/480, edge-to-edge, bottom flush against chrome-rule) + page-content flow (title contains M20 pill)
- **variant**: forest
- **grid mode**: flow (title) + abs (solution-staggered self-pinned)
- **content**:
  - title (contains 1 `<mark class="pill-highlight">`):
    ```
    Introducing Brand — the
    productivity tool that treats <mark class="pill-highlight">focus-by-default</mark>
    as the design, not the problem.
    ```
    (3-line structure; do NOT add `<br>` immediately after the first segment, otherwise it auto-wraps to 4 lines and hits the top of solution-staggered)
  - 5 cards (label + body):
    - 01 `Quiet by default` / `No notifications until you opt in. Signal stays signal.`
    - 02 `Outcomes, not activity` / `Track shipped work, not tabs opened.`
    - 03 `One window` / `Everything in one place.`
    - 04 `Time-blocked focus` / `90-minute blocks, automatic. Calendar respects them.`
    - 05 `Ship reports` / `Weekly summary written for you. Share with your team.`
- **placement**:
  - title: `.h1-12 text-align: center` (font-size 64px)
  - solution-staggered: self-pin `bottom: 96; left/right: 96; height: 480`
- **chrome**: `03` page-number

## P4 — Feature Grid

- **block**: feature-grid (5 cards 2×3 staggered, pitch-cards forced to forest-green fill) + page-content flow (title)
- **variant**: lite (cards forced to deep green)
- **grid mode**: flow (title) + abs (feature-grid self-pinned)
- **content**:
  - title: `Five things this product<br>does on every screen.` (font-size 76px, h1-8)
  - 5 cards (icon + label + body, **no num** — avoids hitting the 240 min-height):
    - 01 (icon: rect+lines) `Quiet by default` / `No notifications until you opt in.`
    - 02 (icon: clock) `Outcomes UI` / `Track shipped work, not tabs opened.`
    - 03 (icon: bar+dot) `Focus pill` / `Single goal pinned to your day.`
    - 04 (icon: 3 rects) `Time-blocked` / `90-minute blocks, automatic.`
    - 05 (icon: rect-in-rect) `Ship reports` / `Auto-written weekly summary.`
- **placement**:
  - title: `.h1-8` (left 2/3 width)
  - feature-grid: self-pin `bottom: calc(96 + 32) = 128; left/right: 96; grid-template-areas: "a b ." "c d e"`
- **chrome**: `04` page-number

## P5 — Stat Split

- **block**: page-content flow (left h1-6 title+body, right h8-12 stat-split × 2)
- **variant**: dark
- **grid mode**: flow
- **content**:
  - title: `Two ideas carry<br>the productivity gain.` (font-size 76px, h1-6)
  - body: `Quiet defaults and outcomes UI — these are the levers that distinguish this product from any other productivity tool. Internalize them and the rest of the design follows.` (font-size 22px, h1-6, color muted, margin-top 32)
  - 2 stat-blocks (num + caption):
    - `4 hrs` / `Daily focus time recovered, on average — measured by attention switches per session before / after.`
    - `92%` / `Users hit weekly shipped goal at least 3× before churning — vs 41% on competing tools.`
- **placement**:
  - title `.h1-6`, body `.h1-6 margin-top: 32`, stat-split `.h8-12` (same row 1, horizontally aligned with title)
- **chrome**: `05` page-number

## P6 — Compare Stagger

- **block**: page-content flow (left h1-6 title+body) + 4 abs compare-cards in zig-zag
- **variant**: forest
- **grid mode**: flow (title) + abs (4 compare-cards each self-pinned)
- **content**:
  - title: `How this differs<br>from other tools.` (font-size 96px, h1-6 — one step larger than typical 64–76 to emphasize the comparison)
  - body: `Compared to Slack, Linear, and Notion — the productivity tools you probably already use — this product takes opposite stances on four design choices.` (font-size 22px, h1-6, color muted)
  - 4 compare-cards (num + label + body):
    - 01 `Quiet defaults` / `Slack ships notifications on. We ship them off.`
    - 02 `Outcomes UI` / `Dashboards show shipped work, not opened tabs.`
    - 03 `Time-blocking` / `Calendar respects focus blocks. Others don't.`
    - 04 `Weekly reports` / `Auto-written summaries. Others make you write them.`
- **placement** (zig-zag N-shape):
  - 01: `left: 96; bottom: 128`
  - 02: `left: 680; bottom: 258`
  - 03: `right: 96; bottom: 388`
  - 04: `right: 96; bottom: 128`
- **chrome**: `06` page-number

## P7 — Chart Narrative

- **block**: page-content flow (left h1-5 title+body, right h7-12 best-fit + avoid-for box) + abs chart-card (chart-line-default)
- **variant**: forest
- **grid mode**: flow + abs (chart-card self-pinned)
- **content**:
  - left (`.h1-5`):
    - h1: `Where this<br>tool fits.` (font-size 64px)
    - p: `The right-hand annotations describe team-fit by company stage.` (font-size 16px, muted)
  - right (`.h7-12`):
    - **Best fit:** `Series A startups · Engineering teams 10-50 · Product orgs that ship weekly · Remote-first companies` (font-size 18px, foreground)
    - **Avoid for:** (with box, bg `var(--foreground) 8%`, border, padding 22 24, radius 16) `Solo freelancers (overkill). Enterprises > 500 (use Asana). Agencies running multiple client orgs (use Notion).`
  - chart (chart-line-default adapted):
    - 8 data points Q1·24 → Q4·25
    - primary series: `This product · weekly shipped goals hit` (fg 2px line + 0.08 area fill + 5px dots)
    - secondary series: `Industry baseline` (muted 2px line)
    - annotation pill at the Q3·25 inflection: `↗ 7× baseline`
- **placement**:
  - left/right boxes: flow row 1 cols 1-5 + cols 7-12 auto-aligned
  - chart-card: self-pin `bottom: 120 (96 + 24 breathing); left/right: 96`, with SVG viewBox 1664×400
- **chrome**: `07` page-number

## P8 — Pricing 3-up

- **block**: page-content flow (title + body) + abs pricing-row × 3
- **variant**: lite (dark cards on light base)
- **grid mode**: flow + abs (pricing-row self-pinned)
- **content**:
  - title: `For when productivity must look like it costs something.` (font-size 40px, h1-7, line-height 1.15)
  - body: `Three plans, scaled by team size. Annual billing knocks 20% off the listed monthly rate.` (font-size 16px, h1-7, muted, margin-top 16)
  - 3 pricing-cards (label + price + desc + features × 4 + cta):
    - **Starter**: $0/Month / `Solo, learning the ropes.` / `1 user · 3 active goals · Daily focus blocks · Weekly auto-summary` / CTA `Start Free ↗`
    - **Pro**: ~~$15~~ $9.99/Month / `Single contributor shipping weekly.` / `Unlimited goals · Calendar integration · Custom focus rules · Priority support` / CTA `Get Pro ↗`
    - **Team**: ~~$24~~ $19.99/User/Mo / `Engineering teams 5-50.` / `All Pro features · Shared goal tracking · Team weekly reports · SSO & admin controls` / CTA `Get Team ↗`
- **placement**:
  - title `.h1-7`, body `.h1-7 margin-top: 16`
  - pricing-row: self-pin `bottom: 120; left/right: 96; grid 3 columns gap 32; min-height 560`
- **chrome**: `08` page-number

## P9 — Closing

- **block**: hero-block(`--top`) (`.h1-9 .v3-8`) + ambient-frames (closing variant) + contact-list
- **variant**: lite
- **grid mode**: strict 12×8
- **content**:
  - title: `The Future of Productivity<br>is Here.`
  - contact-list (`<ul>` + 3 `<li>`):
    - `hello@brand.com`
    - `+1 (415) 555-0100`
    - `www.brand.com`
- **placement**:
  - hero-block(`--top`): `.h1-9 .v3-8` (upper-left, `align-self: start`)
  - ambient-frames: container `top: 180; right: -237` (overall shift down 480px from cover; the letter P spills past the slide's bottom edge)
- **chrome**: `09` page-number
- **notes**: |
    Closing: wrap up in three sentences. <strong>First</strong>, our product opposes
    every default in today's productivity tools — notifications off by default,
    dashboards measuring shipped not active, one window one goal.

    <strong>Second</strong>, the data backs the direction: users recover
    <strong>4 hours</strong> of focus per day on average; weekly shipped-goal hit rate
    <strong>92%</strong> — more than double any competing tool.

    <strong>Third</strong>, no decision needed today. If any of the above resonates,
    contact info is in the upper right — <strong>email / phone / web</strong>, pick one
    and you reach me directly. <strong>Thank you.</strong>

---

## Copy guidelines

1. Titles carry no terminal punctuation (except `?` / `!`) — pitch style
2. **At most 1 M20 pill per page** (multiple pills compete; emphasis fails)
3. Closing copy is a "concrete close" (contact / channel / call to action), not a "manifesto echo" — see P1/P9 as a `bookend echo`, NOT a `bookend repeat`
4. Numbers carry a unit (`4 hrs` / `92%` / `7×`), never bare digits (`4` / `92` / `7`) — the M07 oversized-numeral at 88px needs a unit anchor
