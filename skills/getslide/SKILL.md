---
name: getslide
description: Use for any task involving a getslide v2 HTML slide deck — building a new deck, swapping themes, adding/editing a slide, authoring a new theme spec, adding new components/blocks/charts, debugging chrome runtime (sidebar/present/scaling/print), or shadcn component import. Triggers on phrases like "make me a deck about X", "build a presentation", "create slides", "switch to <X> theme", "add a new theme", "add a new slide type", "import shadcn component", "fix the chrome", "thumbnails wrong", "present mode broken", "this slide is too crowded".
---

# getslide v2

构建一份 getslide deck。**4 层架构**：component（视觉砖块）+ spec（主题原语）+ content（deck 大纲）+ sample（build 产物）+ chrome runtime（deck 浏览外壳）。

## Quick context

- **完整架构 / 命名约定 / 设计原则** → 读 `PLAN.md`（必读）
- **当前可用资源**：
  - `components/` — 10 个原子（alert / badge / button / card / chart-line-default / contact-list / numeral / pill-highlight / pitch-card / separator）
    - `chart-line-default` 是 **chart 转写规范样板**（reference），不是 slot-fillable 砖块——写新 chart 时翻它当模板
  - `blocks/` — 9 个组合（ambient-frames / cards-row / chart-card / compare-stagger / feature-grid / hero-block / pricing-3up / solution-staggered / stat-split）
  - `chrome/runtime.md` — deck 运行时外壳（topbar/sidebar/canvas/翻页/present）
  - `samples/<deck>/` — 每个 deck 自带 spec.md（主题）+ content.md（大纲）+ `<deck>.html`（产物）三件套，self-contained 一文件夹
    - 当前唯一：`samples/pitch/`（含 lite/dark/forest 三 variant + 9 页完整 sample）
- **shadcn 桥** → `npx shadcn@latest view @shadcn/<name>` 拿源码，转写为 `.md`（参考已有 component 的 frontmatter+sections schema）

## Routing

| 任务 | 起点 |
|---|---|
| **看整体架构 / 4 层模型 / token 命名** | `PLAN.md`（必读） |
| **写新 deck** | 1) 选 `samples/<slug>/spec.md` 2) 在 `samples/<deck>/content.md` 写大纲 3) 拼 `samples/<deck>/<deck>.html`（手工，照 `samples/pitch/pitch.html` 模板） |
| **切换 deck 风格**（不动内容）| 改 `samples/<deck>/content.md` 的 `theme:` 指向另一个 spec；拼 sample 时换 spec 的 fenced CSS 块 |
| **写新主题** | `samples/<slug>/spec.md`：palette（shadcn 命名）+ scale + chrome runtime 默认值 + Chrome / Page Shell（HTML 模板 + CSS）+ 选用清单 + Motif 政策 + Red lines。参考 `samples/pitch/spec.md`（含 multi-variant） |
| **写新 component** | `components/<name>.md`：frontmatter（name/deps/tokens/source）+ Template + Style + Slots + Constraints + Variants + Why。CSS 只用 `var(--xxx)`，**直接吃 palette + scale**，不设二级 alias |
| **写新 block** | `blocks/<name>.md`，同上 schema。block 可越界给 component 加规则（用 `> [data-slot="..."]` 后代选择器） |
| **写新 chart** | 翻 `components/chart-line-default.md` 末尾"v2 chart 转写规范"10 条；`npx shadcn@latest view @shadcn/chart-<X>-<variant>` 拿源；SVG 直接 inline 到 deck sample HTML 里，**不抽组件**。某 chart 跨 ≥2 deck 反复用再提取 |
| **改 chrome（topbar/sidebar/翻页等）** | `chrome/runtime.md`。颜色用 `--chrome-*` namespace（7 个 token），spec 给默认值 |
| **shadcn 抄组件** | `npx shadcn@latest view @shadcn/<name>`，照已有 component 转写规则：React → HTML+`data-slot`，Tailwind utility → CSS（`var(--xxx)`） |
| **修单页排版/字号/溢出** | 改 content.md 的 styling 段或 sample 里 page 内 inline style |

## Hard rules（不可违反）

- **CSS 只用 `var(--xxx)`**，绝不写死颜色/数值——一切由 spec 注入
- **不设 component 二级 alias**（不要 `--card-bg / --card-padding-x` 这层）。Component CSS 直接 `var(--card)` / `var(--space-6)`
- **Palette 命名对齐 shadcn**：`--background / --foreground / --card / --card-foreground / --primary / --primary-foreground / --secondary / --secondary-foreground / --muted / --muted-foreground / --accent / --accent-foreground / --destructive / --destructive-foreground / --border`
- **Chrome 用 `--chrome-*` namespace**（独立 7 token：bg / canvas-bg / fg / muted-fg / border / accent / accent-fg），spec 给默认值
- **每个 component 用 `data-slot="..."` 属性**（保留 shadcn 约定，跨主题选择器命中）
- **slide 容器是 1920×1080 + page-shell**（spec 提供）；`.slide-shell > .page-shell` 是 chrome runtime 的硬约定
- **每 deck 一份 `content.md`，每主题一份 `spec.md`**——不要把内容/主题写进 component

## When to refuse

- 「让 slide 响应式」「加 build step」「引入 npm runtime」——v2 走"单文件 self-contained HTML"
- 「在 component 里写死颜色」——所有视觉值必须 token 化
- 「在 spec 里加 component 二级 alias」——v2 已经决议删除（B2 路线）
- 「改 palette token 命名」——已对齐 shadcn 锁定

## Bootstrap from scratch

新 deck 最小流程：

1. 复制 `samples/pitch/` 为模板，或新建 `samples/<slug>/`（当前唯一主题：pitch）
2. 建 deck：`samples/<slug>/content.md` 写大纲（参考 `samples/pitch/content.md`）
3. 拼 sample：`samples/<slug>/<slug>.html`（每 deck 一个文件夹），照 `samples/pitch/pitch.html` 模板内联 spec + 用到的 components/blocks/chrome runtime CSS + body HTML
4. 浏览器打开验证

无 build 工具时全是手工拼装；build 工具落地后这步自动化（PLAN 待决议 #1）。
