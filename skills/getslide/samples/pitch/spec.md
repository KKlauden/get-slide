---
name: pitch
description: "现代科技 pitch deck，单一 grotesk + 三 variant 轮换 + 全 chrome"
version: 0.2.0
source: "迁移自 old/skills/getslide/designs/pitch/index.md；命名对齐 shadcn"
---

# Pitch Theme

YC / TechCrunch 风格的 pitch deck 主题。**Signature**：

1. **单一 grotesk 字族**——Cabinet Grotesk 通吃全部，无 serif/italic
2. **3 variant 轮换**——每页通过 `data-variant` 切换 lite / dark / forest 三种背景
3. **全 chrome**——每一页都有 8 锚点的外壳（logo / copyright / 页码 / brand / chrome-rule），cover 也不例外
4. **Monochrome**——`--accent` = `--primary` = `--foreground`，强调靠 M20 pill 不靠颜色
5. **几何装饰**——M18 ambient frames（圆角矩形 SVG）+ M20 pill highlight

## Token 命名约定

- **Palette 层**完全对齐 [shadcn ui](https://ui.shadcn.com/docs/theming)：`--background / --foreground / --card / --card-foreground / --primary / --primary-foreground / --secondary / --secondary-foreground / --muted / --muted-foreground / --accent / --accent-foreground / --destructive / --destructive-foreground / --border`
- **Scale 层**自定义（shadcn 用 Tailwind config，我们没有 Tailwind）：`--text-* / --space-* / --radius* / --weight-* / --leading-* / --tracking-* / --shadow*`
- **不设 component 二级 alias**（无 `--card-bg / --card-padding-x` 等）——component CSS 直接 `background: var(--card)` / `padding: var(--space-6)`

## Palette / Type / Shape

```css
:root {
  /* ─────────── 非 variant 共享 token ─────────── */
  --canvas-bg: #dcdcdc;            /* 外层画布（chrome runtime 用） */
  --chrome-bg: #f0f0f0;            /* sidebar / topbar */

  /* ─────────── typography (scale) ─────────── */
  --font-display: "Cabinet Grotesk", "Geist", "Inter",
                  -apple-system, BlinkMacSystemFont, "Segoe UI",
                  "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei",
                  sans-serif;
  --font-body:    var(--font-display);
  --font-mono:    ui-monospace, "JetBrains Mono", "SF Mono", Menlo, monospace;

  --text-xs:        16px;
  --text-sm:        18px;
  --text-md:        22px;
  --text-lg:        32px;
  --text-xl:        44px;
  --text-2xl:       76px;
  --text-3xl:       88px;
  --text-4xl:      120px;

  --weight-normal:    400;
  --weight-medium:    500;
  --weight-semibold:  600;
  --weight-bold:      700;

  --leading-tight:   1.05;
  --leading-snug:    1.15;
  --leading-normal:  1.4;
  --leading-body:    1.55;

  --tracking-tight:   -0.02em;
  --tracking-snug:    -0.015em;
  --tracking-normal:   0;
  --tracking-wide:     0.22em;

  /* ─────────── shape (scale) ─────────── */
  --radius-sm:    16px;
  --radius:       20px;
  --radius-full:  999px;

  --space-1:    4px;
  --space-2:    8px;
  --space-3:   12px;
  --space-4:   16px;
  --space-6:   24px;
  --space-8:   32px;
  --space-12:  48px;
  --space-16:  64px;
  --space-24:  96px;
  --space-32: 128px;

  /* ─────────── grid (12 × 8) ─────────── */
  --grid-columns:        12;
  --grid-rows:            8;
  --grid-gutter:        24px;
  --grid-row-gap:       24px;
  --grid-margin-x:      96px;
  --grid-margin-top:    96px;
  --grid-margin-bottom: 96px;
  --track-eyebrow: 1 / 4;
  --track-title:   1 / 9;
  --track-body:    1 / 9;
  --track-hero:    1 / 9;
  --track-stat:    7 / -1;
  --track-aside:   9 / -1;

  /* ─────────── elevation (scale) ─────────── */
  --shadow-sm: none;          /* pitch 主题不用阴影 */
  --shadow:    none;

  /* ─────────── 默认 variant: lite ─────────── */
  --background:             #f0f0f0;
  --foreground:             #0d2a25;
  --muted:                  rgba(13, 42, 37, 0.06);
  --muted-foreground:       rgba(13, 42, 37, 0.55);
  --border:                 rgba(13, 42, 37, 0.20);
  --card:                   #f0f0f0;
  --card-foreground:        #0d2a25;
  --primary:                #0d2a25;
  --primary-foreground:     #f0f0f0;
  --secondary:              #e6e6e6;
  --secondary-foreground:   #0d2a25;
  --accent:                 #0d2a25;
  --accent-foreground:      #f0f0f0;
  --destructive:            #0d2a25;     /* monochrome 限制：见 Red lines */
  --destructive-foreground: #ffffff;

  /* ─────────── chrome runtime (固定浅色 chrome，跨 variant 不变) ─────────── */
  --chrome-fg:        #0d2a25;
  --chrome-muted-fg:  rgba(13, 42, 37, 0.55);
  --chrome-border:    rgba(13, 42, 37, 0.20);
  --chrome-accent:    #0d2a25;
  --chrome-accent-fg: #f0f0f0;
}

/* ─────────── 显式 variant: lite（对称，嵌套场景能切回）─────────── */
[data-variant="lite"] {
  --background:             #f0f0f0;
  --foreground:             #0d2a25;
  --muted:                  rgba(13, 42, 37, 0.06);
  --muted-foreground:       rgba(13, 42, 37, 0.55);
  --border:                 rgba(13, 42, 37, 0.20);
  --card:                   #f0f0f0;
  --card-foreground:        #0d2a25;
  --primary:                #0d2a25;
  --primary-foreground:     #f0f0f0;
  --secondary:              #e6e6e6;
  --secondary-foreground:   #0d2a25;
  --accent:                 #0d2a25;
  --accent-foreground:      #f0f0f0;
  --destructive:            #0d2a25;
  --destructive-foreground: #ffffff;
}

/* ─────────── variant: dark ─────────── */
[data-variant="dark"] {
  --background:             #0a0a0a;
  --foreground:             #faf9f5;
  --muted:                  rgba(255, 255, 255, 0.06);
  --muted-foreground:       rgba(255, 255, 255, 0.55);
  --border:                 rgba(255, 255, 255, 0.10);
  --card:                   rgba(0, 0, 0, 0.30);   /* 叠黑：深底上更深 */
  --card-foreground:        #faf9f5;
  --primary:                #faf9f5;
  --primary-foreground:     #0a0a0a;
  --secondary:              rgba(0, 0, 0, 0.30);
  --secondary-foreground:   #faf9f5;
  --accent:                 #faf9f5;
  --accent-foreground:      #0a0a0a;
  --destructive:            #faf9f5;
  --destructive-foreground: #0a0a0a;
}

/* ─────────── variant: forest ─────────── */
[data-variant="forest"] {
  --background:             #0d2a25;
  --foreground:             #faf9f5;
  --muted:                  rgba(255, 255, 255, 0.06);
  --muted-foreground:       rgba(255, 255, 255, 0.55);
  --border:                 rgba(255, 255, 255, 0.12);
  --card:                   rgba(0, 0, 0, 0.24);   /* 叠黑：来自 v1 sample */
  --card-foreground:        #faf9f5;
  --primary:                #faf9f5;
  --primary-foreground:     #0d2a25;
  --secondary:              rgba(0, 0, 0, 0.24);
  --secondary-foreground:   #faf9f5;
  --accent:                 #faf9f5;
  --accent-foreground:      #0d2a25;
  --destructive:            #faf9f5;
  --destructive-foreground: #0d2a25;
}

/* ─────────── component-level variant overrides
   spec 是颜色出处——这里硬编码品牌色 OK，component 内不能硬编码 ─────────── */

/* P2 problem-numbered：dark variant 下 pitch-card 用 forest green solid
   block，而非默认的叠黑 var(--card)。设计意图：黑底上要 solid block，
   不是半透明叠层。 */
[data-variant="dark"] .pitch-card {
  background: #0d2a25;
  border-color: transparent;
}

/* ─────────── 全局 base
   TODO: * reset / overflow 等迁移到 chrome runtime 桶（待建）
   ─────────── */
* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  font-family: var(--font-body);
  color: var(--foreground);
  background: var(--canvas-bg);
  -webkit-font-smoothing: antialiased;
}
```

## Chrome / Page Shell

Pitch 主题 signature：**8 锚点 chrome ring**——top-left logo / top-right copyright / bottom-left brand / bottom-right page-number / 之间的 chrome-rule。每页都有，**不可省略**。

### Template

```html
<section class="page-shell" data-variant="lite">  <!-- lite | dark | forest -->

  <div class="chrome-top-left">
    <span class="logo-mark">P</span>             <!-- deck initial letter -->
    <strong>Brand.</strong>
  </div>

  <div class="chrome-top-right">
    © Brand, Inc<br>
    All Rights Reserved
  </div>

  <hr class="chrome-rule" />

  <div class="chrome-bottom-left">Brand.</div>
  <div class="chrome-bottom-right">01</div>

  <div class="page-content">
    {页面内容}
  </div>

</section>
```

`logo-mark` 内容是 deck 名首字母（"P" for Pitch / "B" for Brand 等）。空心方块也行，但放字母会让 chrome 同时承担 brand 印章功能，更精致。

### Style

```css
.page-shell {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: var(--background);
  color: var(--foreground);
  padding: var(--grid-margin-top) var(--grid-margin-x) var(--grid-margin-bottom);
  overflow: hidden;
}

.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  grid-template-rows:    repeat(var(--grid-rows), 1fr);
  column-gap: var(--grid-gutter);
  row-gap:    var(--grid-row-gap);
  height: 100%;
}

/* ───── flow 模式：12 列单行 + 顶部对齐，元素流式从上往下堆 ─────
   切换：<div class="page-content" data-layout="flow">
   适用：title + body + cards 顺次往下排的中间页 (P2-P8)
   元素只锚 column (.h*-*)，不锚 row；间距用 inline margin-top 控制。
   page-content 加 96 上下 padding 给内容跟 chrome-top/bottom 留 buffer
   （叠加 page-shell padding 96 = 总 192 from edge，对齐 v1）。 */
.page-content[data-layout="flow"] {
  grid-template-rows: none;
  grid-auto-rows: auto;
  align-content: start;
  padding-top:    var(--grid-margin-top);
  padding-bottom: var(--grid-margin-bottom);
  row-gap: 0;
}

/* ───── 8 锚点 chrome ring ───── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-size: var(--text-sm);
  color: var(--muted-foreground);
  z-index: 1;
}

/* 上锚点 top = grid-margin-top（96），下锚点 bottom = 56（chrome-rule 96 - 40 breathing）。
   全锚点 left/right 都吃 grid-margin-x，跟 chrome-rule 共用一条左右基线。 */

.chrome-top-left {
  top: var(--grid-margin-top);
  left: var(--grid-margin-x);
  display: inline-flex;
  align-items: center;
  gap: 18px;
  font-size: 30px;
  font-weight: var(--weight-semibold);
  color: var(--foreground);
  letter-spacing: var(--tracking-tight);
}

.logo-mark {
  width: 44px;
  height: 44px;
  border-radius: 10px;
  background: var(--foreground);
  color: var(--background);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: var(--weight-bold);
  font-size: 26px;
  flex-shrink: 0;
}

.chrome-top-right {
  top: var(--grid-margin-top);
  right: var(--grid-margin-x);
  text-align: right;
  line-height: var(--leading-snug);
}

.chrome-bottom-left {
  bottom: 56px;
  left: var(--grid-margin-x);
}

.chrome-bottom-right {
  bottom: 56px;
  right: var(--grid-margin-x);
  font-variant-numeric: tabular-nums;
}

.chrome-rule {
  position: absolute;
  left:  var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);
  height: 1px;
  margin: 0;
  border: 0;
  background: var(--border);
}

/* ───── h/v 网格定位工具类 ───── */

.h1-2  { grid-column: 1 / 2; }   .h1-3 { grid-column: 1 / 3; }
.h1-4  { grid-column: 1 / 4; }   .h1-5 { grid-column: 1 / 5; }
.h1-6  { grid-column: 1 / 6; }   .h1-7 { grid-column: 1 / 7; }
.h1-8  { grid-column: 1 / 8; }   .h1-9 { grid-column: 1 / 9; }
.h1-12 { grid-column: 1 / 13; }
.h7-12 { grid-column: 7 / 13; }
.h8-12 { grid-column: 8 / 13; }

.v1    { grid-row: 1; }
.v1-2  { grid-row: 1 / 3; }
.v1-3  { grid-row: 1 / 4; }
.v2-3  { grid-row: 2 / 4; }
.v3-4  { grid-row: 3 / 5; }
.v3-5  { grid-row: 3 / 6; }
.v3-8  { grid-row: 3 / 9; }
.v4-5  { grid-row: 4 / 6; }
.v5-7  { grid-row: 5 / 8; }
.v5-8  { grid-row: 5 / 9; }
.v6-7  { grid-row: 6 / 8; }
.v1-8  { grid-row: 1 / 9; }
```

CJK 自动缩字（display 在中文里 120px 太抢镜）：

```css
:root:lang(zh) { --text-4xl: 112px; }
:root:lang(ja) { --text-4xl: 112px; }
:root:lang(ko) { --text-4xl: 112px; }
```

## 选用清单

- **components (shadcn keep)**: card / separator / badge / button / alert
- **components (pitch-specific add)**: pitch-card / pill-highlight (M20) / numeral / contact-list
- **blocks (shadcn 无对应，全部 add)**: hero-block / cards-row / ambient-frames / solution-staggered / feature-grid / stat-split / compare-stagger / chart-card / pricing-3up
- **charts (shadcn keep + adapt)**: chart-line-default（@shadcn/chart-line-default 适配 SVG）

## Page-content 两种 grid 模式

`.page-content` 是 page-shell 内的内容容器，**根据页面类型切换两种 grid**：

| 模式 | 切换 | 特征 | 用法 |
|---|---|---|---|
| **strict 12×8**（默认） | `<div class="page-content">` | 12 列 × 8 行严格 grid，元素必须显式锚 cell（`.h*-* .v*-*`） | **cover / closing**——hero-block 锚 lower-third (`.v6-7`) 或 upper-third (`.v3-8`) |
| **flow** | `<div class="page-content" data-layout="flow">` | 12 列单行 + `align-content: start` + padding 96 上下，元素流式从顶往下堆，间距用 inline `margin-top` | **中间页 P2-P8**——title + body + cards 顺次往下排 |

**为什么两种**：v1 也是双层 grid（外层 strict 给 cover 用，内层 `.content` flow 给中间页用），单 grid 模型不能兼顾"位置感强的 cover"和"流式的中间页"。

## 9 页推荐布局（pitch 9 页节奏）

| # | 名 | variant | block 组合 | grid 模式 |
|---|---|---|---|---|
| P1 | Cover | lite | hero-block (`.h1-9 .v6-7`) + ambient-frames (cover variant) | strict |
| P2 | Problem | dark | cards-row + pitch-card × 3 (num+label) | flow |
| P3 | Solution | forest | solution-staggered (5 卡塔形) + page-content title 含 M20 pill | flow + abs |
| P4 | Feature Grid | lite | feature-grid (5 卡 2x3 错位 icon+label+body) | flow + abs |
| P5 | Stat Split | dark | left h1-6 (title+body) + right h8-12 (stat-split × 2) | flow |
| P6 | Compare Stagger | forest | left h1-6 (title+body) + 4 abs compare-card zig-zag | flow + abs |
| P7 | Chart Narrative | forest | left h1-5 + right h7-12 + abs chart-card (chart-line-default) | flow + abs |
| P8 | Pricing 3-up | lite | flow title+body + abs pricing-row × 3 | flow + abs |
| P9 | Closing | lite | hero-block(--top) (`.h1-9 .v3-8`) + ambient-frames (closing variant) + contact-list | strict |

**Variant 节奏**：`lite → dark → forest → lite → dark → forest → forest → lite → lite`（v1 沉淀，避免相邻同 variant 视觉拖沓）。

## 用法约定（component 在 pitch 主题下的特殊性）

- **pitch-card**：4 slot 但 `num` / `icon` **二选一**——P2 用 num+label，P4 用 icon+label+body。同时给会撑爆 feature-grid 的 240 min-height
- **feature-grid**：内部 `pitch-card` **强制深绿填充**（`#0d2a25`），不论外层 variant——v1 设计意图：lite slide 上的内容块要"实"
- **dark variant + pitch-card**：spec selector override (`[data-variant="dark"] .pitch-card`)，强制 forest green 而非默认 `var(--card)`
- **chrome ring 8 锚点跟随 variant token**——chrome-top-left 文字色 = `var(--foreground)`，自动反白（lite/forest/dark 都对）
- **chrome 跟 slide 解耦**：spec 顶层 `--chrome-fg: #0d2a25` 等是固定浅色（chrome runtime 的 sidebar/topbar 不变色），跟 page-shell 内的 chrome ring 不冲突

## Motif 政策

**白名单**：
- **M07** oversized-numeral（stat 页 88px）
- **M08** hairline-rule（chrome-rule + 卡片描边）
- **M18** geometric-shape（ambient rounded-rect frames）
- **M20** pill-highlight-word（标题强调）

**黑名单**：
- M01 eyebrow-uppercase（chrome 已经提供页面标识，eyebrow 冗余）
- M03 drop-cap（编辑感，错风格）
- M04 italic-accent-word（grotesk 没有合适的斜体）
- M16 color-block（破坏 monochrome）
- M19 image-scrim（破坏几何纯净）

## Anchor-cards 模式

多页（feature-grid / compare-stagger / chart-narrative / pricing-3up）的下半部内容直接钉到 chrome-rule 上方（不让 grid 行间距撑开）：

```css
.anchored-row {
  position: absolute;
  left:  var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: 120px;   /* 96 chrome-rule + 24 breathing；密集网格用 128 */
}
```

## Variant 节奏

9 页配比期望：**4 lite · 2 dark · 3 forest**。

参考序列：`lite → dark → forest → lite → dark → forest → forest → lite → lite`

## Red lines

- **No italic / no serif / no chromatic accent**——`--accent` = `--primary` = `--foreground`，强调靠 M20 pill
- **Destructive variant 在 monochrome 限制下视觉等同 default**——避免使用 `data-variant="destructive"` 的 button / badge。如必需，inline 一个 chromatic 红色（`#ef4444`）覆盖
- **Cards must have ≥ 16 px corner radius**——锐角 = wireframe，错调性
- **No more than one M20 pill per slide**——多 pill 互相竞争，强调失效
- **Chrome on every slide**——cover 也不省略；想省 chrome 就换主题（用 claude / apple）
