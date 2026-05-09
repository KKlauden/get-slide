---
name: pitch
description: "现代科技 pitch deck — 单一 grotesk + 三 variant 轮换 + 全 chrome ring + monochrome + 几何装饰"
version: 0.3.0
source: "迁移自 pitch/spec.md（v0.2.0）；按 framework/design.md schema 重写"
---

# Pitch Theme

> YC / TechCrunch 风格的 pitch deck 主题。单字族、单色、节奏感的 variant 轮换，让一份 deck 像一支精剪短片。

## §1 Atmosphere

Pitch deck 是 demo day 的「next big thing」气质：紧凑、现代、自信。整套主题的视觉感来自——大屏幕、强 grid、克制留白；每一寸像素都该说事，不该装点。观众对屏幕的注意力是稀缺品，pitch 主题用「单一字族 + 单一色族 + 轮换的背景 variant」把每一帧都做得像电影分镜里的一格——前后页之间气口分明，但帧内绝不喧宾夺主。

字是这套主题的脊骨。**Cabinet Grotesk** 一族通吃 display + body + nav，连 logo mark 都共用。没有 serif、没有 italic、没有 secondary face。weight 走 normal / semibold / bold 三档跳，靠粗细对比拉视觉层次而非靠字形。CJK fallback 接 PingFang / 微软雅黑，落到中文上 display 自动从 120px 缩到 112px——120 在中文里太抢镜。

色的逻辑是 **monochrome**——`--accent` = `--primary` = `--foreground` 三个变量同值。蓝/红/橙之类的「品牌彩」不存在；强调一律靠 M20 pill highlight 的描边，不靠染色。但 deck 不是单调的：**lite / dark / forest** 三个 variant 在 deck 内轮换（lite 浅米 / dark 纯黑 / forest 深绿 #0d2a25），每页 `data-variant` 切换让 9 页 deck 有「节奏带」——P1 lite 开场、P2 dark 提出问题、P3 forest 解决方案……variant 之间的色温差替代了「彩色 accent」，提供节奏而不打破纯净。

装饰是几何的、永远是几何的。**M18 ambient frames**（圆角矩形 SVG 内嵌于 cover / closing 的背景层）是这套主题的 wordless eyebrow——没有文字、没有渐变、纯线、纯几何，只为页面做空间 gestalt。**M20 pill highlight** 给标题里 1 个关键词加描边胶囊，是整页唯一允许的「染色」（描边色 = 当前 variant 的 `--accent`）。无阴影（`--shadow: none`）、无图片、无渐变、无 drop-cap、无 eyebrow（chrome 已经提供页面标识）——这套主题刻意把所有"偏 editorial"的 motif 列入黑名单。

**Key Characteristics**:
- 单一 Cabinet Grotesk 字族（display + body + nav）
- 3 variant 轮换：lite (浅米) / dark (纯黑) / forest (深绿 #0d2a25)
- 8 锚点全 chrome ring（cover 也不省略）
- monochrome：`--accent` = `--primary` = `--foreground`
- 几何装饰白名单：M07 oversized-numeral / M08 hairline-rule / M18 ambient-frames / M20 pill-highlight-word
- 12×8 strict grid + `data-layout="flow"` 单流模式双轨
- 无阴影、无渐变、无 italic、无 serif、无 chromatic accent

## §2 Palette values

### Background Surfaces
- `--background` — page canvas
  - lite: `#f0f0f0`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`
- `--card` — 卡片底色
  - lite: `#f0f0f0`（与 background 同色，靠描边区分）
  - dark: `rgba(0, 0, 0, 0.30)`（叠黑：深底上更深）
  - forest: `rgba(0, 0, 0, 0.24)`
- `--secondary` — 次级表面
  - lite: `#e6e6e6`
  - dark: `rgba(0, 0, 0, 0.30)`
  - forest: `rgba(0, 0, 0, 0.24)`

### Text & Content
- `--foreground` — primary text
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- `--card-foreground` — 卡片内文字
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- `--secondary-foreground` — 次级表面文字（同上）
- `--muted` — 静音区域底色（rgba 透明，跟 foreground 关联）
  - lite: `rgba(13, 42, 37, 0.06)`
  - dark: `rgba(255, 255, 255, 0.06)`
  - forest: `rgba(255, 255, 255, 0.06)`
- `--muted-foreground` — caption / 弱文字
  - lite: `rgba(13, 42, 37, 0.55)`
  - dark: `rgba(255, 255, 255, 0.55)`
  - forest: `rgba(255, 255, 255, 0.55)`

### Brand & Accent
- `--primary` — 主品牌色（在 monochrome 限制下 = foreground）
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- `--primary-foreground` — primary 上的文字
  - lite: `#f0f0f0`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`
- `--accent` — 强调（同 primary）
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- `--accent-foreground` — accent 上的文字（同 primary-foreground）

### Status
- `--destructive` — 在 monochrome 限制下 = primary，**视觉等同 default**
  - lite: `#0d2a25`
  - dark: `#faf9f5`
  - forest: `#faf9f5`
- `--destructive-foreground`
  - lite: `#ffffff`
  - dark: `#0a0a0a`
  - forest: `#0d2a25`

### Border
- `--border`
  - lite: `rgba(13, 42, 37, 0.20)`
  - dark: `rgba(255, 255, 255, 0.10)`
  - forest: `rgba(255, 255, 255, 0.12)`

### Chrome Runtime（跨 variant 不变）
- `--canvas-bg` — `#dcdcdc`（外层 viewport 底色）
- `--chrome-bg` — `#f0f0f0`（topbar / sidebar 底色）
- `--chrome-fg` — `#0d2a25`
- `--chrome-muted-fg` — `rgba(13, 42, 37, 0.55)`
- `--chrome-border` — `rgba(13, 42, 37, 0.20)`
- `--chrome-accent` — `#0d2a25`
- `--chrome-accent-fg` — `#f0f0f0`

## §3 Typography values

### Font Family
- **Primary (display + body)**: `"Cabinet Grotesk", "Geist", "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`
- **Mono**: `ui-monospace, "JetBrains Mono", "SF Mono", Menlo, monospace`

### Size Scale
- `--text-xs`   `16px`  — caption / 注释
- `--text-sm`   `18px`  — chrome anchor 文字 / 表格内文
- `--text-md`   `22px`  — body / 正文
- `--text-lg`   `32px`  — section heading / 卡片标题
- `--text-xl`   `44px`  — secondary 标题 / lead-in
- `--text-2xl`  `76px`  — feature title
- `--text-3xl`  `88px`  — M07 oversized numeral / stat
- `--text-4xl`  `120px` — cover hero title（CJK 自动 112px）

### Weight Scale
- `--weight-normal`    `400`  — body
- `--weight-medium`    `500`  — caption / chrome anchor
- `--weight-semibold`  `600`  — section heading / brand mark
- `--weight-bold`      `700`  — display / hero title / numeral

### Leading + Tracking
- Leading: `tight 1.05` / `snug 1.15` / `normal 1.4` / `body 1.55`
- Tracking: `tight -0.02em` / `snug -0.015em` / `normal 0` / `wide 0.22em`

### Principles
- **单字族**——所有 hierarchy 由 weight + size 拉开，不引入 serif / italic / secondary face
- **大字 hero 配 tight leading**：`--text-2xl` 以上一律 `--leading-tight (1.05)` + `--tracking-tight (-0.02em)`，让 display 紧凑挤密
- **CJK 自动缩字**：`:root:lang(zh|ja|ko) { --text-4xl: 112px }`，120 在中文里太抢镜
- **数字用 tabular-nums**：page number、stat、表格里的数字都加 `font-variant-numeric: tabular-nums`，避免对齐漂移
- **Tracking-wide 仅用于 chrome / eyebrow-like meta**：正文绝不能 `tracking-wide`（pitch 没 eyebrow，但 chrome 锚点的 ALL CAPS 元数据需要它）

## §4 Shape values

### Radius
- `--radius-sm`     `16px`  — 小卡片 / button / pill 内层
- `--radius`        `20px`  — 默认卡片
- `--radius-full`   `999px` — pill / chip / 圆头按钮

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

### Grid（12 × 8）
- `--grid-columns` `12`
- `--grid-rows` `8`
- `--grid-gutter` `24px`
- `--grid-row-gap` `24px`
- `--grid-margin-x` `96px`
- `--grid-margin-top` `96px`
- `--grid-margin-bottom` `96px`
- `--track-eyebrow` `1 / 4`
- `--track-title` `1 / 9`
- `--track-body` `1 / 9`
- `--track-hero` `1 / 9`
- `--track-stat` `7 / -1`
- `--track-aside` `9 / -1`

### Shadow
- `--shadow-sm` `none`     — pitch 主题不用阴影
- `--shadow`    `none`

## §5 Variants

- **lite** — 浅米 (`#f0f0f0`) 底，深绿 (`#0d2a25`) 文字。**默认 / cover 首选**——干净开场。3 variant 中视觉最轻，给 P1 cover / P4 feature-grid / P8 pricing / P9 closing 用
- **dark** — 纯黑 (`#0a0a0a`) 底，米白 (`#faf9f5`) 文字。**问题 / 强情绪页首选**——P2 problem 用 dark 强化"痛点凝重"，P5 stat-split 也用 dark 让数字更冲击
- **forest** — 深绿 (`#0d2a25`) 底，米白文字。**solution / 数据沉淀页首选**——P3 solution / P6 compare / P7 chart 都用 forest，让"答案 / 证据"系出现在沉稳底上

### 多变体设计原则
- 期望 9 页配比：**4 lite · 2 dark · 3 forest**
- 参考序列：`lite → dark → forest → lite → dark → forest → forest → lite → lite`（v1 沉淀，避免相邻同 variant 视觉拖沓）
- variant 之间的色温差替代了"彩色 accent"——pitch monochrome 限制下，节奏感来源于 variant 切换而非 hue 切换

## §6 Decorative signature

### Chrome Ring (8 锚点)
- **是什么**：每页都有的 deck 印章——top-left logo + brand / top-right copyright / chrome-rule (`<hr>` 一条 hairline) / bottom-left brand / bottom-right page number
- **几何**：4 个 div 锚 4 角 + 1 条 hr 横线压在底边上方 96px 处。logo-mark 是 44×44 圆角 10 的方块，里面塞 1 个字母（deck 首字母大写）
- **用法**：cover 也不省略——pitch 主题刻意保留全 chrome 让"slide deck 的工业感"持续存在；想省 chrome 就换主题（用 claude / apple）
- **HTML 模板**（每页 page-shell 内）：

```html
<div class="chrome-top-left">
  <span class="logo-mark">P</span>
  <strong>Brand.</strong>
</div>
<div class="chrome-top-right">© Brand, Inc<br>All Rights Reserved</div>
<hr class="chrome-rule" />
<div class="chrome-bottom-left">Brand.</div>
<div class="chrome-bottom-right">01</div>
```

### M18 Ambient Frames
- **是什么**：cover / closing 页背景层的圆角矩形 SVG 嵌套，做空间 gestalt
- **几何**：3-5 层 stroke-only 圆角矩形（无 fill），各层不同尺寸 + 不同 stroke-width，叠加形成"画框感"
- **用法**：仅 cover (P1) + closing (P9) 用；中间页禁用——会跟 grid 内容冲突
- **HTML 模板**：见 `samples/pitch/pitch.html` § COMPONENT CSS 段（搜索 `from blocks/ambient-frames.md` comment）

### M20 Pill Highlight
- **是什么**：标题里给 1 个关键词加 stroke-only 椭圆胶囊
- **几何**：`border: 1.5px solid var(--accent)` + `border-radius: 999px` + `padding: 0 0.4em` + `display: inline-block`，行内不换行
- **用法**：每页**最多 1 个** pill；多 pill 互相竞争，强调失效
- **HTML 模板**：见 `samples/pitch/pitch.html` § COMPONENT CSS 段（搜索 `from components/pill-highlight.md` comment）

### Component-level bend：dark variant + pitch-card
- **是什么**：`pitch-card` 在 dark variant 下不用默认的 `var(--card)` (rgba 叠黑半透明)，而是用 forest green 实色块 (`#0d2a25`)
- **理由**：黑底上要 solid block 不是半透明叠层——半透明会看起来"漂"，实色块更"扎"
- **写法**：design.md §8 Append CSS 段单独 selector override

## §7 Red Lines
- **No italic / no serif / no chromatic accent** — `--accent` = `--primary` = `--foreground`，强调靠 M20 pill 不靠染色
- **Destructive variant 视觉等同 default** — 避免使用 `data-variant="destructive"` 的 button / badge；如必需，inline 一个 chromatic 红色 (`#ef4444`) 覆盖
- **Cards must have ≥ 16px corner radius** — 锐角 = wireframe 调性，错风格
- **No more than one M20 pill per slide** — 多 pill 互相竞争，强调失效
- **Chrome on every slide** — cover 也不省略；想省 chrome 就换主题
- **No M01 eyebrow / M03 drop-cap / M04 italic-accent / M16 color-block / M19 image-scrim** — pitch 主题装饰白名单只有 M07 / M08 / M18 / M20，其余 motif 一律黑名单
- **No shadows** — `--shadow: none`；卡片靠 border 划分

## §8 Append CSS

```css
/* ─────────── 1. Chrome 8 锚点位置 ─────────── */

.chrome-top-left,
.chrome-top-right,
.chrome-bottom-left,
.chrome-bottom-right {
  position: absolute;
  font-size: var(--text-sm);
  color: var(--muted-foreground);
  z-index: 1;
}

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

/* ─────────── 2. .page-shell 内边距 ─────────── */

.page-shell {
  position: relative;
  width: 1920px;
  height: 1080px;
  background: var(--background);
  color: var(--foreground);
  padding: var(--grid-margin-top) var(--grid-margin-x) var(--grid-margin-bottom);
  overflow: hidden;
}

/* ─────────── 3. .page-content 容器 + 双 grid 模式 ─────────── */

.page-content {
  position: relative;
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  grid-template-rows:    repeat(var(--grid-rows), 1fr);
  column-gap: var(--grid-gutter);
  row-gap:    var(--grid-row-gap);
  height: 100%;
}

/* flow 模式：12 列单行 + 顶部对齐，元素流式从上往下堆。
   切换：<div class="page-content" data-layout="flow">
   适用：title + body + cards 顺次往下排的中间页 (P2-P8)
   元素只锚 column (.h*-*)，不锚 row；间距用 inline margin-top 控制。
   page-content 加 96 上下 padding 给内容跟 chrome-top/bottom 留 buffer
   （叠加 page-shell padding 96 = 总 192 from edge）。 */
.page-content[data-layout="flow"] {
  grid-template-rows: none;
  grid-auto-rows: auto;
  align-content: start;
  padding-top:    var(--grid-margin-top);
  padding-bottom: var(--grid-margin-bottom);
  row-gap: 0;
}

/* ─────────── 4. Utility classes：12×8 grid 锚点 ─────────── */

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

/* anchored-row：多页 (feature-grid / compare / chart / pricing) 下半部
   钉到 chrome-rule 上方的浮动行 */
.anchored-row {
  position: absolute;
  left:  var(--grid-margin-x);
  right: var(--grid-margin-x);
  bottom: 120px;   /* 96 chrome-rule + 24 breathing；密集网格用 128 */
}

/* ─────────── 5. Component-level overrides ─────────── */

/* P2 problem-numbered：dark variant 下 pitch-card 用 forest green solid
   block，不用默认的叠黑半透明。设计意图：黑底上要 solid block 才"扎"。 */
[data-variant="dark"] .pitch-card {
  background: #0d2a25;
  border-color: transparent;
}

/* feature-grid 内部 pitch-card 强制深绿填充——不论外层 variant。
   设计意图：lite slide 上的内容块要"实"。 */
.feature-grid .pitch-card {
  background: #0d2a25;
  color: #faf9f5;
  border-color: transparent;
}

/* ─────────── 6. CJK 自动缩字 ─────────── */

:root:lang(zh) { --text-4xl: 112px; }
:root:lang(ja) { --text-4xl: 112px; }
:root:lang(ko) { --text-4xl: 112px; }
```
