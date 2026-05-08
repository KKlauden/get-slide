---
name: archive
description: "Industrial system pitch — file-archive aesthetic，黑白双 variant + grotesk 大字 + mono 系统标签 chrome"
version: 0.1.0
source: "原创设计参考；与 pitch 主题为同级、不同节奏的 sample（异常多 abs 钉位，非 grid-cell）"
---

# Archive Theme

工业系统 pitch deck 主题。**Signature**：

1. **粗 grotesk 大字**——hero 用 800-900 weight 紧字距 grotesk（Geist / Inter），display 字号超大（112-280px）
2. **2 variant 强对比**——`dark`（cover / chapter divider）+ `lite`（contents / 主体）。**纯黑 + 纯白**，不留中间色
3. **Mono 系统标签 chrome**——chrome ring 文字全部 mono，11px，全大写，带流水号气质：`VERSION: V1.0.0 / BUILD_0425` / `INDEX_REF: FILE#0201` / `SYS_MODE: ACTIVE`
4. **Chrome ring 灵活渲染**——8 锚点元素始终存在（chrome runtime 约束），但 `data-chrome-rule="off"` modifier 控制 chrome-rule 显隐；锚点内容空字符串则视觉不占位
5. **Abs 钉位为主**——内容元素直接 abs 钉到 page-shell（不走 page-content grid）。这是跟 pitch 最大的工作流差异
6. **No rounding**——`--radius: 0`，工业感来自锐角

## Token 命名约定

- **Palette 层**完全对齐 [shadcn ui](https://ui.shadcn.com/docs/theming)：`--background / --foreground / --card / --primary / --secondary / --muted / --accent / --destructive / --border` 全套
- **Scale 层**自定义：`--text-* / --space-* / --radius / --weight-* / --leading-* / --tracking-*`
- **不设 component 二级 alias**——CSS 直接 `var(--foreground)` / `var(--text-4xl)`

## Palette / Type / Shape

```css
:root {
  /* ─────────── 非 variant 共享 token ─────────── */
  --canvas-bg: #1a1a1a;            /* dark canvas (chrome runtime 用) */
  --chrome-bg: #0d0d0d;            /* sidebar / topbar 近黑 */

  /* ─────────── typography ─────────── */
  --font-display: "Geist", "Inter", "Helvetica Neue",
                  -apple-system, BlinkMacSystemFont, "Segoe UI",
                  "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei",
                  sans-serif;
  --font-body:    var(--font-display);
  --font-mono:    "Geist Mono", ui-monospace, "JetBrains Mono",
                  "SF Mono", Menlo, monospace;

  --text-xs:        11px;       /* mono system labels */
  --text-sm:        14px;
  --text-md:        18px;
  --text-lg:        24px;
  --text-xl:        32px;       /* contents toc 项 */
  --text-2xl:       48px;
  --text-3xl:       72px;
  --text-4xl:      112px;       /* cover hero */
  --text-mega:     280px;       /* contents super-title */

  --weight-normal:    400;
  --weight-medium:    500;
  --weight-semibold:  600;
  --weight-bold:      700;
  --weight-black:     800;

  --leading-tight:   0.95;
  --leading-snug:    1.0;
  --leading-normal:  1.2;
  --leading-body:    1.5;

  --tracking-tighter: -0.04em;
  --tracking-tight:   -0.02em;
  --tracking-normal:   0;
  --tracking-wide:     0.04em;
  --tracking-mono:     0.08em;   /* mono uppercase labels */

  /* ─────────── shape ─────────── */
  --radius:    0;                /* archive: industrial, no rounding */
  --radius-sm: 0;
  --radius-lg: 0;

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

  /* ─────────── grid (page-shell padding) ─────────── */
  --grid-margin-x:      64px;     /* archive 比 pitch 紧 (96 → 64) */
  --grid-margin-top:    56px;
  --grid-margin-bottom: 56px;

  --shadow-sm: none;
  --shadow:    none;

  /* ─────────── 默认 variant: dark ─────────── */
  --background:             #000000;
  --foreground:             #ffffff;
  --muted:                  rgba(255, 255, 255, 0.06);
  --muted-foreground:       rgba(255, 255, 255, 0.55);
  --border:                 rgba(255, 255, 255, 0.18);
  --card:                   rgba(255, 255, 255, 0.04);
  --card-foreground:        #ffffff;
  --primary:                #ffffff;
  --primary-foreground:     #000000;
  --secondary:              rgba(255, 255, 255, 0.08);
  --secondary-foreground:   #ffffff;
  --accent:                 #ffffff;
  --accent-foreground:      #000000;
  --destructive:            #ef4444;
  --destructive-foreground: #ffffff;

  /* ─────────── chrome runtime tokens ─────────── */
  --chrome-fg:        #ffffff;
  --chrome-muted-fg:  rgba(255, 255, 255, 0.55);
  --chrome-border:    rgba(255, 255, 255, 0.10);
  --chrome-accent:    #ffffff;
  --chrome-accent-fg: #000000;
}

/* ─────────── 显式 variant: dark ─────────── */
[data-variant="dark"] {
  --background:             #000000;
  --foreground:             #ffffff;
  --muted:                  rgba(255, 255, 255, 0.06);
  --muted-foreground:       rgba(255, 255, 255, 0.55);
  --border:                 rgba(255, 255, 255, 0.18);
  --card:                   rgba(255, 255, 255, 0.04);
  --card-foreground:        #ffffff;
  --primary:                #ffffff;
  --primary-foreground:     #000000;
  --secondary:              rgba(255, 255, 255, 0.08);
  --secondary-foreground:   #ffffff;
  --accent:                 #ffffff;
  --accent-foreground:      #000000;
  --destructive:            #ef4444;
  --destructive-foreground: #ffffff;
}

/* ─────────── variant: lite ─────────── */
[data-variant="lite"] {
  --background:             #fafafa;
  --foreground:             #0a0a0a;
  --muted:                  rgba(10, 10, 10, 0.06);
  --muted-foreground:       rgba(10, 10, 10, 0.55);
  --border:                 rgba(10, 10, 10, 0.18);
  --card:                   rgba(10, 10, 10, 0.02);
  --card-foreground:        #0a0a0a;
  --primary:                #0a0a0a;
  --primary-foreground:     #fafafa;
  --secondary:              rgba(10, 10, 10, 0.06);
  --secondary-foreground:   #0a0a0a;
  --accent:                 #0a0a0a;
  --accent-foreground:      #fafafa;
  --destructive:            #ef4444;
  --destructive-foreground: #ffffff;
}

/* ─────────── 全局 base ─────────── */
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

Archive chrome 跟 pitch 完全不同——没有 logo 圆角方块、没有 brand letter，**全 mono 系统标签**。chrome ring 8 锚点元素始终存在（chrome runtime 硬约束），但每个锚点的**内容**由 content.md 决定，可填空字符串。

### Template

```html
<section class="page-shell" data-variant="dark" data-chrome-rule="off">
  <!-- chrome ring 8 锚点：mono 系统标签风格 -->
  <div class="chrome-top-left">PITCH DOCUMENT<br>FAULTLINE ©2025</div>
  <div class="chrome-top-right">VERSION: V1.0.0 / BUILD_0425</div>
  <hr class="chrome-rule" />
  <div class="chrome-bottom-left"></div>
  <div class="chrome-bottom-right"></div>

  <!-- 内容元素：abs 钉位，page-shell 直接子（不走 page-content grid） -->
  <div class="archive-tags">…</div>
  <h1 class="archive-hero">…</h1>
  <div class="archive-ring-motif">…SVG…</div>
</section>
```

`data-chrome-rule="off"` modifier 隐藏 chrome-rule（cover 用）。lite variant 的 contents 页保留 rule。

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

/* dark variant 中间晕染散开渐变（垂直方向，hero band 亮 #2e2e2e，向上下渐入纯黑） */
.page-shell[data-variant="dark"] {
  background: linear-gradient(
    180deg,
    #000000 0%,
    #2e2e2e 50%,
    #000000 100%
  );
}

/* page-content（保留 chrome runtime 约定但 archive 不依赖 grid）：
   archive 内容元素直接 abs 钉 page-shell；page-content 留作未来主体页用 */
.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(8, 1fr);
  column-gap: 24px;
  row-gap: 24px;
  height: 100%;
}

/* ───── 8 锚点 chrome ring ───── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-family: var(--font-mono);
  font-size: var(--text-md);                 /* 18px——跟 archive-tags 同档，统一系统标签字号 */
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--foreground);
  line-height: 1.5;
  z-index: 3;
}
.chrome-top-left {
  top: var(--grid-margin-top);
  left: var(--grid-margin-x);
}
.chrome-top-right {
  top: var(--grid-margin-top);
  right: var(--grid-margin-x);
  text-align: right;
}
.chrome-bottom-left {
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
}
.chrome-bottom-right {
  bottom: var(--grid-margin-bottom);
  right: var(--grid-margin-x);
  text-align: right;
}

/* chrome-rule 默认在 ~60% 高度处（contents 用）；cover 用 [data-chrome-rule="off"] 隐藏 */
.chrome-rule {
  position: absolute;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  top: 720px;       /* 默认接近 contents mega-title 顶部 */
  height: 1px;
  margin: 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 55%, transparent);   /* 跨 dark/lite 自适应 */
  z-index: 2;
}
.page-shell[data-chrome-rule="off"] .chrome-rule { display: none; }
.page-shell[data-chrome-rule="top"] .chrome-rule { top: 140px; }      /* P3 about-page：chrome ring 下方 hairline */
```

## Archive-specific blocks

主题特有的 block，直接钉到 page-shell。**不抽到 `blocks/` 目录**——这些有强 archive identity，跨主题复用价值低。

### archive-tags · cover 三 inline 系统标签

```css
.archive-tags {
  position: absolute;
  top: 50%;                                /* 垂直 slide 中心，对齐 gradient 中心 */
  transform: translateY(-50%);
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: flex;
  gap: 200px;                              /* 大间距强调系统感 */
  font-family: var(--font-mono);
  font-size: var(--text-md);               /* 18px——跟 chrome ring 同档 */
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}
```

### archive-hero · cover 大字 hero（bottom-anchored）

```css
.archive-hero {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 88px;                         /* hero size matched to dribbble ref；改 token 影响其他 sample，此处直写 */
  font-weight: var(--weight-bold);         /* 700——比 black(800) 略轻，跟 dribbble ref 对齐 */
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}
```

### archive-ring-motif · cover 右侧椭圆

SVG nested ellipses + radial gradient 模拟金属光泽。**位置略溢出 page-shell**（right: -80）让视觉上"嵌入"slide 边缘。

```css
.archive-ring-motif {
  position: absolute;
  top: 0;
  right: -80px;
  width: 700px;
  height: 100%;
  z-index: 0;                              /* hero / chrome 全部覆盖 motif */
  pointer-events: none;
}
.archive-ring-motif svg {
  display: block;
  width: 100%;
  height: 100%;
}
```

### archive-toc · contents 9 项 3×3 网格

3 列 × 3 行，**grid-auto-flow: column**——HTML order 1.0~9.0 渲染为 col1=[1,2,3] / col2=[4,5,6] / col3=[7,8,9]。

```css
.archive-toc {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, auto);
  grid-auto-flow: column;                  /* HTML 顺序 1-9 → 视觉 1/4/7 在第一行 */
  align-content: start;                    /* 行高严格按内容，避免默认 stretch 导致间距视觉不一 */
  gap: 64px 64px;                          /* row 跟 col gap 都 64，统一视觉间距 */
  list-style: none;
  margin: 0;
  padding: 0;
  z-index: 2;
}
.archive-toc li {
  display: flex;
  align-items: baseline;
  gap: 64px;
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: var(--weight-semibold);     /* 600——比 bold(700) 略轻 */
  letter-spacing: var(--tracking-tight);
  line-height: 1;                          /* 固定 li height，确保行间距完全均匀 */
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-toc .num {
  font-variant-numeric: tabular-nums;      /* 数字等宽对齐 */
  min-width: 60px;
}
```

### archive-mega-title · contents 超大字标题（bottom-anchored）

```css
.archive-mega-title {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 180px;                        /* 缩小到 180，比 --text-mega(280) 小两档 */
  font-weight: var(--weight-bold);         /* 跟 archive-hero 同 700，统一 hero/mega 字重 */
  line-height: 0.85;
  letter-spacing: var(--tracking-tighter);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}
```

### archive-brand-mark · contents 顶部右 dashed 圆环

简单 dashed 圆环代表 logo / brand identity。

```css
.archive-brand-mark {
  width: 36px;
  height: 36px;
  color: var(--foreground);
}

/* ───── P3 about-page 专属 blocks ───── */

.chrome-top-right.archive-page-meta {
  left: 1100px;                            /* 跨右 ~40% slide */
  display: flex;
  align-items: center;
  justify-content: flex-end;               /* 右对齐打包，brand-mark 顶页右边 */
  gap: 100px;                              /* gap 加大：label+数字整体往左推，避免太靠近右边 */
  text-align: left;
}
.chrome-top-right.archive-page-meta > span:nth-child(2) {
  width: 64px;                             /* 数字列固定宽 */
  text-align: center;                      /* 02/04/09 都居中在 64px 框内，左右边都不漂移 */
}

.archive-page-title {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  display: flex;
  gap: 200px;                              /* ABOUT 跟 FAULTLINE 之间宽间距 */
  font-family: var(--font-display);
  font-size: 88px;                         /* 跟 cover hero 同档 */
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  line-height: 1;
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

.archive-page-number {
  position: absolute;
  top: 280px;
  right: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 1;
  color: var(--foreground);
  z-index: 2;
}

.archive-body {
  position: absolute;
  top: 460px;
  left: var(--grid-margin-x);
  width: 700px;                            /* 18px 字号需要更宽，防 wrap 多行挤压下方 stat */
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  text-align: justify;
  color: var(--foreground);
  z-index: 2;
}

.archive-stat-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 880px;                            /* 留出 image-frame (右侧 880) + 32 gap 空间 */
  z-index: 2;
}
.archive-stat-row {
  display: grid;
  grid-template-columns: 220px 240px 1fr;
  align-items: center;
  gap: 0 32px;
  padding: 32px 0;
  border-top: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-row:last-child {
  border-bottom: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-row .stat-num {
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 1;
  letter-spacing: var(--tracking-tight);
}
.archive-stat-row .stat-label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-semibold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
}
.archive-stat-row .stat-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--foreground);
}

.archive-image-frame {
  position: absolute;
  top: 280px;
  right: var(--grid-margin-x);
  width: 880px;
  bottom: var(--grid-margin-bottom);
  background: var(--muted);                /* 灰色占位 */
  border: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: 18px;
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--muted-foreground);
  z-index: 1;
}

/* ───── P4 problem-page 专属 blocks ───── */

.archive-numbered-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 800px;                            /* 半宽，留右 800 给 metric-panel */
  z-index: 2;
}
.archive-numbered-row {
  display: grid;
  grid-template-columns: 80px 1fr 1fr;     /* num prefix / label / desc */
  align-items: center;
  gap: 0 32px;
  padding: 28px 0;
  border-top: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-numbered-row:last-child {
  border-bottom: 1px solid color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-numbered-row .num {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-semibold);
  color: var(--foreground);
}
.archive-numbered-row .label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-numbered-row .desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}

.archive-metric-panel {
  position: absolute;
  top: 460px;
  right: var(--grid-margin-x);
  width: 800px;
  z-index: 2;
}
.archive-metric-panel .metric-title {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-metric-panel .metric-rule {
  height: 1px;
  margin: 16px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-metric-panel .metric-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--muted-foreground);
}

/* P4 metric-panel 内 bar-chart 顶部留白（block 见 blocks/bar-chart.md） */
.archive-metric-panel .bar-chart {
  margin-top: 80px;
}

.archive-metric-bignum {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  right: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 240px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tighter);
  color: var(--foreground);
  z-index: 2;
}

/* ───── P5 solution-page 专属 blocks ───── */

.archive-layer-list {
  position: absolute;
  top: 480px;
  right: var(--grid-margin-x);
  width: 540px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  z-index: 2;
}
.archive-layer-item .layer-title {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
  text-align: right;                       /* 靠 layer-rule 右端对齐（=chrome-rule 右端） */
}
.archive-layer-item .layer-rule {
  height: 1px;
  margin: 12px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-layer-item .layer-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
  text-align: right;                       /* 跟 layer-title 同侧对齐 */
}

/* ───── P6 performance-data 专属 blocks（solution sub-page 03.1） ───── */

.archive-stat-cards {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  z-index: 2;
}
.archive-stat-card {
  background: color-mix(in srgb, var(--foreground) 4%, transparent);   /* 浅灰底 */
  padding: 56px 56px 48px;
  display: flex;
  flex-direction: column;
  min-height: 360px;
}
.archive-stat-card .card-title {
  font-family: var(--font-display);
  font-size: 32px;                         /* 跟 P2 archive-toc 同档 */
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-stat-card .card-rule {
  height: 1px;
  margin: 24px 0 24px;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-stat-card .card-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}
.archive-stat-card .card-foot {
  margin-top: auto;                        /* num + cap 推到底部 */
  display: flex;
  flex-direction: column;                  /* num 在上，cap 在下 */
  align-items: flex-end;                   /* 整体右靠 */
  gap: 8px;
}
.archive-stat-card .card-num {
  font-family: var(--font-display);
  font-size: 88px;                         /* 跟 page-title 同档 */
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: var(--foreground);
  text-align: right;
}
.archive-stat-card .card-cap {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
  text-align: right;
}

.archive-coverage-panel {
  position: absolute;
  top: 660px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);        /* 跟其他页同 56 底部空隙 */
  background: #0a0a0a;                      /* 强制 dark，与 lite page 形成对比 */
  --foreground: #ffffff;                    /* 反衬：内部 var(--foreground) 子元素全部翻白 —— chart-gauge bars/marker 自动跟随 */
  --muted-foreground: rgba(255, 255, 255, 0.6);
  color: var(--foreground);
  padding: 56px;
  z-index: 2;
}
.archive-coverage-panel .coverage-title {
  font-family: var(--font-display);
  font-size: 32px;                          /* 跟 stat-card title / toc ABOUT 同档 */
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: #ffffff;
}
.archive-coverage-panel .coverage-desc {
  margin-top: 12px;
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.6);
  max-width: 700px;
}
.archive-coverage-panel .coverage-bignum {
  position: absolute;
  top: 56px;
  left: calc(56px + 0.73 * (100% - 112px));   /* 跟 gauge marker 同 x（73% of inner gauge width） */
  transform: translateX(-50%);
  font-family: var(--font-display);
  font-size: 56px;
  font-weight: var(--weight-bold);
  line-height: 1;
  color: #ffffff;
}

/* P6 archive coverage-panel 内 chart-gauge 的主题层 override（block 见 blocks/chart-gauge.md） */
.archive-coverage-panel .chart-gauge {
  position: absolute;
  left: 56px;
  right: 56px;
  bottom: 56px;
}
.archive-coverage-panel .chart-gauge .gauge-labels {
  font-family: var(--font-mono);             /* mono uppercase 是 archive 风格预设 */
  font-size: 18px;
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  color: rgba(255, 255, 255, 0.7);
}

/* ───── P7 system-performance 专属 blocks（performance-page 04.0） ───── */

/* archive-perf-overlay 已弃用（用户决定 P7 不要 image overlay） */

.archive-perf-list {
  position: absolute;
  top: 480px;
  left: 720px;
  right: var(--grid-margin-x);
  display: flex;
  flex-direction: column;
  gap: 64px;
  z-index: 2;
}
.archive-perf-row .perf-title {
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-perf-row .perf-rule {
  height: 1px;
  margin: 24px 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 35%, transparent);
}
.archive-perf-row .perf-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;                     /* 左 body 跟右 perf-stat 垂直居中 */
  gap: 32px;
}
.archive-perf-row .perf-body {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--foreground);
}
.archive-perf-row .perf-stat {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 8px;
}
.archive-perf-row .perf-num {
  font-family: var(--font-display);
  font-size: 120px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: color-mix(in srgb, var(--foreground) 22%, transparent);    /* 浅灰大数字，跟参考图一致 */
}
.archive-perf-row .perf-cap {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  color: var(--muted-foreground);
  text-align: right;
}

/* ───── P8 global-impact 专属 blocks（market-page 05.0） ───── */

/* dark variant 但用底部蓝向上渐变（覆盖默认 dark gradient） */
/* 蓝色集中在底部 ~25%，26% 以上即纯黑（分界点比 P8 v0.1 更下移） */
.page-shell[data-bg-mode="market"] {
  background: linear-gradient(
    0deg,
    #2563eb 0%,                              /* 底：vivid blue */
    #1e40af 7%,                              /* 上 75px：deep blue */
    #15225a 14%,                             /* 上 150px：dark navy */
    #0a0a0a 26%,                             /* 上 280px：纯黑 */
    #0a0a0a 100%
  );
}
/* 顶部 chrome-rule 跟中部 impact-rule 透明度统一降到 30% */
.page-shell[data-bg-mode="market"] .chrome-rule,
.page-shell[data-bg-mode="market"] .archive-impact-rule {
  background: color-mix(in srgb, var(--foreground) 30%, transparent);
}

.archive-mega-stat {
  position: absolute;
  top: 280px;                                /* 顶部对齐 page-title (top: 280) */
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 280px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tighter);
  line-height: 0.85;
  color: var(--foreground);
  z-index: 2;
}

.archive-mega-stat-label {
  position: absolute;
  top: 600px;                                /* 紧跟 $5.5 下方 */
  left: 320px;                               /* 缩进与 $5.5 的"5" 视觉对齐 */
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: var(--weight-bold);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

.archive-impact-rule {
  position: absolute;
  top: 720px;
  left: 360px;                               /* 左端不顶页面边缘，arrow 占左 64-284，中间留 76px 水平 gap */
  right: var(--grid-margin-x);               /* 右端顶到页面右边缘 */
  height: 1px;
  margin: 0;
  border: 0;
  /* base 55%，但 P8 上方 scoped rule 会覆盖到 30% */
  background: color-mix(in srgb, var(--foreground) 55%, transparent);
  z-index: 2;
}

.archive-arrow {
  position: absolute;
  top: 684px;                                /* 中心 36（viewBox y=36）落在 720，跟 rule 同水平面 */
  left: var(--grid-margin-x);
  color: var(--foreground);
  z-index: 2;
}
/* 箭头 SVG 220×72 viewBox（tail 0→215 在 y=36，arrowhead 张开 ~70°: (170,4)→(215,36)→(170,68)） */

.archive-impact-body {
  position: absolute;
  top: 760px;
  right: var(--grid-margin-x);
  width: 700px;
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  color: var(--foreground);
  z-index: 2;
}
.archive-impact-body .muted {
  color: var(--muted-foreground);            /* 第二句淡化，跟参考图一致 */
}

/* ───── P9 global-reach 专属 blocks（market sub-page 05.1） ───── */

.archive-stage-grid {
  position: absolute;
  top: 280px;                                /* 跟左侧 archive-page-title GLOBAL REACH 顶对齐 */
  left: 480px;
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);         /* 底部保留 56 空隙，跟其他页一致 */
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  z-index: 2;
}
.archive-stage-col {
  position: relative;
  border-left: 1px solid color-mix(in srgb, var(--foreground) 30%, transparent);
  padding: 0 32px;                           /* 顶 padding 去掉，stage-num 顶端 = grid 顶端 = y=280 */
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.archive-stage-col .stage-num {
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tight);
  color: color-mix(in srgb, var(--foreground) 25%, transparent);  /* 浅灰大数 */
}
.archive-stage-col .stage-label {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-bold);
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-stage-col .stage-desc {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.4;
  color: var(--muted-foreground);
}
/* P9 stage-col 内 bar-chart 定位 + 默认宽 + tone 调色板（block 见 blocks/bar-chart.md） */
.archive-stage-col .bar-chart {
  position: absolute;
  left: 32px;
  right: 32px;
  bottom: 0;
  height: 480px;                             /* bars zone 固定高 */
  --bar-w: 10px;                             /* 列内 bars 统一 10px 宽 */
}
/* tone variants：从左到右 blue → 浅灰 → 中灰 → 黑（深度递增） */
.archive-stage-col[data-tone="blue"] .bar-chart .bar {
  background: #2563eb;
}
.archive-stage-col[data-tone="muted-light"] .bar-chart .bar {
  background: rgba(10, 10, 10, 0.18);
}
.archive-stage-col[data-tone="muted-mid"] .bar-chart .bar {
  background: rgba(10, 10, 10, 0.4);
}
/* dark 用 bar-chart 默认 var(--bar-color, var(--foreground)) */
```

## 选用清单

- **blocks 引用**:
  - `blocks/bar-chart.md` —— 用于 P4 metric-panel（widths varying）+ P9 stage-cols（heights varying via `--bar-fill`）
  - `blocks/chart-gauge.md` —— 用于 P6 coverage-panel（73% gauge with marker）
- **components (shadcn keep)**: 暂未用
- **archive-specific blocks**（自带视觉风格预设、不抽到 blocks/）：
  - chrome 系：archive-tags / archive-hero / archive-toc / archive-mega-title / archive-brand-mark / archive-page-meta
  - 通用复合：archive-page-title / archive-page-number / archive-body / archive-image-frame
  - 列表 / 卡：archive-stat-list / archive-numbered-list / archive-stat-card / archive-layer-list
  - 复合：archive-metric-panel / archive-coverage-panel / archive-perf-list / archive-stage-grid
  - P8 专属：archive-mega-stat / archive-impact-rule / archive-arrow / archive-impact-body

## 工作流约定（跟 pitch 的差异）

- **Abs 钉位优先**：archive 内容元素直接 abs 钉到 `.page-shell`，**不走** `.page-content` grid。这是跟 pitch（grid placement + abs 混合）最大的差异
- **page-content 保留但常空**：保持 chrome runtime 兼容，但视觉上不占位
- **chrome-rule 可隐**：`data-chrome-rule="off"` 关闭，cover 默认关
- **chrome 内容由 content.md 决定**：每页的 chrome-top-left/-right/-bottom-* 内容字符串都不一样（cover 用 "PITCH DOCUMENT"，contents 用 "INDEX_REF: FILE#0201"），spec 不约束具体字符串

## Motif 政策

**白名单**：
- **M07** mono-numbered-toc（contents 1.0/2.0... 流水号）
- **M08** hairline-rule（chrome-rule + 后续主体页可能用的分隔）
- **M21** metallic-render（cover SVG ellipse motif，限 cover）
- **M22** super-large-display（contents 280px mega title）

**黑名单**：
- M03 drop-cap（不符工业感）
- M04 italic-accent-word（grotesk 800 weight 没有合适 italic）
- M16 color-block（破坏黑白对比）
- M18 ambient-frames（pitch 的 motif，archive 用 ring 替代）
- M20 pill-highlight-word（pitch 的 motif，archive 不需要）

## Red lines

- **No middle gray**——背景只能是 `#000` 或 `#fafafa`，不允许中间灰
- **No serif / no italic / no chromatic accent**（除 destructive 红，仅按需 inline override）
- **Cards / blocks 必须是锐角**——`--radius: 0`，圆角破坏工业感
- **Hero 字号 ≥ 112px**——archive 的"大"是视觉血液，缩了就废
- **Chrome 系统标签必须 mono**——sans 写 chrome 会失去 file-archive 气质
