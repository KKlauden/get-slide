---
name: archive
description: "Industrial system pitch — file-archive aesthetic，黑白双 variant + grotesk 大字 + mono 系统标签 chrome + 锐角 + abs 钉位"
version: 0.2.0
source: "迁移自 archive/spec.md（v0.1.0）；按 framework/design.md schema 重写。9 页 FAULTLINE 地质监测虚构 deck"
---

# Archive Theme

> 工业系统 pitch deck 主题。文件归档美学：纯黑 + 纯白、流水号、mono 标签、超大字 hero。每一页像一份带版本号的工程档案。

## §1 Atmosphere

Archive 主题的画面感来自工程档案室——黑白通透、零圆角、流水号编号、机器打印体。整体不"现代"，更像一份带 BUILD 号的内部白皮书：cover 是封面页（dark gradient + 大字 hero + 系统标签），contents 是目录页（lite 底 + 9 项 3×3 流水号网格 + 280px mega title），主体页是档案条目（lite 底 + abs 钉位的数字 / list / chart）。它的"工业感"不是来自图标或纹理，而是**纯几何 + 字号差 + 流水号秩序感**——一切元素都在 mono 标签的系统语境里被编码。

字是这套主题的骨。**Geist** 通吃 display + body，weight 走到 800 black，hero 字号最大 280px（contents super-title），中等档常驻 88px / 112px。display 与 mono 共存：display 用于大字 hero / 标题 / list label，mono（Geist Mono）专门用于 chrome 锚点 / 系统标签 / archive-tags 这类「元数据」位置——刻意造出"系统在说话 vs 设计师在说话"的两套声调。CJK fallback 接 PingFang，中文里 hero 维持 88-112px（mega 280 不缩，因为 CJK 字密足以扛住）。

色的逻辑是**纯黑 + 纯白二选一**——`#000` / `#fafafa` 锁死。中间灰一律 `color-mix(in srgb, var(--foreground) NN%, transparent)` 派生（35% 是分隔线、55% 是 chrome-rule、25% 是浅灰大数字）。两个 variant 担纲完全不同的角色：**dark** 给 cover / chapter divider 用，配 `linear-gradient(180deg, #000 0%, #2e2e2e 50%, #000 100%)` 中部晕染，hero 字浮在亮带上；**lite** 给 contents 和所有主体页用，纯 `#fafafa` 底，hero / 大字 / list 全部 `#0a0a0a` 实色。例外只有两处：destructive 用 `#ef4444` 红（仅按需 inline override），P8 market-page 用 `data-bg-mode="market"` 切到底部蓝向上渐变（`#2563eb` 到 `#0a0a0a`）。

装饰是几何 + 流水号 + 系统标签——**Chrome ring 全 mono uppercase 18px**（`PITCH DOCUMENT / VERSION: V1.0.0 / BUILD_0425 / INDEX_REF: FILE#0201`），8 锚点元素始终存在但内容由 content.md 每页决定，可填空。**Chrome-rule 三档模式**：`data-chrome-rule="off"`（cover 关闭）/ default（page-shell 720px 处，contents 用）/ `="top"`（140px，P3 about 用）。**archive-ring-motif**（cover 右侧 SVG 嵌套椭圆 + radial gradient 模拟金属光泽）+ **archive-mega-title**（contents 180-280px super-display）+ **archive-toc**（3×3 grid 流水号 1.0–9.0）是 archive 三大签名 motif。所有内容元素**直接 abs 钉到 `.page-shell`**——**不走** `.page-content` grid——这是 archive 跟 pitch 工作流上最大的差异：每页的具体定位都靠 archive-* class 的 absolute top/left/right/bottom 锚点。

**Key Characteristics**:
- 单一 Geist 字族 + Geist Mono 双轨（display + system labels）
- 2 variant 强对比：dark（cover / divider 配中部晕染 gradient）+ lite（contents / 主体页）
- 8 锚点 chrome ring 全 mono uppercase 18px，3 档 chrome-rule 显隐
- 纯黑 + 纯白锁死，中间灰由 `color-mix` 派生
- 锐角：`--radius: 0`（工业感来自零圆角）
- Abs 钉位为主：内容元素直接 absolute 到 page-shell，**不走** grid-cell
- 大字 hero ≥ 112px，contents super-title 180-280px
- Motif 白名单：M07 mono-numbered-toc / M08 hairline-rule / M21 metallic-render / M22 super-large-display

## §2 Palette values

### Background Surfaces
- `--background` — page canvas
  - dark: `#000000`
  - lite: `#fafafa`
- `--card` — 卡片底色（极浅叠透明）
  - dark: `rgba(255, 255, 255, 0.04)`
  - lite: `rgba(10, 10, 10, 0.02)`
- `--secondary` — 次级表面
  - dark: `rgba(255, 255, 255, 0.08)`
  - lite: `rgba(10, 10, 10, 0.06)`

### Text & Content
- `--foreground` — primary text
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- `--card-foreground` — 同 foreground
- `--secondary-foreground` — 同 foreground
- `--muted` — 静音区域底色
  - dark: `rgba(255, 255, 255, 0.06)`
  - lite: `rgba(10, 10, 10, 0.06)`
- `--muted-foreground` — caption / 弱文字 / 浅灰大数字
  - dark: `rgba(255, 255, 255, 0.55)`
  - lite: `rgba(10, 10, 10, 0.55)`

### Brand & Accent
- `--primary` — 主品牌色（黑白限制下 = foreground）
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- `--primary-foreground`
  - dark: `#000000`
  - lite: `#fafafa`
- `--accent` — 强调（同 primary）
  - dark: `#ffffff`
  - lite: `#0a0a0a`
- `--accent-foreground`（同 primary-foreground）

### Status
- `--destructive` — `#ef4444`（**两 variant 共用**——错误 / 警告通用工程红，跨 dark/lite 不变）
- `--destructive-foreground` — `#ffffff`

### Border
- `--border`
  - dark: `rgba(255, 255, 255, 0.18)`
  - lite: `rgba(10, 10, 10, 0.18)`

### Chrome Runtime（跨 variant 不变）
- `--canvas-bg` — `#1a1a1a`（外层 viewport 暗灰）
- `--chrome-bg` — `#0d0d0d`（topbar / sidebar 近黑）
- `--chrome-fg` — `#ffffff`
- `--chrome-muted-fg` — `rgba(255, 255, 255, 0.55)`
- `--chrome-border` — `rgba(255, 255, 255, 0.10)`
- `--chrome-accent` — `#ffffff`
- `--chrome-accent-fg` — `#000000`

## §3 Typography values

### Font Family
- **Primary (display + body)**: `"Geist", "Inter", "Helvetica Neue", -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`
- **Mono**: `"Geist Mono", ui-monospace, "JetBrains Mono", "SF Mono", Menlo, monospace`

### Size Scale
- `--text-xs`   `11px`  — 极弱注释（保留给 micro-print 场景，archive 主样本目前未用）
- `--text-sm`   `14px`  — 表格内文 / 极小说明
- `--text-md`   `18px`  — body / chrome anchor / mono system labels（**主导字号**）
- `--text-lg`   `24px`  — caption / 中级标题
- `--text-xl`   `32px`  — contents toc 项 / stat-card title / 中级 heading
- `--text-2xl`  `48px`  — 中级 hero / 跨页 lead-in
- `--text-3xl`  `72px`  — page-title 第二档
- `--text-4xl`  `112px` — cover archive-hero / mega body 数
- `--text-mega` `280px` — contents super-title / P8 mega-stat

### Weight Scale
- `--weight-normal`    `400` — body / desc
- `--weight-medium`    `500` — chrome anchor / mono labels（系统标签默认）
- `--weight-semibold`  `600` — toc 项
- `--weight-bold`      `700` — 大字 hero / page-title / list label / stat-num（**主导 weight**）
- `--weight-black`     `800` — 仅极重 hero 备用（archive 主样本目前未用）

### Leading + Tracking
- Leading: `tight 0.95` / `snug 1.0` / `normal 1.2` / `body 1.5`（注意：archive `tight` 比 pitch `tight` 更紧）
- Tracking: `tighter -0.04em` / `tight -0.02em` / `normal 0` / `wide 0.04em` / `mono 0.08em`

### Principles
- **大字一律 tight tracking + tight leading**：display ≥ 88px 必须 `letter-spacing: tracking-tight (-0.02em)` + `line-height: 0.85` 或 `tight 0.95`，否则 archive 大字会"软"
- **数字用 tabular-nums**：page number / stat-num / toc num 强制 `font-variant-numeric: tabular-nums` 防对齐漂移
- **Mono 专用于系统语义**：chrome anchor / archive-tags / coverage-panel labels —— 凡是「元数据 / 编号 / 系统状态」一律 mono uppercase + `tracking-mono (0.08em)`
- **Display + Mono 不能混用**：标题 / hero 永远 display；mono 永远只做标签 / 编号
- **CJK 不缩 mega**：`--text-mega` 在中文里也保持 280px——CJK 字本身视觉密度高，撑得起

## §4 Shape values

### Radius
- `--radius-sm`  `0`  — 锐角
- `--radius`     `0`  — 锐角（archive 工业感来自零圆角）
- `--radius-lg`  `0`  — 锐角

### Space
- `--space-1`    `4px`
- `--space-2`    `8px`
- `--space-3`   `12px`
- `--space-4`   `16px`
- `--space-6`   `24px`
- `--space-8`   `32px`
- `--space-12`  `48px`
- `--space-16`  `64px`
- `--space-24`  `96px`
- `--space-32` `128px`

### Grid（page-shell padding only — archive 不用 page-content grid）
- `--grid-margin-x`      `64px`  — 比 pitch (96) 紧
- `--grid-margin-top`    `56px`
- `--grid-margin-bottom` `56px`

> archive 内容元素直接 abs 钉到 `.page-shell`，**不走** `.page-content` 12-col grid。grid token 此处仅给 `.page-shell` 内边距用。

### Shadow
- `--shadow-sm` `none`   — archive 主题不用阴影
- `--shadow`    `none`

## §5 Variants

- **dark** — 纯黑 (`#000000`) 底，纯白 (`#ffffff`) 文字。**cover / chapter divider / 强情绪页首选**——P1 cover 用 dark + 中部晕染 gradient（`linear-gradient(180deg, #000 0%, #2e2e2e 50%, #000 100%)`），hero 字浮在亮带；P8 market-page 也用 dark 但叠 `data-bg-mode="market"` 底部蓝渐变
- **lite** — 浅米 (`#fafafa`) 底，深黑 (`#0a0a0a`) 文字。**contents / 所有主体页（P2-P9 中除 P8 外）默认**——纯实色不渐变，让大字 hero 跟 archive-* abs 内容元素清晰呈现

### 多变体设计原则
- 期望 9 页配比：**3 dark · 6 lite**（cover + chapter divider + market 用 dark 提情绪节奏；contents 与所有主体页用 lite 保稳读）
- 参考序列：`dark → lite → lite → lite → lite → lite → lite → dark → lite`（archive sample 实际节奏；P1 cover dark / P8 market dark / 其余 lite）
- 不允许中间灰 variant（如 #1a1a1a 或 #888 底）——archive 视觉血液是黑白对比，过渡灰会让设计软掉

## §6 Decorative signature

### Chrome Ring (8 锚点) — Mono 系统标签
- **是什么**：每页 8 锚点元素始终存在（chrome runtime 硬约束），但内容由 content.md 每页决定，可填空字符串
- **几何**：4 个 div 锚 4 角 + 1 条 hr（chrome-rule）。4 角 div 全 mono uppercase 18px medium weight，颜色 `var(--foreground)`。锚点之间靠空字符串实现「视觉省略」
- **3 档 chrome-rule 模式**：
  - `data-chrome-rule="off"` — 关闭（cover 默认）
  - 默认（无 modifier）— 在 page-shell 720px 处（contents 用，紧贴 mega-title 顶部）
  - `data-chrome-rule="top"` — 在 140px（P3 about-page 用，chrome ring 下方 hairline）
- **示例字符串**：
  - cover: `PITCH DOCUMENT / FAULTLINE ©2025` (top-left), `VERSION: V1.0.0 / BUILD_0425` (top-right)
  - contents: `INDEX_REF: FILE#0201` (top-left), `SYS_MODE: ACTIVE` (top-right)
  - 主体页: 同 contents 模式 + bottom-right 流水号 `02 / 04 / 09`
- **HTML 模板**（每页 page-shell 内）：

```html
<div class="chrome-top-left">PITCH DOCUMENT<br>FAULTLINE ©2025</div>
<div class="chrome-top-right">VERSION: V1.0.0 / BUILD_0425</div>
<hr class="chrome-rule" />
<div class="chrome-bottom-left"></div>
<div class="chrome-bottom-right"></div>
```

### M21 Metallic Ring Motif (cover 专属)
- **是什么**：cover 右侧悬浮的 SVG 嵌套椭圆 + radial gradient，模拟金属光泽
- **几何**：3-5 层椭圆嵌套，外层最大、内层最小；每层 `stroke-only`、不同 stroke-width；最内层叠 `radial-gradient` 给金属反光感。位置略溢出 page-shell（`right: -80px`）让视觉上"嵌入"slide 边缘
- **用法**：仅 cover (P1) 用；其他页一律不用——这是 archive cover 唯一的"图形"装饰
- **HTML 模板**：见 archive sample P1 的 `.archive-ring-motif > svg`

### M22 Super-Large Display (contents super-title)
- **是什么**：contents 页底部的 180-280px 大字标题（如 "DESIGN ARCHIVE / FAULTLINE"），bottom-anchored
- **几何**：`font-size: 180px` 或 `280px`，`line-height: 0.85`，`letter-spacing: tighter (-0.04em)`，`text-transform: uppercase`，`font-weight: bold (700)`
- **用法**：contents (P2) 用 180px；P8 market-page 用 280px（archive-mega-stat）

### M07 Mono Numbered TOC (contents 流水号)
- **是什么**：contents 页 3×3 grid 中的 9 项目录，每项前缀 `1.0 / 2.0 / ... / 9.0` 流水号 + 大写 label
- **几何**：`grid-template-columns: repeat(3, 1fr)` + `grid-auto-flow: column`（HTML 顺序 1-9 → 视觉 1/4/7 第一行），`gap: 64px 64px`，每项 `display: flex; align-items: baseline; gap: 64px`，num `min-width: 60px` + tabular-nums

### Component-level bend：abs 钉位为主
- **是什么**：archive 内容元素**全部** abs 钉到 page-shell（不走 page-content grid）。这是跟 pitch 最大的差异
- **理由**：archive 每页布局都不一样、每页都是 bespoke——强 grid 反而是束缚。abs + 数字坐标更直接
- **写法**：design.md §8 Append CSS 段定义 ~30 个 archive-* selector，每个声明自己的 `position: absolute; top/left/right/bottom`

## §7 Red Lines
- **No middle gray** — 背景只能是 `#000` 或 `#fafafa`，不允许中间灰；中间灰必须靠 `color-mix(in srgb, var(--foreground) NN%, transparent)` 派生
- **No serif / no italic / no chromatic accent** — 除 destructive 红和 P8 market 蓝渐变外，颜色锁死黑白
- **Cards / blocks 必须锐角** — `--radius: 0`；圆角破坏工业档案气质
- **Hero 字号 ≥ 112px** — archive "大"是视觉血液，缩了就废
- **Chrome 系统标签必须 mono** — sans 写 chrome 会失去 file-archive 气质
- **No M03 drop-cap / M04 italic-accent / M16 color-block / M18 ambient-frames / M20 pill-highlight** — pitch 的 motif (ambient-frames / pill) 在 archive 一律黑名单
- **不准用 `.page-content` grid** — archive 内容元素一律 abs 钉位；想用 grid 就换 pitch 主题

## §8 Append CSS

```css
/* ─────────── 1. Chrome 8 锚点位置 ─────────── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-family: var(--font-mono);
  font-size: var(--text-md);                    /* 18px——跟 archive-tags 同档，统一系统标签字号 */
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

/* ─────────── 2. .page-shell 内边距 + dark variant gradient ─────────── */

.page-shell {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: var(--background);
  color: var(--foreground);
  padding: var(--grid-margin-top) var(--grid-margin-x) var(--grid-margin-bottom);
  overflow: hidden;
}

/* dark variant 中部晕染散开渐变（垂直方向，hero band 亮 #2e2e2e，向上下渐入纯黑） */
.page-shell[data-variant="dark"] {
  background: linear-gradient(
    180deg,
    #000000 0%,
    #2e2e2e 50%,
    #000000 100%
  );
}

/* ─────────── 3. .page-content 容器（保留兼容但 archive 不依赖 grid） ─────────── */

.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(8, 1fr);
  column-gap: 24px;
  row-gap: 24px;
  height: 100%;
}

/* ─────────── 5. archive-specific blocks（abs 钉位主体）─────────── */

/* P1 cover 三 inline 系统标签 */
.archive-tags {
  position: absolute;
  top: 50%;                                 /* 垂直 slide 中心，对齐 gradient 中心 */
  transform: translateY(-50%);
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: flex;
  gap: 200px;                               /* 大间距强调系统感 */
  font-family: var(--font-mono);
  font-size: var(--text-md);                /* 18px——跟 chrome ring 同档 */
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P1 cover 大字 hero (bottom-anchored) */
.archive-hero {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 88px;                          /* matched to dribbble ref */
  font-weight: var(--weight-bold);          /* 700——比 black(800) 略轻 */
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P1 cover 右侧椭圆 motif (溢出 right: -80) */
.archive-ring-motif {
  position: absolute;
  top: 0;
  right: -80px;
  width: 700px;
  height: 100%;
  z-index: 0;                               /* hero / chrome 全部覆盖 motif */
  pointer-events: none;
}
.archive-ring-motif svg {
  display: block;
  width: 100%;
  height: 100%;
}

/* P2 contents 9 项 3×3 grid */
.archive-toc {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  right: var(--grid-margin-x);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, auto);
  grid-auto-flow: column;                   /* HTML 顺序 1-9 → 视觉 1/4/7 第一行 */
  align-content: start;                     /* 行高严格按内容 */
  gap: 64px 64px;                           /* row 跟 col gap 都 64 */
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
  font-size: var(--text-xl);                /* 32px */
  font-weight: var(--weight-semibold);      /* 600 */
  letter-spacing: var(--tracking-tight);
  line-height: 1;
  text-transform: uppercase;
  color: var(--foreground);
}
.archive-toc .num {
  font-variant-numeric: tabular-nums;
  min-width: 60px;
}

/* P2 contents super-title (bottom-anchored, 180px) */
.archive-mega-title {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  font-family: var(--font-display);
  font-size: 180px;                         /* 比 --text-mega(280) 小两档 */
  font-weight: var(--weight-bold);
  line-height: 0.85;
  letter-spacing: var(--tracking-tighter);
  text-transform: uppercase;
  color: var(--foreground);
  z-index: 2;
}

/* P2 contents 顶部右 dashed 圆环 */
.archive-brand-mark {
  width: 36px;
  height: 36px;
  color: var(--foreground);
}

/* ─── P3 about-page (主体页通用 page-title/body) ─── */

/* P3 chrome-top-right 横幅式 page-meta（变体 chrome-top-right + .archive-page-meta） */
.chrome-top-right.archive-page-meta {
  left: 1100px;                             /* 跨右 ~40% slide */
  display: flex;
  align-items: center;
  justify-content: flex-end;                /* 右对齐打包，brand-mark 顶页右边 */
  gap: 100px;                               /* 整体往左推，避免太靠近右边 */
  text-align: left;
}
.chrome-top-right.archive-page-meta > span:nth-child(2) {
  width: 64px;                              /* 数字列固定宽 */
  text-align: center;                       /* 02/04/09 都居中在 64px 框内 */
}

.archive-page-title {
  position: absolute;
  top: 280px;
  left: var(--grid-margin-x);
  display: flex;
  gap: 200px;                               /* ABOUT 跟 FAULTLINE 之间宽间距 */
  font-family: var(--font-display);
  font-size: 88px;                          /* 跟 cover hero 同档 */
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
  width: 700px;                             /* 18px 字号需要更宽 */
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  line-height: 1.5;
  text-align: justify;
  color: var(--foreground);
  z-index: 2;
}

/* P3 archive-stat-list 3 行带 hairline 分割（KPI 数字列表） */
.archive-stat-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 880px;                             /* 留出 image-frame (右侧 880) + 32 gap */
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

/* P3 archive-image-frame 灰色占位框 */
.archive-image-frame {
  position: absolute;
  top: 280px;
  right: var(--grid-margin-x);
  width: 880px;
  bottom: var(--grid-margin-bottom);
  background: var(--muted);
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

/* ─── P4 problem-page ─── */

.archive-numbered-list {
  position: absolute;
  bottom: var(--grid-margin-bottom);
  left: var(--grid-margin-x);
  width: 800px;
  z-index: 2;
}
.archive-numbered-row {
  display: grid;
  grid-template-columns: 80px 1fr 1fr;      /* num prefix / label / desc */
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
.archive-metric-panel .bar-chart {
  margin-top: 80px;                         /* metric-panel 内 bar-chart 顶部留白 */
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

/* ─── P5 solution-page (右侧 layer-list) ─── */

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
  text-align: right;                        /* 靠 layer-rule 右端对齐（=chrome-rule 右端） */
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
  text-align: right;                        /* 跟 layer-title 同侧对齐 */
}

/* ─── P6 performance-data (3 stat cards + dark coverage panel) ─── */

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
  font-size: 32px;                          /* 跟 P2 archive-toc 同档 */
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
  margin-top: auto;                         /* num + cap 推到底部 */
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 8px;
}
.archive-stat-card .card-num {
  font-family: var(--font-display);
  font-size: 88px;                          /* 跟 page-title 同档 */
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
  --foreground: #ffffff;                    /* 反衬：内部 var(--foreground) 子元素全部翻白 */
  --muted-foreground: rgba(255, 255, 255, 0.6);
  color: var(--foreground);
  padding: 56px;
  z-index: 2;
}
.archive-coverage-panel .coverage-title {
  font-family: var(--font-display);
  font-size: 32px;
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
  left: calc(56px + 0.73 * (100% - 112px)); /* 跟 gauge marker 同 x（73% of inner gauge width） */
  transform: translateX(-50%);
  font-family: var(--font-display);
  font-size: 56px;
  font-weight: var(--weight-bold);
  line-height: 1;
  color: #ffffff;
}
.archive-coverage-panel .chart-gauge {
  position: absolute;
  left: 56px;
  right: 56px;
  bottom: 56px;
}
.archive-coverage-panel .chart-gauge .gauge-labels {
  font-family: var(--font-mono);
  font-size: 18px;
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-mono);
  color: rgba(255, 255, 255, 0.7);
}

/* ─── P7 system-performance ─── */

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
  align-items: center;
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
  color: color-mix(in srgb, var(--foreground) 22%, transparent);    /* 浅灰大数字 */
}
.archive-perf-row .perf-cap {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: var(--weight-normal);
  color: var(--muted-foreground);
  text-align: right;
}

/* ─── P8 global-impact (market-page，dark + 底部蓝渐变) ─── */

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
.page-shell[data-bg-mode="market"] .chrome-rule,
.page-shell[data-bg-mode="market"] .archive-impact-rule {
  background: color-mix(in srgb, var(--foreground) 30%, transparent);
}

.archive-mega-stat {
  position: absolute;
  top: 280px;                               /* 顶部对齐 page-title (top: 280) */
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
  top: 600px;
  left: 320px;                              /* 缩进与 $5.5 的"5" 视觉对齐 */
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
  left: 360px;                              /* 左端不顶页边缘，arrow 占左 64-284 */
  right: var(--grid-margin-x);
  height: 1px;
  margin: 0;
  border: 0;
  background: color-mix(in srgb, var(--foreground) 55%, transparent);
  z-index: 2;
}
.archive-arrow {
  position: absolute;
  top: 684px;                               /* 中心 36（viewBox y=36）落在 720 */
  left: var(--grid-margin-x);
  color: var(--foreground);
  z-index: 2;
}
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
  color: var(--muted-foreground);
}

/* ─── P9 global-reach (4 stage cols with bar-chart) ─── */

.archive-stage-grid {
  position: absolute;
  top: 280px;                               /* 跟左侧 archive-page-title GLOBAL REACH 顶对齐 */
  left: 480px;
  right: var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  z-index: 2;
}
.archive-stage-col {
  position: relative;
  border-left: 1px solid color-mix(in srgb, var(--foreground) 30%, transparent);
  padding: 0 32px;                          /* 顶 padding 去掉 */
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
  color: color-mix(in srgb, var(--foreground) 25%, transparent);    /* 浅灰大数 */
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
.archive-stage-col .bar-chart {
  position: absolute;
  left: 32px;
  right: 32px;
  bottom: 0;
  height: 480px;                            /* bars zone 固定高 */
  --bar-w: 10px;                            /* 列内 bars 统一 10px 宽 */
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
/* dark stage 用 bar-chart 默认 var(--bar-color, var(--foreground)) */
```
