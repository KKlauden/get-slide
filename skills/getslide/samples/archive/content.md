---
title: Faultline — Autonomous Geological Intelligence Network
author: kklauden
date: 2026-05-08
theme: ./design.md
brand: "Faultline"
---

# Faultline Pitch Deck

`archive` 主题（工业 system pitch / file-archive aesthetic）的 9 页虚构 deck。题材：FAULTLINE —— 一家虚构的全球地质监控公司，在断层带、隧道、坝体、关键基建上铺自主传感器节点，实时捕捉地震 / 结构 / 地表位移信号。

**Variant 节奏**（9 页）：dark → lite → lite → lite → lite → lite → lite → dark → lite

**叙事线**: cover → contents → about → problem → solution → sensor performance (solution sub) → system performance → global impact → network reach (market sub)

> 文案完全虚构，跟 archive 风格题材自洽。FAULTLINE / 47 countries / 14,000+ stations / planetary mesh 等数据点是为 deck 设计的，**不**对应任何真实公司。

---

## P1 — Cover

- **variant**: dark
- **chrome-rule**: off (`data-chrome-rule="off"`)
- **layout**: abs 钉位
- **content**:
  - chrome-top-left: `PITCH DOCUMENT<br>FAULTLINE ©2026`
  - chrome-top-right: `VERSION: V2.0.4 / BUILD_0612`
  - chrome-bottom-* : 空
  - archive-tags 三标签（mid-page 横排，flex gap 200）:
    - `PERSISTENT.`
    - `PRECISE.`
    - `PREDICTIVE.`
  - archive-hero（bottom-left 88px bold）:
    ```
    REAL-TIME FAULT INTELLIGENCE
    FOR GLOBAL INFRASTRUCTURE
    ```
  - 背景：dark variant 默认 gradient（中间晕染散开）
- **notes**: |
    开场白：大家好，今天我想用十分钟的时间，介绍一下
    <strong>FAULTLINE</strong> 在做的事——一个面向全球关键基础设施的
    <strong>实时地质风险情报网络</strong>。

    先做一个简单类比：现在所有大型工程项目都装了大量传感器，
    但传感器读数是<strong>孤岛式</strong>的——大坝有自己的监控系统，
    桥梁有自己的，地铁有自己的，没有人在做横向关联。我们要解决的
    就是这个"孤岛"问题。

    接下来，我会先讲三个数据点说明问题的<strong>规模</strong>，
    然后看我们的产品怎么落地，最后用全球部署情况说明我们已经到的位置。

## P2 — Contents

- **variant**: lite
- **chrome-rule**: 默认（top: 720px）
- **content**:
  - chrome-top-left: `INDEX_REF: FILE#0201<br>SYS_MODE: ACTIVE`
  - chrome-top-right: `archive-brand-mark` SVG（dashed 圆环）
  - chrome-bottom-right: `PITCH DOCUMENT<br>FAULTLINE ©2026`
  - archive-toc（3×3 grid，`grid-auto-flow: column`）:
    - `1.0 ABOUT` / `2.0 PROBLEM` / `3.0 SOLUTION`
    - `4.0 SENSOR PERFORMANCE` / `5.0 NETWORK COVERAGE` / `6.0 FIELD CASE STUDIES`
    - `7.0 ROADMAP` / `8.0 PARTNERS & GRID` / `9.0 CLOSING / CTA`
  - archive-mega-title（bottom-left 180px bold）: `CONTENTS`

## P3 — About FAULTLINE

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **content**:
  - chrome-top-left: `INDEX_REF: FILE#0201<br>SYS_MODE: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `ABOUT_PAGE` + `01` + brand-mark
  - archive-page-title (`top: 280, left`): `ABOUT FAULTLINE`（单 span，88px bold，不拆 gap）
  - archive-body (`top: 460, left, width 700`):
    - `FAULTLINE operates a planet-scale geological intelligence network, surfacing seismic and structural anomalies before they become incidents.`
  - archive-stat-list (`bottom: 56, left, width 880`): 3 stat row
    - `99%` / `SENSOR UPTIME` / `Continuous fault data acquisition across 14,000+ stations.`
    - `30%` / `FASTER ALERT TIME` / `Real-time anomaly detection from edge-deployed inference.`
    - `45%` / `COVERAGE EXPANSION` / `Network growth across underserved tectonic regions.`
  - archive-image-frame (`top: 280, right, width 880, bottom: 56`): 灰底占位 `[ image placeholder ]`

## P4 — The Problem

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **content**:
  - chrome-top-left: `SYS_REF: 02.0<br>STATUS: ANALYZED`
  - chrome-top-right (`archive-page-meta`): `PROBLEM_PAGE` + `02` + brand-mark
  - archive-page-title: `THE PROBLEM`（88px bold）
  - archive-page-number: `2.0`
  - archive-body (`top: 460, left, width 700`):
    - `Traditional seismic monitoring relies on sparse station networks and post-event analysis. Critical infrastructure operators learn about subsurface anomalies hours — sometimes days — after they begin. By the time an alert is issued, structural integrity may already be compromised.`
  - archive-numbered-list (`bottom: 56, left, width 800`): 3 numbered row
    - `01.` / `SPARSE NETWORKS` / `Coverage gaps leave entire fault zones unmonitored.`
    - `02.` / `POST-EVENT ANALYSIS` / `Insights arrive after damage is done.`
    - `03.` / `SLOW RESPONSE` / `Manual review delays critical decisions.`
  - 右半：
    - archive-metric-panel (`top: 460, right, width 800`): title `SUBSURFACE BLINDSPOT INDEX` + rule + desc `Sparse coverage extends time-to-detection across critical zones.` + bar-chart
    - bar-chart: 10 vertical bars，左 6 粗（12px）右 4 细（3px），高度统一 80px
    - archive-metric-bignum (`bottom: 56, right`): `63%`（240px bold）

## P5 — The Solution

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: 左 body / 中 image-frame / 右 layer-list
- **content**:
  - chrome-top-left: `SYS_REF: 03.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `SOLUTION_PAGE` + `03` + brand-mark
  - archive-page-title: `THE<br />SOLUTION`（两行 88px bold）
  - archive-page-number: `3.0`
  - archive-body 第一段 (`top: 540, left, width 360`):
    - `FAULTLINE deploys an integrated sensor mesh — autonomous nodes embedded along fault lines, structural assets, and underground utilities. Each node analyzes local seismic and structural data on-edge, then relays findings to a central correlation engine for cross-site pattern detection.`
  - archive-body 第二段 footer (`bottom: 56, left, width 360`):
    - `<strong>FAULTLINE</strong> Mesh operates as one continuous geological observatory.`
  - archive-image-frame (`left: 660, width 600, top 280, bottom 56` inline override)
  - archive-layer-list (`top: 480, right, width 540`): 4 layer-item（title/rule/desc 右对齐）
    - `INTERFACE LAYER` / `Where operators visualize alerts and incident timelines.`
    - `CORRELATION LAYER` / `Cross-site pattern detection across the mesh.`
    - `SENSOR ARRAY` / `Edge nodes capturing seismic and structural signals on site.`
    - `DATA INGESTION` / `Receives and normalizes telemetry from connected stations.`

## P6 — Sensor Performance (Solution sub-page 03.1)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: top 3 stat-cards row + bottom 全宽 dark coverage-panel
- **content**:
  - chrome-top-left: `SYS_REF: 03.1<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `SOLUTION_PAGE` + `04` + brand-mark
  - archive-stat-cards (`top: 280`, 3 列 gap 16):
    - `SENSOR FIDELITY` / `Continuous capture across distributed nodes in the field.` / `2.07M` / `readings per minute network-wide`
    - `CROSS-SITE SYNC` / `Sub-second correlation across the deployed mesh.` / `180K` / `events correlated per second`
    - `NETWORK STABILITY` / `Sustained uptime across remote sensor stations.` / `9.3/10` / `mesh integrity index`
  - archive-coverage-panel (`top: 660, bottom: 56`, dark `#0a0a0a` 反衬 + `--foreground: #ffffff` 翻白):
    - `.coverage-title` (32px bold): `HIGH-RISK ZONE COVERAGE`
    - `.coverage-desc`: `FAULTLINE currently monitors 73% of identified high-risk structural and seismic zones — accelerating toward planetary coverage.`
    - `.coverage-bignum` (`top: 56, left: calc(56 + 0.73 * (100% - 112))`, 56px bold): `73%`
    - chart-gauge（block）: 0%/50%/100% labels space-between + 50 bars (37 亮 + 13 暗) + marker 73%

## P7 — System Performance (Performance-page 04.0)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: 左半 image-frame placeholder；右半 page-title + 2 perf-rows
- **content**:
  - chrome-top-left: `SYS_REF: 04.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `PERFORMANCE_PAGE` + `05` + brand-mark
  - archive-image-frame (`left: 64, width: 580, top: 280, bottom: 56`): `[ image placeholder ]`
  - archive-page-title (`top: 280, left: 720`): `SYSTEM<br>PERFORMANCE`（88px bold 两行）
  - archive-page-number: `4.0`
  - archive-perf-list (`top: 480, left: 720, right: 64`): 2 perf-row stack flex-column gap 64
    - row1: title `SIGNAL PROCESSING RATE` + rule + content（左 `(+40%)` 48px bold tight，右 `357K` 浅灰大数 + `signals processed per minute`）
    - row2: title `NETWORK INTEGRITY INDEX` + rule + content（左长段落 `FAULTLINE Mesh sustains continuous integrity across remote and high-risk deployments. Adaptive load distribution ensures no station is overburdened, while predictive maintenance minimizes downtime and keeps cycle latency under 0.2s.`，右 `55K` + `nodes active in field`）

## P8 — Global Impact (Market-page 05.0)

- **variant**: dark + `data-bg-mode="market"`（底部蓝向上渐变 → 26% 之上纯黑）
- **chrome-rule**: `data-chrome-rule="top"`
- **mid-rule**: 自定义 `.archive-impact-rule`（top: 720，左端不顶页边、右端顶页边；arrow 在 rule 左侧同水平面）
- **scoped 透明度**: `[data-bg-mode="market"]` 把 chrome-rule + impact-rule 改 30%
- **content**:
  - chrome-top-left: `SYS_REF: 05.0<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `MARKET_PAGE` + `06` + brand-mark
  - chrome-bottom-left (inline override sans + 非 mono + 非 uppercase): `Annual Asset Coverage`
  - archive-mega-stat (`top: 280, left: 64`，280px bold): `$5.5`
  - archive-mega-stat-label (`top: 600, left: 320`): `TRILLION MONITORED`
  - archive-impact-rule (`top: 720, left: 360, right: 64`)
  - archive-arrow (`top: 684, left: 64`，220×72 SVG，line 在 viewBox y=36 跟 rule 同水平面)
  - archive-page-title (`top: 280, left: 880`): `GLOBAL IMPACT`（单行 88px bold）
  - archive-page-number: `5.0`
  - archive-impact-body (`top: 760, right: 64, width: 700`): 双色段落
    - 第一句白色: `FAULTLINE supports critical infrastructure operators across 47 countries — capturing seismic and structural data at planetary scale, surfacing anomalies before they cascade into failures.`
    - 第二句 muted: `By integrating autonomous sensing with predictive correlation, FAULTLINE compresses incident response time and unlocks measurable resilience across the world's most critical assets.`

## P9 — Network Reach (Market sub-page 05.1)

- **variant**: lite
- **chrome-rule**: `data-chrome-rule="top"`
- **layout**: 左 zone（GLOBAL REACH 标题 + body + `5.1` 左下角）；右 zone（4 列 stage-cols）
- **content**:
  - chrome-top-left: `SYS_REF: 05.1<br>STATUS: ACTIVE`
  - chrome-top-right (`archive-page-meta`): `MARKET_PAGE` + `09` + brand-mark
  - archive-page-title (`top: 280, left: 64`): `GLOBAL<br>REACH`（两行 88px bold）
  - archive-body (`top: 600, left: 64, width: 360`):
    - `FAULTLINE's mesh continues to extend across new fault zones and infrastructure operators — from initial deployment trials to full continental coverage.`
  - archive-page-number (左下角 inline override): `5.1`
  - archive-stage-grid (`top: 280, left: 480, right: 64, bottom: 56`，4 等宽列):
    - col1 `data-tone="blue"`: `230K` / `PILOT NODES` / `Initial monitoring trials across regional fault zones.` / 16 bars `--bar-fill: 28%`
    - col2 `data-tone="muted-light"`: `520K` / `DEPLOYMENT` / `Field-active nodes integrated with operator systems.` / 16 bars `--bar-fill: 52%`
    - col3 `data-tone="muted-mid"`: `890K` / `CONTINENTAL` / `Cross-border mesh coverage of high-risk zones.` / 16 bars `--bar-fill: 70%`
    - col4 `data-tone="dark"`: `1.4M` / `PLANETARY` / `Unified observation grid powered by FAULTLINE.` / 16 bars `--bar-fill: 95%`
- **notes**: |
    收尾页：用三个递进的<strong>数量级</strong>来收束：从 LOCAL 部署的
    230 台站，到 REGIONAL 的 14K 台，再到我们的远期目标——<strong>1.4M
    行星级网格</strong>。

    每一个数量级的跨越，背后都是不同的技术挑战：LOCAL 解决的是"有没有"，
    REGIONAL 解决的是"互联互通"，PLANETARY 要解决"全球统一观测"。

    我们今天处于 REGIONAL 阶段的中段。如果对 FAULTLINE 在做的事感兴趣，
    欢迎下来聊。<strong>谢谢。</strong>

---

## chrome ring 跨页状态总结

- chrome-rule 三态：`off`（cover）/ `top`（P3-P9 主体）/ default 720（P2 contents）
- chrome-top-right `archive-page-meta` 复合（label + 数字 + brand-mark）：`flex-end` 右对齐 + `gap: 100` + 数字固定宽 `64px text-align center` —— 数字 x 位置跨页固定不漂移
- chrome-bottom 通常空；P2 用作 copyright，P8 用作 `Annual Asset Coverage` label

## 文案守则（archive / FAULTLINE 题材）

1. **mono 系统标签气质**：chrome-top-left 用 `SYS_REF: XX.X` / `STATUS: ACTIVE` / `INDEX_REF: FILE#XXXX` 流水号语言
2. **brand 全大写**：FAULTLINE 在正文里出现一律全大写（视觉对齐 mono 标签气质）
3. **段落完整句子**：除标题外，body 文案以完整句子结束（句号 / 句末标点）
4. **数字带单位**：`14,000+ stations` / `47 countries` / `0.2s` / `73%` —— 不裸数字
5. **避免营销腔**：用工程 / 系统语言（`mesh` / `nodes` / `correlation` / `telemetry` / `inference` / `cycle`），不用 `transformative` / `revolutionary` / `seamless ecosystem`
6. **题材自洽**：所有数据点（节点数、监控范围、性能指标）都跟"地质监控网络"题材对齐，不出现 SaaS / 自动化 / 通用 enterprise 词汇
