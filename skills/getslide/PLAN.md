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
chrome/                     ← 运行时外壳（sidebar / 导航 / present 模式 / print）  [TODO]
  sidebar.md
  ...
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
  Step 16  chrome runtime（独立桶）
```

每一步都是验证假设的最小切片，不要一次性铺完。

## 12. v1 → v2 选择性回收

v2 完成后，从 `old/` 里挑可复用的内容：

- `old/skills/getslide/runtime.html` → 拆解为 `chrome/*.md`
- `old/skills/getslide/designs/pitch/sample.html` 里的 `.compare-card / .chart-card / .pricing-card` CSS → 重写为 component .md + spec token
- `old/skills/getslide/SLIDE.md` 的 P1-P7 设计原则 → 留作 cross-cutting concern，可能放 `docs/principles.md`
- `old/skills/getslide/designs/*.md` 的主题 token → 改造成新 spec.md 格式

不要直接复制粘贴。每次回收都是一次重新审视的机会。
