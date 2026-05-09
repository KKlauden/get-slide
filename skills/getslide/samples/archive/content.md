---
title: Faultline — Autonomous Geological Intelligence Network
author: kklauden
date: 2026-05-08
theme: ./design.md
brand: "Faultline"
---

# Faultline Pitch Deck

A 9-page fictional deck in the `archive` theme (industrial system pitch / file-archive aesthetic). Topic: FAULTLINE — a fictional global geological-monitoring company that deploys autonomous sensor nodes along fault zones, tunnels, dam bodies, and critical infrastructure to capture seismic / structural / surface-displacement signals in real time.

**Variant rhythm** (9 pages): dark → lite → lite → lite → lite → lite → lite → dark → lite

**Storyline**: cover → contents → about → problem → solution → sensor performance (solution sub) → system performance → global impact → network reach (market sub)

> The copy is entirely fictional, internally consistent with the archive style. Data points like FAULTLINE / 47 countries / 14,000+ stations / planetary mesh are designed for the deck and **do not** correspond to any real company.

---

## P1 — Cover

- **variant**: dark
- **chrome-rule**: off (`data-chrome-rule="off"`)
- **layout**: abs pinning
- **content**:
  - chrome-top-left: `PITCH DOCUMENT<br>FAULTLINE ©2026`
  - chrome-top-right: `VERSION: V2.0.4 / BUILD_0612`
  - chrome-bottom-*: empty
  - archive-tags 3 tags (mid-page horizontal, flex gap 200):
    - `PERSISTENT.`
    - `PRECISE.`
    - `PREDICTIVE.`
  - archive-hero (bottom-left, 88px bold):
    ```
    REAL-TIME FAULT INTELLIGENCE
    FOR GLOBAL INFRASTRUCTURE
    ```
  - Background: dark variant default gradient (radial wash from center)
- **notes**: |
    Opening: Hi everyone. In the next 10 minutes, I'd like to introduce
    what <strong>FAULTLINE</strong> is doing — a <strong>real-time
    geological risk intelligence network</strong> for global critical infrastructure.

    First, a quick analogy: every large engineering project today carries
    a lot of sensors, but the readings are <strong>siloed</strong> — dams
    have their own monitoring system, bridges have theirs, subways have
    theirs, no one is correlating across them. The "silo" problem is
    what we're solving.

    Next, I'll start with three data points showing the <strong>scale</strong>
    of the problem, then walk through how our product lands in the field,
    and finally use global deployment to show where we are now.

## P2 — Contents

- **variant**: lite
- **chrome-rule**: default (top: 720px)
- **content**:
  - chrome-top-left: `INDEX_REF: FILE#0201<br>SYS_MODE: ACTIVE`
  - chrome-top-right: `archive-brand-mark` SVG (dashed circle ring)
  - chrome-bottom-right: `PITCH DOCUMENT<br>FAULTLINE ©2026`
  - archive-toc (3×3 grid, `grid-auto-flow: column`):
    - `1.0 ABOUT` / `2.0 PROBLEM` / `3.0 SOLUTION`
    - `4.0 SENSOR PERFORMANCE` / `5.0 NETWORK COVERAGE` / `6.0 FIELD CASE STUDIES`
    - `7.0 ROADMAP` / `8.0 PARTNERS & GRID` / `9.0 CLOSING / CTA`
  - archive-mega-title (bottom-left, 180px bold): `CONTENTS`

## P3 — About FAULTLINE

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **content**:
  - chrome-top-left: `INDEX_REF: FILE#0201<br>SYS_MODE: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `ABOUT_PAGE` + `01` + brand-mark
  - archive-page-title (`top: 280, left`): `ABOUT FAULTLINE` (single span, 88px bold, no gap split)
  - archive-body (`top: 460, left, width 700`):
    - `FAULTLINE operates a planet-scale geological intelligence network, surfacing seismic and structural anomalies before they become incidents.`
  - archive-stat-list (`bottom: 56, left, width 880`): 3 stat rows
    - `99%` / `SENSOR UPTIME` / `Continuous fault data acquisition across 14,000+ stations.`
    - `30%` / `FASTER ALERT TIME` / `Real-time anomaly detection from edge-deployed inference.`
    - `45%` / `COVERAGE EXPANSION` / `Network growth across underserved tectonic regions.`
  - archive-image-frame (`top: 280, right, width 880, bottom: 56`): grey-fill placeholder `[ image placeholder ]`

## P4 — The Problem

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **content**:
  - chrome-top-left: `SYS_REF: 02.0<br>STATUS: ANALYZED`
  - chrome-top-right (`archive-page-meta`): `PROBLEM_PAGE` + `02` + brand-mark
  - archive-page-title: `THE PROBLEM` (88px bold)
  - archive-page-number: `2.0`
  - archive-body (`top: 460, left, width 700`):
    - `Traditional seismic monitoring relies on sparse station networks and post-event analysis. Critical infrastructure operators learn about subsurface anomalies hours — sometimes days — after they begin. By the time an alert is issued, structural integrity may already be compromised.`
  - archive-numbered-list (`bottom: 56, left, width 800`): 3 numbered rows
    - `01.` / `SPARSE NETWORKS` / `Coverage gaps leave entire fault zones unmonitored.`
    - `02.` / `POST-EVENT ANALYSIS` / `Insights arrive after damage is done.`
    - `03.` / `SLOW RESPONSE` / `Manual review delays critical decisions.`
  - Right half:
    - archive-metric-panel (`top: 460, right, width 800`): title `SUBSURFACE BLINDSPOT INDEX` + rule + desc `Sparse coverage extends time-to-detection across critical zones.` + bar-chart
    - bar-chart: 10 vertical bars; left 6 thick (12px), right 4 thin (3px); uniform 80px height
    - archive-metric-bignum (`bottom: 56, right`): `63%` (240px bold)

## P5 — The Solution

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: left body / center image-frame / right layer-list
- **content**:
  - chrome-top-left: `SYS_REF: 03.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `SOLUTION_PAGE` + `03` + brand-mark
  - archive-page-title: `THE<br />SOLUTION` (two lines, 88px bold)
  - archive-page-number: `3.0`
  - archive-body paragraph 1 (`top: 540, left, width 360`):
    - `FAULTLINE deploys an integrated sensor mesh — autonomous nodes embedded along fault lines, structural assets, and underground utilities. Each node analyzes local seismic and structural data on-edge, then relays findings to a central correlation engine for cross-site pattern detection.`
  - archive-body paragraph 2 footer (`bottom: 56, left, width 360`):
    - `<strong>FAULTLINE</strong> Mesh operates as one continuous geological observatory.`
  - archive-image-frame (`left: 660, width 600, top 280, bottom 56`, inline override)
  - archive-layer-list (`top: 480, right, width 540`): 4 layer items (title / rule / desc, right-aligned)
    - `INTERFACE LAYER` / `Where operators visualize alerts and incident timelines.`
    - `CORRELATION LAYER` / `Cross-site pattern detection across the mesh.`
    - `SENSOR ARRAY` / `Edge nodes capturing seismic and structural signals on site.`
    - `DATA INGESTION` / `Receives and normalizes telemetry from connected stations.`

## P6 — Sensor Performance (Solution sub-page 03.1)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: top 3 stat-cards row + bottom full-width dark coverage-panel
- **content**:
  - chrome-top-left: `SYS_REF: 03.1<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `SOLUTION_PAGE` + `04` + brand-mark
  - archive-stat-cards (`top: 280`, 3 columns gap 16):
    - `SENSOR FIDELITY` / `Continuous capture across distributed nodes in the field.` / `2.07M` / `readings per minute network-wide`
    - `CROSS-SITE SYNC` / `Sub-second correlation across the deployed mesh.` / `180K` / `events correlated per second`
    - `NETWORK STABILITY` / `Sustained uptime across remote sensor stations.` / `9.3/10` / `mesh integrity index`
  - archive-coverage-panel (`top: 660, bottom: 56`, dark `#0a0a0a` inverse + `--foreground: #ffffff` reversed white):
    - `.coverage-title` (32px bold): `HIGH-RISK ZONE COVERAGE`
    - `.coverage-desc`: `FAULTLINE currently monitors 73% of identified high-risk structural and seismic zones — accelerating toward planetary coverage.`
    - `.coverage-bignum` (`top: 56, left: calc(56 + 0.73 * (100% - 112))`, 56px bold): `73%`
    - chart-gauge (block): 0% / 50% / 100% labels space-between + 50 bars (37 bright + 13 dim) + marker at 73%

## P7 — System Performance (Performance page 04.0)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: left half image-frame placeholder; right half page-title + 2 perf-rows
- **content**:
  - chrome-top-left: `SYS_REF: 04.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `PERFORMANCE_PAGE` + `05` + brand-mark
  - archive-image-frame (`left: 64, width: 580, top: 280, bottom: 56`): `[ image placeholder ]`
  - archive-page-title (`top: 280, left: 720`): `SYSTEM<br>PERFORMANCE` (88px bold, two lines)
  - archive-page-number: `4.0`
  - archive-perf-list (`top: 480, left: 720, right: 64`): 2 perf-row stacked, flex-column gap 64
    - row1: title `SIGNAL PROCESSING RATE` + rule + content (left `(+40%)` 48px bold tight, right `357K` muted-grey big-num + `signals processed per minute`)
    - row2: title `NETWORK INTEGRITY INDEX` + rule + content (left long paragraph `FAULTLINE Mesh sustains continuous integrity across remote and high-risk deployments. Adaptive load distribution ensures no station is overburdened, while predictive maintenance minimizes downtime and keeps cycle latency under 0.2s.`, right `55K` + `nodes active in field`)

## P8 — Global Impact (Market page 05.0)

- **variant**: dark + `data-bg-mode="market"` (bottom blue rising gradient → above 26% pure black)
- **chrome-rule**: `data-chrome-rule="top"`
- **mid-rule**: custom `.archive-impact-rule` (top: 720; left edge does not touch the page edge, right edge does; arrow sits to the rule's left at the same baseline)
- **scoped opacity**: `[data-bg-mode="market"]` drops chrome-rule + impact-rule to 30% opacity
- **content**:
  - chrome-top-left: `SYS_REF: 05.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `MARKET_PAGE` + `06` + brand-mark
  - chrome-bottom-left (inline override sans + non-mono + non-uppercase): `Annual Asset Coverage`
  - archive-mega-stat (`top: 280, left: 64`, 280px bold): `$5.5`
  - archive-mega-stat-label (`top: 600, left: 320`): `TRILLION MONITORED`
  - archive-impact-rule (`top: 720, left: 360, right: 64`)
  - archive-arrow (`top: 684, left: 64`, 220×72 SVG, line at viewBox y=36 sits at the same baseline as the rule)
  - archive-page-title (`top: 280, left: 880`): `GLOBAL IMPACT` (single line, 88px bold)
  - archive-page-number: `5.0`
  - archive-impact-body (`top: 760, right: 64, width: 700`): two-color paragraph
    - first sentence in white: `FAULTLINE supports critical infrastructure operators across 47 countries — capturing seismic and structural data at planetary scale, surfacing anomalies before they cascade into failures.`
    - second sentence in muted: `By integrating autonomous sensing with predictive correlation, FAULTLINE compresses incident response time and unlocks measurable resilience across the world's most critical assets.`

## P9 — Network Reach (Market sub-page 05.1)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: left zone (GLOBAL REACH title + body + `5.1` lower-left); right zone (4-column stage-cols)
- **content**:
  - chrome-top-left: `SYS_REF: 05.1<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `MARKET_PAGE` + `09` + brand-mark
  - archive-page-title (`top: 280, left: 64`): `GLOBAL<br>REACH` (two lines, 88px bold)
  - archive-body (`top: 600, left: 64, width: 360`):
    - `FAULTLINE's mesh continues to extend across new fault zones and infrastructure operators — from initial deployment trials to full continental coverage.`
  - archive-page-number (lower-left, inline override): `5.1`
  - archive-stage-grid (`top: 280, left: 480, right: 64, bottom: 56`, 4 equal-width columns):
    - col1 `data-tone="blue"`: `230K` / `PILOT NODES` / `Initial monitoring trials across regional fault zones.` / 16 bars `--bar-fill: 28%`
    - col2 `data-tone="muted-light"`: `520K` / `DEPLOYMENT` / `Field-active nodes integrated with operator systems.` / 16 bars `--bar-fill: 52%`
    - col3 `data-tone="muted-mid"`: `890K` / `CONTINENTAL` / `Cross-border mesh coverage of high-risk zones.` / 16 bars `--bar-fill: 70%`
    - col4 `data-tone="dark"`: `1.4M` / `PLANETARY` / `Unified observation grid powered by FAULTLINE.` / 16 bars `--bar-fill: 95%`
- **notes**: |
    Closing: wrap up with three escalating <strong>orders of magnitude</strong> —
    from LOCAL deployment at 230 stations, to REGIONAL at 14K stations, and our
    long-term goal — <strong>1.4M planetary grid</strong>.

    Each leap between magnitudes is a different technical challenge: LOCAL
    solves "do they exist", REGIONAL solves "are they connected",
    PLANETARY solves "global unified observation".

    We sit in the middle of the REGIONAL stage today. If FAULTLINE's work
    resonates, let's chat after. <strong>Thank you.</strong>

---

## Chrome ring cross-page state summary

- chrome-rule three states: `off` (cover) / `top` (P3–P9 main body) / default 720 (P2 contents)
- chrome-top-right `archive-page-meta` composite (label + numeral + brand-mark): `flex-end` right-aligned + `gap: 100` + numeral fixed-width `64px text-align center` — the numeral's x position is locked across pages, never drifts
- chrome-bottom usually empty; P2 uses it for copyright; P8 uses it for the `Annual Asset Coverage` label

## Copy guidelines (archive / FAULTLINE topic)

1. **Mono system-label voice**: chrome-top-left uses serial-number language like `SYS_REF: XX.X` / `STATUS: ACTIVE` / `INDEX_REF: FILE#XXXX`
2. **Brand in all caps**: every appearance of FAULTLINE in body copy is uppercase (visually aligns with the mono-label voice)
3. **Complete sentences in body**: outside titles, body copy ends with a complete sentence (period / terminal punctuation)
4. **Numerals carry units**: `14,000+ stations` / `47 countries` / `0.2s` / `73%` — never bare digits
5. **Avoid marketing voice**: use engineering / system language (`mesh` / `nodes` / `correlation` / `telemetry` / `inference` / `cycle`); avoid `transformative` / `revolutionary` / `seamless ecosystem`
6. **Topic-internal coherence**: every data point (node count, coverage scope, performance metric) aligns with the "geological monitoring network" topic; no SaaS / automation / generic enterprise vocabulary
