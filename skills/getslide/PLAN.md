# getslide v2 重建计划

> 状态：架构定型中。v1 已归档到 `old/`，根目录是空白起步。
> 本文是 v2 的"宪法"——先想清楚，再动手。

## 1. 为什么重启

v1 在做 pitch 主题过程中暴露的问题：

- **5 层粒度模糊**（Motif / Block / Recipe / Theme / Runtime），中间 Block 这层隐式存在于 sample.html，**没形式化**——每次起新主题都得反向工程
- **跨主题命名漂移风险**：N 个主题之后，`compare-card / contrast-tile / match-up` 这种"语义同义但命名各异"的散乱不可控
- **Runtime drift**：`runtime.html` 是 canonical，但每个 deck 复制一份。改一次 runtime 要手工同步 N 份
- **Spec 与实现互相漂移**：sample.html 里立的约定经常没回到 index.md，反过来 index.md 写了的 recipe 在 sample 里实现不全
- **起新主题成本高**：得读 sample 学，没有"组件清单"可查

**核心诊断**：粒度切错了。砖块应该跨主题共享、独立定义；目前把砖块和主题强耦合了。

## 2. 心智模型：四层结构

```
┌──────────────────────────────────────────────────────────────────┐
│ Layer 1 — Component (契约层)                                      │
│   components/<name>.md  /  blocks/<name>.md                      │
│   声明: HTML 结构 + CSS + 消费哪些 token + slot                    │
│   作用: 跨主题、跨 deck 通用，去风格化                              │
│   不知道: 在哪个主题/页里被用                                       │
└──────────────────────────────────────────────────────────────────┘
                          ↑ 直接消费 palette + scale
┌──────────────────────────────────────────────────────────────────┐
│ Layer 2 — Spec (原语层 / 设计规范文档)                             │
│   samples/<slug>/spec.md                                         │
│   定义: palette (shadcn 命名) + type/shape/spacing scale         │
│         + chrome / page-shell（本主题的页面外壳 HTML + CSS）        │
│   作用: 一份主题的"设计系统"                                        │
│   不规定: 某一页里 card 到底接主色还是副色                          │
└──────────────────────────────────────────────────────────────────┘
                          ↑ 引用 spec + 选 block
┌──────────────────────────────────────────────────────────────────┐
│ Layer 3 — Content (内容层 / deck 大纲)                            │
│   samples/<slug>/content.md                                      │
│   规定: 这份 deck 有哪几页 + 顺序 + 每页用什么 block / chart       │
│         + 每页内容文本 + 每页情境视觉覆盖                          │
│   作用: 每份 deck 一份                                             │
└──────────────────────────────────────────────────────────────────┘
                          ↓ build 工具拼装
┌──────────────────────────────────────────────────────────────────┐
│ Layer 4 — Sample (产物层)                                         │
│   samples/<slug>/<slug>.html                                     │
│   单文件 self-contained HTML，包含全部 CSS + JS + 内容             │
│   由 (component + spec + content) 编译而来                        │
└──────────────────────────────────────────────────────────────────┘

**物理布局**：spec / content / sample 三件套合并到 `samples/<slug>/` 同一文件夹。
逻辑上仍是 3 层（spec 跨 deck 可复用 → 复制 spec.md 到新 deck 文件夹即可）。
```

### 类比映射

| getslide v2 | shadcn ui | Next.js |
|---|---|---|
| Component .md | components/ui | components/ |
| Spec .md      | tailwind.config + globals.css | theme.css |
| Content .md   | (不对应) | pages/posts/*.md |
| Sample .html  | demo page | build output |

### 全局 vs 情境

- **全局一致**（spec 一锤定音）：palette / 字体 family / 字号 scale / 间距 scale / 圆角
- **情境化**（content 决定）：本页 card 用主色还是副色、本页字号档位选择、本页间距档位选择

## 3. 仓库结构

```
docs/                       ← 项目元规范、决策记录、本文
components/                 ← 原子（不依赖其他 component）
  card.md
  numeral.md
  chart-line-default.md     ← chart 转写规范样板（reference，非 slot-fillable）
  ...
blocks/                     ← 组合（依赖 component）
  cards-row.md
  cover.md
  chart-card.md             ← chart 视觉外壳（card + legend），SVG 直接 inline 到 sample
  ...
framework/                  ← v2 契约三件套（曾叫 chrome/）
  deck.html                 ← 可直接拷贝的 HTML 骨架（topbar/sidebar/canvas + § TOKENS / § SLIDES 占位 + canonical CSS/JS）
  design.md                 ← 主题设计契约（schema + 氛围写作指南 + CSS 生成程序，写 design 文档时读）
  content.md                ← 内容结构契约（schema + notes 三规 + 页数节奏 + 常用页模板，写 content 文档时读）
samples/<slug>/             ← 每 deck 一个文件夹（spec + content + 产物三件套）
  spec.md                   ← 主题（palette + scale + chrome 模板）
  content.md                ← 内容大纲（每页 block + 文案 + placement）
  <slug>.html               ← build 产物（self-contained HTML）
old/                        ← v1 全部归档（不再修改，仅供参考）
```

## 4. Token 命名约定（核心架构决策）

- **Palette 层**完全对齐 [shadcn ui](https://ui.shadcn.com/docs/theming)：
  - `--background / --foreground`
  - `--card / --card-foreground`
  - `--primary / --primary-foreground`
  - `--secondary / --secondary-foreground`
  - `--muted / --muted-foreground`
  - `--accent / --accent-foreground`
  - `--destructive / --destructive-foreground`
  - `--border`
- **Scale 层**自定义（shadcn 用 Tailwind config，我们没有 Tailwind）：
  - `--text-xs..5xl`、`--font-display/body/mono`、`--weight-*`、`--leading-*`、`--tracking-*`
  - `--radius-sm/lg/full`、`--space-1..32`、`--shadow-sm/-`
- **不设 component 二级 alias**——不要 `--card-bg / --card-padding-x / --button-default-bg` 这层
  - component CSS 直接 `background: var(--card)` / `padding: var(--space-6)`
  - 主题想改 component 视觉：在 spec 里改 palette token 值，或写 `.card { padding: 32px }` 覆盖

理由：跟 shadcn 命名 + 架构完全一致 → 未来从 shadcn 抄组件零脑力转换；token 数量少一半，spec 短一截。

## 5. Component `.md` 文件格式

每个 component 是一个 `.md` 文件：frontmatter 给机器读，正文给 AI / 人读。

```markdown
---
name: card
deps: []                    # 依赖的其他 component
tokens:                     # 消费的 palette + scale token 列表
  - --card
  - --card-foreground
  - --border
  - --radius
  - --space-6
  - --shadow-sm
source: "@shadcn/card"      # 上游来源（可选，便于将来同步）
---

# Card

矩形内容块。

## Template

​```html
<div class="card" data-slot="card">…</div>
​```

## Style

​```css
.card {
  background: var(--card);
  color: var(--card-foreground);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
}
​```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|

## Constraints

- ...

## Variants（可选）
## Why（可选，记录设计意图）
```

**铁律**：CSS 里**只用** `var(--xxx)`，绝不写死颜色/数值。直接吃 palette + scale，**不设 component 二级 alias**。

## 6. Spec `.md`（设计规范文档）

> **更新（2026-05-08）**：本节是 v2 早期 spec.md 格式的历史记录。**新主题统一按 `framework/design.md` schema 写**——文件改名为 `design.md`，结构升级为 Atmosphere 散文 + 值表 + 装饰系统 + Red lines（不再含 fenced CSS 块）。CSS 由 framework/design.md §8 程序生成。
> 现有 `samples/pitch/spec.md` 和 `samples/archive/spec.md` 保留旧格式直到逐份迁移。

```markdown
---
name: dark-demo
description: "暗灰主题示例"
version: 0.2.0
---

# Dark Demo Theme

## Palette / Type / Shape

​```css
:root {
  /* palette (shadcn 命名) */
  --background:             #0e0f12;
  --foreground:             #ececec;
  --muted:                  #1c1f26;
  --muted-foreground:       #8a8f99;
  --border:                 #2a2d35;
  --card:                   #16181d;
  --card-foreground:        #ececec;
  --primary:                #4f8af5;
  --primary-foreground:     #ffffff;
  --secondary:              #1c1f26;
  --secondary-foreground:   #ececec;
  --accent:                 #62d29a;
  --accent-foreground:      #0e0f12;
  --destructive:            #ef5b5b;
  --destructive-foreground: #ffffff;

  /* typography (scale) */
  --font-display: -apple-system, "SF Pro Display", sans-serif;
  --font-body:    -apple-system, "SF Pro Text", sans-serif;
  --text-xs..5xl: ...;
  --weight-*: ...;
  --leading-*: ...;
  --tracking-*: ...;

  /* shape (scale) */
  --radius-sm: 6px;
  --radius:   12px;
  --radius-lg: 20px;
  --radius-full: 999px;
  --space-1..32: ...;

  /* elevation (scale) */
  --shadow-sm: ...;
  --shadow:    ...;
}

/* 多 variant 主题（如 pitch 的 lite/dark/forest）：*/
[data-variant="dark"] { --background: ...; --foreground: ...; ... }
[data-variant="forest"] { ... }
​```

**注**：spec.md 不再有 component-level 默认映射段。component CSS 直接 `var(--card)` / `var(--space-6)`。

## 选用清单

- components: card / numeral / hairline / pill / drop-cap / chart-line-default（reference 样板）
- blocks:     cover / cards-row / stat-split / pricing-3up / chart-card

## Chrome / Page Shell

每主题独有的页面外壳——logo / 页脚 / 装饰线 / 角落信息等每页重复出现的视觉元素。
**结构 + 样式都属于本主题**，不抽为通用 component（换 spec 时 chrome 整体换）。

​```html
<div class="page-shell" data-variant="...">
  <div class="chrome-top-left">…</div>
  <div class="chrome-top-right">…</div>
  <hr class="chrome-rule" />
  <div class="chrome-bottom-left">…</div>
  <div class="chrome-bottom-right">…</div>
  <slot>{页面内容}</slot>
</div>
​```

## Motif 政策（可选）

- anchor-cards 模式：卡片 `position:absolute` 钉到 chrome-rule 上
- chart 风格：hairline 网格、area fill 0.08 透明度

## Red lines（可选）

- 主题特有的禁忌：no italic / no chromatic accent / 等
```

## 7. Content `.md`（deck 大纲）

> **更新（2026-05-08）**：本节是 v2 早期 content.md 格式的历史记录。**新 deck 统一按 `framework/content.md` schema 写**——每页 4 子段（block / variant / content / chrome / notes，notes 强制），加 §3 写作三规、§2 页数节奏、§6 常用页模板。
> 现有 `samples/pitch/content.md` 和 `samples/archive/content.md` 已大致符合新 schema（仅 notes 部分需补全）。

每页一个 H2 + 三个段：`block` / `content` / `styling`。bullet 格式（自由形态、便于阅读）。

```markdown
---
title: getslide v2 介绍
author: kklauden
date: 2026-05-07
theme: ./spec.md     # 同文件夹内的 spec（samples/<slug>/spec.md）
---

## P1 — Cover

- **block**: cover
- **content**:
  - title: "getslide v2"
  - subtitle: "presentation framework, not a deck"
- **styling**:
  - 背景接 `--primary`（强调入场）

## P2 — Problem

- **block**: problem-numbered
- **content**:
  - 1. v1 粒度切错了
  - 2. Spec 与实现互相漂移
  - 3. 起新主题成本高
- **styling**: 默认

## P7 — Chart narrative

- **block**: chart-narrative
- **chart**: line-chart
- **content**:
  - title: "Adoption growth"
  - data: [12, 18, 27, 38, 51, 64, 76, 86]
  - annotation: "↗ 7× baseline"
- **styling**: 默认
```

**约定**：

- `block` 必须存在
- `content` 必须存在（哪怕只有 title）
- `styling` 缺省 = 用 spec 默认；要覆盖时写 token 名 + 新值
- `chart` 仅图表页有
- 散文/注释直接写在 bullet 里，build 工具忽略非匹配行

## 8. 四层职责边界

| 层 | 知道 | 不知道 |
|---|---|---|
| Component | HTML/CSS + 消费哪些 palette/scale token + 自身约束 | 在哪个主题/页里用 |
| Spec      | palette + type + shape + 选用清单 + chrome 模板 | 某页 component 到底接主色还是副色 |
| Content   | deck 结构 + 每页 block + 文本 + 情境覆盖 | component 内部怎么实现 |
| Sample    | 编译产物，不修改 | — |

跨层 invariant：**契约一致，实现各异**。card 在所有主题里都有 `title / content` slot，但视觉由 spec + content 决定。

## 9. Bootstrap：从 shadcn 开始

种子是 shadcn ui 的现有目录（用 `npx shadcn@latest` CLI 按需访问，不再装本地 devDependency）：

1. `npx shadcn@latest search @shadcn` 拉清单（共 405 项，含 ui/block/style）
2. `npx shadcn@latest view @shadcn/<name>` 拿单个 .tsx 源码 + metadata
3. 每项标 `keep` / `adapt` / `drop` / `add`：
   - `keep`：直接对应（`card`, `separator`, `alert`, `badge`, `button`）
   - `adapt`：要改名或重写（`badge` → `eyebrow`，`alert` → `callout`）
   - `drop`：交互 UI 用不上（`dialog`, `accordion`, `combobox`, `command`）
   - `add`：slide 特有（`numeral`, `drop-cap`, `ambient-frames`, `pill-highlight`, `hero-block`, `pitch-card`）
4. 转写流程：JSON → AI 转 .md
   - React 组件 → HTML + `data-slot` 属性
   - Tailwind utility → CSS（用 `var(--xxx)`，不写死，直接吃 palette + scale）
   - 提取 token 列表到 frontmatter

## 10. 待决议

| # | 问题 | 候选 | 状态 |
|---|---|---|---|
| 1 | 构建流程：`content + spec + components → sample.html` 怎么编译？ | Python 脚本 / Node CLI / 手工拼装 | N=2 sample 时手工，N≥3 时启用工具 |
| 2 | spec.md token 存放形式 | fenced CSS 块 / 独立 tokens.css | ✅ fenced CSS 块 |
| 3 | content.md `styling` 段格式 | bullet / yaml | ✅ bullet |
| 4 | content.md 散文部分如何被 build 忽略 | 段落标记 / regex 提取 | build 工具落地时 |
| 5 | charts 是独立桶还是 components 子集 | 独立 / 合并 | ✅ 合并到 components/（chart-line-default 作为转写规范样板；新 chart 直接 inline 到 sample HTML，不抽组件，跨 ≥2 deck 复用再提取） |
| 6 | 命名约定 | 自创 / shadcn 对齐 | ✅ shadcn 对齐 + 删二级 alias |
| 7 | Runtime sync 工具：何时启用？ | N=2 sample 时 / 现在预先 | N=2 时（已经接近） |
| 8 | AGENTS.md / CLAUDE.md 重写时机 | 第一份 spec + content + sample 跑通后 | 第一份 pitch P1 跑通后 |

## 11. 下一步顺序

```
✓ Step 1   写 1 个 component (card.md)
✓ Step 2   写 1 份 spec.md (dark-demo)
✓ Step 3   写 1 份 content.md
✓ Step 4   手工拼一页 sample 跑通全链路 (demo.html)
✓ Step 5   复制 spec → red-demo 验证换皮 (demo-red.html, 19 行 :root diff)

✓ Step 6   多 component（共 7 个：card / cards-row / separator / badge / button / alert / numeral）
✓ Step 7   shadcn 命名对齐 + 删二级 alias（13 个文件批量重写）
✓ Step 8   pitch spec.md（3 variant + chrome 模板）

→ Step 9   批次 2：cover/closing 元素（hero-block / ambient-frames / pill-highlight）
           + samples/pitch/content.md (P1 + P9)
           + samples/pitch/pitch.html (跑通第一页 pitch sample)

  Step 10  批次 3：卡片家族（pitch-card / feature-grid / solution-staggered）→ P2-P4
  Step 11  批次 4：stat-split / compare-stagger → P5/P6
  Step 12  批次 5：图表（chart-card / line-chart 等）→ P7
  Step 13  批次 6：pricing → P8
  Step 14  完整 9 页 pitch sample
  Step 15  build 工具落地
  Step 16  chrome runtime（独立桶 → 改为 framework/runtime.md，已落地，2026-05-08）
✓ Step 17  framework 三件套：runtime + design + content（schema skeleton），样板待迁移
✓ Step 18  runtime.md → deck.html：runtime 改为可直接 cp 的 HTML 骨架（framework/deck.html），删除 .md 形式（2026-05-08）
```

每一步都是验证假设的最小切片，不要一次性铺完。

## 12. 外部参考：open-design 借鉴清单

> 源：[nexu-io/open-design](https://github.com/nexu-io/open-design) `skills/html-ppt/` + 50+ 派生 theme skill + 71 个 `design-systems/<brand>/DESIGN.md`。
> 33k stars，定位 "Anthropic Claude Design 的本地开源替代"。
> 我们做选择性吸纳——只抄结构上对的、回避方向上错的。

### 高优先级（短期落地，挂到 chrome / runtime 层）

#### A. Presenter Mode（S 键演讲者模式）—— 杀手级特性

按 S 弹出**新窗口**，4 张磁吸卡：
- 🔵 **CURRENT** — 当前页 iframe（用 `?preview=N` 加载，单页无 chrome）
- 🟣 **NEXT** — 下一页 iframe（同样 preview 模式）
- 🟠 **SPEAKER SCRIPT** — 大字号 `<div class="notes">` 内容，可滚动
- 🟢 **TIMER** — 已用时长 + 当前页码 + prev/next/reset 按钮

实现要点：
- 主窗 ↔ 预览窗用 `BroadcastChannel` 双向同步翻页
- iframe 收到 `postMessage({type:'preview-goto', idx:N})` 只切 `.is-active`，**不刷新**
- 卡片 header 可拖、右下角可拉伸、`localStorage` 记住布局
- iframe 用 `?preview=N` URL 参数 → runtime 检测到则只渲染第 N 页、隐藏所有 chrome → 预览像素级一致

**为什么不能省**：任何"我要去给团队讲 xxx / 怕讲不流畅"场景的入门门槛。我们若没有这个，就只是"好看的 HTML 文件"。

#### B. `<div class="notes">` 逐字稿规范

- 每页内嵌一个 hidden `<div class="notes">…</div>`，CSS 默认 `display:none`
- 仅 S 键 presenter window 把它捞出来大字号显示
- **铁律**："NEVER put presenter-only text on the slide"——叙述性文字（"这一页讲…"、"speaker: 提到 X 时…"）必须放进 notes，不准漏到正文 `<p>` / `<span>`
- 写作三规：
  1. **不是讲稿，是提示信号**——加粗核心词 + 过渡句独立成段
  2. **每页 150–300 字**——2–3 分钟/页节奏
  3. **用口语，不用书面语**——"因此"→"所以"，"该方案"→"这个方案"
- 收进 SKILL.md 作为强约束 + samples 都补齐 notes

#### C. T 键主题循环

```html
<body data-themes="dark,lite,forest" data-theme-base="../themes/">
```

- runtime 监听 T → 切 `<link id="theme-link">` 的 href，循环 `data-themes` 列表
- 极轻量：用户能"按 T 现场试 3 套风格"
- 我们已有 `data-variant` 概念，可以扩为同 spec 多 variant 直接 cycle，或跨 spec.md 文件 cycle

#### D. O 键 Overview 网格

- 每个 `.slide` 必须有 `data-title="..."` 属性
- O 键展示所有页缩略图（CSS scale + grid），点击跳转
- 长 deck（> 10 页）必备
- runtime.js 实现成本低（每页 `transform: scale(0.18)` + grid）

#### E. URL hash 深链 `#/N` + `?preview=N` 单页模式

- `#/N` 直接定位到第 N 页（用于嵌入分享、刷新保留位置）
- `?preview=N` 只渲染单页 + 隐藏所有 chrome（presenter iframe 必需）
- runtime 启动时解析这两个参数

### 中优先级（架构启发，落地时机灵活）

#### F. DESIGN.md 设计系统 spec 格式升级

参考 `design-systems/linear-app/DESIGN.md`（370 行）、`apple/DESIGN.md`（250 行）。
**散文 + 表格 + token 列表混合**的结构，AI 读完就能生成一致风格组件：

```
1. Visual Theme & Atmosphere   ← 一段散文，描述整体感受
2. Color Palette & Roles       ← 表格 + 用途说明（不只是 hex 列表）
3. Typography Rules            ← 完整字号矩阵 + 三层权重原则
4. Component Stylings          ← 每个组件的视觉规则
5. Layout Principles           ← 间距系统、网格规则
6. Decorative Vocabulary       ← chrome / 装饰元素的语法
```

我们现在的 `spec.md` 偏纯 CSS 块，信息密度低、AI 生成新 sample 时缺背景。可以升级 spec.md 模板，把"风格原则"用散文写出来。

#### G. 通用语义类去前缀

他们所有 theme 共用一套不带前缀的 primitive 类：
`.kicker / .lede / .eyebrow / .dim / .dim2 / .pill / .card / .row / .grid.g2/g3/g4 / .gradient-text`

我们现在 `archive-page-title / archive-body / archive-impact-rule` 这种 sample 前缀化命名是**反过来的**。
- 教训：**block 层类名不应带 sample 前缀**——primitive 跨主题共用、theme 通过更具体 selector 锁视觉
- 已经做的（bar-chart / chart-gauge）正确，但 archive sample 内部还有大量 `archive-*` 该重新审视哪些可以提为 unprefixed

#### H. 强制的 3 问预飞 workflow

SKILL.md 里写死，AI 不对齐 3 件事不准动笔：
1. **内容/页数/受众**——engineer / exec / 小红书 / VC？
2. **风格主题**——从 N 套里推荐 2-3 个候选
3. **起点**——从哪个 sample 复制？还是空白？

我们现在 SKILL.md 没有这种硬性 gate——AI 容易直接开冲。

#### I. `od:` machine-readable frontmatter

如果未来发布 skill，参考这个 schema：
```yaml
od:
  mode: deck
  scenario: marketing | tech-sharing | report | xhs
  preview: { type: html, entry: example.html }
  design_system: { requires: false }
  speaker_notes: true
  animations: true
```
便于 skill registry 索引。

#### J. 极少量动画基元（不抄全套）

只抄三个真正有用的：
- `data-anim="fade-up"` — 元素入场
- `class="anim-stagger-list"` + `data-anim-target` — 列表/grid 子项级联出现
- `class="counter"` `data-to="92"` — 数字滚动

不抄他们的 27 CSS + 20 canvas FX 全集（粒子/烟花/星空/矩阵雨大部分是噪声）。

### 不抄（明确放弃）

- ❌ **每个 theme 一个独立 skill 包**（50+ html-ppt-* 派生 skill）——他们牺牲 DRY 换发现率，我们 4 层组合架构方向相反
- ❌ **71 个 brand DESIGN.md**——他们 prototype/web 通用，我们只做 slide
- ❌ **27 CSS animations + 20 canvas FX 全集**——大部分对严肃 deck 是噪声
- ❌ **过宽的 trigger 词覆盖**（"monochrome / restrained / literary / considered…"）——容易误激活
- ❌ **`scripts/render.sh` headless Chrome → PNG/PPTX/MP4 export**——v2 不进入导出战场，浏览器打印 PDF 已够用

### 落地节奏

挂到现有 roadmap，建议插入位置：

| 项 | 优先级 | 落地阶段 | 备注 |
|---|---|---|---|
| A. Presenter Mode (S) | P0 | Step 16（chrome runtime）核心模块 | 单独大块 |
| B. `<div class="notes">` 规范 | P0 | Step 16 同期 + 回填到 archive/pitch 已有 sample | A 的前置依赖 |
| C. T 键主题循环 | P1 | Step 16 | 5 行 runtime |
| D. O 键 Overview | P1 | Step 16 | 30 行 runtime + CSS |
| E. `#/N` + `?preview=N` | P0 | Step 16 | A 的前置依赖（preview iframe 必需）|
| F. DESIGN.md 格式升级 | P1 | ✅ framework/design.md + content.md skeleton 已落地（2026-05-08）；samples 迁移待批 | framework/ 三件套就位 |
| G. block 去前缀 | P1 | Step 14 完成后整理 | 跟 archive sample 收尾一起做 |
| H. 3 问预飞 workflow | P2 | SKILL.md rewrite 时（Step 8 决议项）| 文档而非代码 |
| I. `od:` frontmatter | P3 | 发布 skill 前 | 未定 |
| J. 三个动画基元 | P2 | Step 16 之后单独小批次 | 不必跟 chrome 绑定 |

## 13. v1 → v2 选择性回收

v2 完成后，从 `old/` 里挑可复用的内容：

- `old/skills/getslide/runtime.html` → 拆解为 `framework/deck.html` + `framework/design.md` + `framework/content.md`
- `old/skills/getslide/designs/pitch/sample.html` 里的 `.compare-card / .chart-card / .pricing-card` CSS → 重写为 component .md + spec token
- `old/skills/getslide/SLIDE.md` 的 P1-P7 设计原则 → 留作 cross-cutting concern，可能放 `docs/principles.md`
- `old/skills/getslide/designs/*.md` 的主题 token → 改造成新 spec.md 格式

不要直接复制粘贴。每次回收都是一次重新审视的机会。
