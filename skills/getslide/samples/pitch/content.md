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

V2 pitch design 完整 9 页 deck — 验证全栈 chrome ring + hero-block + ambient-frames + contact-list + cards-row + pitch-card + solution-staggered + pill-highlight + feature-grid + stat-split + compare-stagger + chart-card + chart-line-default + pricing-3up。

**Variant 节奏**：lite → dark → forest → lite → dark → forest → forest → lite → lite

**叙事线**：cover (Take Control) → problem (broken) → solution (5 支柱) → features (5 things) → stats (4hrs/92%) → compare (4 维度) → chart (7× baseline) → pricing (3 plans) → closing (contact)

---

## P1 — Cover

- **block**: hero-block (`.h1-9 .v6-7`) + ambient-frames (cover variant)
- **variant**: lite
- **grid 模式**: strict 12×8
- **content**:
  - title: `Take Control of<br>Your Productivity!`
  - lede: `Work Smarter, Not Harder`
- **placement**:
  - hero-block: `.h1-9 .v6-7`（左下三分之一）+ default `align-self: end`
  - ambient-frames: 容器 `top:-300; right:-237; width:1100; height:1380`（字母 P 从 slide 上沿溢出）
- **chrome**:
  - top-left: `<span class="logo-mark">P</span><strong>Pitch.</strong>`
  - top-right: `© Brand, Inc<br>All Rights Reserved`
  - bottom-left: `Pitch`
  - bottom-right: `01`
- **notes**: |
    开场白：大家好。今天我想用八分钟的时间，介绍一份我们做了两年的
    **生产力工具**——它跟你用过的所有 productivity tool 都不一样。

    先抛一个反直觉的观察：现在市面上每一个生产力工具，本质上都在
    **训练你分心**——push 通知、活跃度仪表盘、聊天 + 文档 + 看板
    的多窗口切换……我们盯着这些屏幕越久，**真正完成的事反而越少**。

    接下来 9 页，我会按这个顺序讲清楚：第一段讲<strong>问题的本质</strong>，
    第二段讲<strong>我们的方案</strong>，第三段讲<strong>数据验证</strong>，
    最后讲价格和怎么开始。

## P2 — Problem

- **block**: cards-row (3 列) + pitch-card × 3 (num + label，无 icon/body)
- **variant**: dark（forest green cards on black bg，design.md §8 selector override）
- **grid 模式**: flow
- **content**:
  - title: `Productivity is broken.<br>Here's why.` (font-size 64px, line-height 1.2, h1-12)
  - body: `The tools we trust to make us focused now fight for our attention. The dashboards we rely on to measure progress reward activity instead. The result: more tabs, less done.` (font-size 22px, h1-8, margin-top 32)
  - 3 cards (num + label):
    - 01: `Notifications train us to ignore signal.`
    - 02: `Dashboards measure motion, not progress.`
    - 03: `Context-switching kills focus before it starts.`
- **placement**:
  - title `.h1-12`、body `.h1-8 margin-top: 32`、cards-row `.h1-12 margin-top: 64`（v1 流式 spacing）
- **chrome**: `02` page-number

## P3 — Solution

- **block**: solution-staggered (5 卡塔形 480/340/220/340/480, 贴接, 底直角对齐 chrome-rule) + page-content flow (title 含 M20 pill)
- **variant**: forest
- **grid 模式**: flow（title）+ abs（solution-staggered 自钉）
- **content**:
  - title (含 1 个 `<mark class="pill-highlight">`):
    ```
    Introducing Brand — the
    productivity tool that treats <mark class="pill-highlight">focus-by-default</mark>
    as the design, not the problem.
    ```
    (3 行结构，不要在第 1 段后立即 `<br>` 否则会自动换 4 行撞 solution-staggered 顶端)
  - 5 cards (label + body):
    - 01 `Quiet by default` / `No notifications until you opt in. Signal stays signal.`
    - 02 `Outcomes, not activity` / `Track shipped work, not tabs opened.`
    - 03 `One window` / `Everything in one place.`
    - 04 `Time-blocked focus` / `90-minute blocks, automatic. Calendar respects them.`
    - 05 `Ship reports` / `Weekly summary written for you. Share with your team.`
- **placement**:
  - title: `.h1-12 text-align: center` (font-size 64px)
  - solution-staggered: 自钉 `bottom: 96; left/right: 96; height: 480`
- **chrome**: `03` page-number

## P4 — Feature Grid

- **block**: feature-grid (5 卡 2x3 错位，pitch-card 强制 forest green) + page-content flow (title)
- **variant**: lite（卡片强制深绿填充）
- **grid 模式**: flow（title）+ abs（feature-grid 自钉）
- **content**:
  - title: `Five things this product<br>does on every screen.` (font-size 76px, h1-8)
  - 5 cards (icon + label + body，**无 num**，避免撞 240 min-height):
    - 01 (icon: rect+lines) `Quiet by default` / `No notifications until you opt in.`
    - 02 (icon: clock) `Outcomes UI` / `Track shipped work, not tabs opened.`
    - 03 (icon: bar+dot) `Focus pill` / `Single goal pinned to your day.`
    - 04 (icon: 3 rects) `Time-blocked` / `90-minute blocks, automatic.`
    - 05 (icon: rect-in-rect) `Ship reports` / `Auto-written weekly summary.`
- **placement**:
  - title: `.h1-8` (左 2/3 宽度)
  - feature-grid: 自钉 `bottom: calc(96 + 32) = 128; left/right: 96; grid-template-areas: "a b ." "c d e"`
- **chrome**: `04` page-number

## P5 — Stat Split

- **block**: page-content flow（左 h1-6 title+body, 右 h8-12 stat-split × 2）
- **variant**: dark
- **grid 模式**: flow
- **content**:
  - title: `Two ideas carry<br>the productivity gain.` (font-size 76px, h1-6)
  - body: `Quiet defaults and outcomes UI — these are the levers that distinguish this product from any other productivity tool. Internalize them and the rest of the design follows.` (font-size 22px, h1-6, color muted, margin-top 32)
  - 2 stat-blocks (num + caption):
    - `4 hrs` / `Daily focus time recovered, on average — measured by attention switches per session before / after.`
    - `92%` / `Users hit weekly shipped goal at least 3× before churning — vs 41% on competing tools.`
- **placement**:
  - title `.h1-6`、body `.h1-6 margin-top: 32`、stat-split `.h8-12`（同 row 1 与 title 横向并列）
- **chrome**: `05` page-number

## P6 — Compare Stagger

- **block**: page-content flow（左 h1-6 title+body）+ 4 abs compare-card zig-zag
- **variant**: forest
- **grid 模式**: flow（title）+ abs（4 张 compare-card 各自钉位）
- **content**:
  - title: `How this differs<br>from other tools.` (font-size 96px, h1-6 — 比常规 64-76 大一档强调对比)
  - body: `Compared to Slack, Linear, and Notion — the productivity tools you probably already use — this product takes opposite stances on four design choices.` (font-size 22px, h1-6, color muted)
  - 4 compare-cards (num + label + body):
    - 01 `Quiet defaults` / `Slack ships notifications on. We ship them off.`
    - 02 `Outcomes UI` / `Dashboards show shipped work, not opened tabs.`
    - 03 `Time-blocking` / `Calendar respects focus blocks. Others don't.`
    - 04 `Weekly reports` / `Auto-written summaries. Others make you write them.`
- **placement** (zig-zag N 字轨迹):
  - 01: `left: 96; bottom: 128`
  - 02: `left: 680; bottom: 258`
  - 03: `right: 96; bottom: 388`
  - 04: `right: 96; bottom: 128`
- **chrome**: `06` page-number

## P7 — Chart Narrative

- **block**: page-content flow（左 h1-5 title+body, 右 h7-12 best-fit + avoid-for box）+ abs chart-card (chart-line-default)
- **variant**: forest
- **grid 模式**: flow + abs（chart-card 自钉）
- **content**:
  - left (`.h1-5`):
    - h1: `Where this<br>tool fits.` (font-size 64px)
    - p: `The right-hand annotations describe team-fit by company stage.` (font-size 16px, muted)
  - right (`.h7-12`):
    - **Best fit:** `Series A startups · Engineering teams 10-50 · Product orgs that ship weekly · Remote-first companies` (font-size 18px, foreground)
    - **Avoid for:** (含 box, bg `var(--foreground) 8%`, border, padding 22 24, radius 16) `Solo freelancers (overkill). Enterprises > 500 (use Asana). Agencies running multiple client orgs (use Notion).`
  - chart (chart-line-default 适配版):
    - 8 数据点 Q1·24 → Q4·25
    - primary series: `This product · weekly shipped goals hit` (fg 2px line + 0.08 area fill + 5px dots)
    - secondary series: `Industry baseline` (muted 2px line)
    - 注释 pill at Q3·25 inflection: `↗ 7× baseline`
- **placement**:
  - left/right boxes: flow row 1 col 1-5 + col 7-12 自动并列
  - chart-card: 自钉 `bottom: 120 (96 + 24 breathing); left/right: 96`，含 SVG viewBox 1664×400
- **chrome**: `07` page-number

## P8 — Pricing 3-up

- **block**: page-content flow（title + body）+ abs pricing-row × 3
- **variant**: lite（深色卡 on 浅底）
- **grid 模式**: flow + abs（pricing-row 自钉）
- **content**:
  - title: `For when productivity must look like it costs something.` (font-size 40px, h1-7, line-height 1.15)
  - body: `Three plans, scaled by team size. Annual billing knocks 20% off the listed monthly rate.` (font-size 16px, h1-7, muted, margin-top 16)
  - 3 pricing-cards (label + price + desc + features × 4 + cta):
    - **Starter**：$0/Month / `Solo, learning the ropes.` / `1 user · 3 active goals · Daily focus blocks · Weekly auto-summary` / CTA `Start Free ↗`
    - **Pro**：~~$15~~ $9.99/Month / `Single contributor shipping weekly.` / `Unlimited goals · Calendar integration · Custom focus rules · Priority support` / CTA `Get Pro ↗`
    - **Team**：~~$24~~ $19.99/User/Mo / `Engineering teams 5-50.` / `All Pro features · Shared goal tracking · Team weekly reports · SSO & admin controls` / CTA `Get Team ↗`
- **placement**:
  - title `.h1-7`、body `.h1-7 margin-top: 16`
  - pricing-row: 自钉 `bottom: 120; left/right: 96; grid 3 列 gap 32; min-height 560`
- **chrome**: `08` page-number

## P9 — Closing

- **block**: hero-block(`--top`) (`.h1-9 .v3-8`) + ambient-frames (closing variant) + contact-list
- **variant**: lite
- **grid 模式**: strict 12×8
- **content**:
  - title: `The Future of Productivity<br>is Here.`
  - contact-list (`<ul>` + 3 `<li>`):
    - `hello@brand.com`
    - `+1 (415) 555-0100`
    - `www.brand.com`
- **placement**:
  - hero-block(`--top`): `.h1-9 .v3-8`（左上, `align-self: start`）
  - ambient-frames: 容器 `top: 180; right: -237`（cover 整体下移 480px，字母 P 从 slide 下沿溢出）
- **chrome**: `09` page-number
- **notes**: |
    收尾页：用三句话收束。<strong>第一</strong>，我们的产品反对当下
    productivity tool 的所有默认设置——通知默认关、仪表盘看交付不看活跃、
    一个窗口一个目标。

    <strong>第二</strong>，数据证明这个方向是对的：用户平均每天找回
    <strong>4 小时</strong>专注时间；周交付目标命中率 <strong>92%</strong>，
    是其他工具的两倍多。

    <strong>第三</strong>，今天不需要做决定。如果上面任何一点引起共鸣，
    页面右上有联系方式——<strong>email / phone / web</strong> 三选一，
    都能直接到我。<strong>谢谢。</strong>

---

## 文案守则

1. title 不带句尾标点（除非 `?` / `!`）—— pitch 风格
2. **每页 ≤ 1 个 M20 pill**（多 pill 互相竞争，强调失效）
3. closing 文案是"具体收尾"（contact / channel / call to action），不是"宣言镜像"——见 P1/P9 是 `bookend echo` 不是 `bookend repeat`
4. 数字带单位（`4 hrs` / `92%` / `7×`），不裸数字（`4` / `92` / `7`）—— oversized-numeral M07 在 88px 下需要单位锚点
