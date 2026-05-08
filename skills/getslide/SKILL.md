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
  - `blocks/` — 11 个组合（ambient-frames / bar-chart / cards-row / chart-card / chart-gauge / compare-stagger / feature-grid / hero-block / pricing-3up / solution-staggered / stat-split）
  - `chrome/runtime.md` — deck 运行时外壳（topbar/sidebar/canvas/翻页/演讲者模式/preview/overview/键盘/print）—— **所有新 deck 的 inline CSS+JS 都从这里 copy**
  - `samples/<deck>/` — 每个 deck 自带 spec.md（主题）+ content.md（大纲）+ `<deck>.html`（产物）三件套，self-contained 一文件夹
    - `samples/pitch/`（含 lite/dark/forest 三 variant + 9 页完整 sample）
    - `samples/archive/`（工业档案风 + FAULTLINE 虚构地质监测 deck，9 页）
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
| **改 chrome（topbar/sidebar/翻页/演讲者/overview/preview）** | `chrome/runtime.md` 是 canonical 源头；改完后必须把新 CSS+JS **同步到所有 sample 的 inline `<style>`/`<script>`**。颜色用 `--chrome-*` namespace（7 个 token），spec 给默认值 |
| **加 notes / 演讲者逐字稿** | 每个 `<section class="page-shell">` 内嵌 `<div class="notes">…</div>`；CSS 自动 hidden，S 键 presenter 窗口读取展示。写作三规：提示信号不写讲稿 / 150-300 字 / 用口语 |
| **shadcn 抄组件** | `npx shadcn@latest view @shadcn/<name>`，照已有 component 转写规则：React → HTML+`data-slot`，Tailwind utility → CSS（`var(--xxx)`） |
| **修单页排版/字号/溢出** | 改 content.md 的 styling 段或 sample 里 page 内 inline style |

## Hard rules（不可违反）

### 视觉与命名
- **CSS 只用 `var(--xxx)`**，绝不写死颜色/数值——一切由 spec 注入
- **不设 component 二级 alias**（不要 `--card-bg / --card-padding-x` 这层）。Component CSS 直接 `var(--card)` / `var(--space-6)`
- **Palette 命名对齐 shadcn**：`--background / --foreground / --card / --card-foreground / --primary / --primary-foreground / --secondary / --secondary-foreground / --muted / --muted-foreground / --accent / --accent-foreground / --destructive / --destructive-foreground / --border`
- **Chrome 用 `--chrome-*` namespace**（独立 7 token：bg / canvas-bg / fg / muted-fg / border / accent / accent-fg），spec 给默认值
- **每个 component 用 `data-slot="..."` 属性**（保留 shadcn 约定，跨主题选择器命中）

### slide 结构（chrome runtime 强约束 —— AI 生成新 deck 必须遵守）
- **slide 容器是 1920×1080 + page-shell**（spec 提供）；`.slide-shell > .page-shell` 是硬约定
- **每个 `<section class="page-shell">` 必须有 `data-title="..."` 属性**——O 键 overview 网格、S 键 presenter NEXT 卡都靠这个；缺失 fallback 到 "Slide N" 但很丑
- **每个 page-shell 应内嵌 `<div class="notes">…</div>`**（演讲者逐字稿）。CSS `.slide-shell .notes { display: none }` 永远隐藏；S 键 presenter 窗口才取出展示
- **铁律**：演讲者面向的描述文字（"这一页讲…"、灰色解释 caption）一律放进 `.notes`，**禁止**漏到正文 `<p>`/`<span>`。slide 主体只放观众面向的标题/数据/图
- notes 写作三规：① 不是讲稿是**提示信号**（核心词加粗 + 过渡句独立段）② 每页 150–300 字（2–3 分钟节奏）③ 用口语，不写书面语
- **chrome ring 8 锚点元素类型 + class 名 不可改**：`.chrome-top-left/-right/-bottom-left/-bottom-right` 是 `<div>`，`.chrome-rule` 是 `<hr>`。spec 仅填锚点内的文字 / SVG

### Runtime 行为（inline 到 deck HTML 的 CSS+JS 来源）
- **每 deck 的 inline CSS + JS 必须从 `chrome/runtime.md` 完整 copy**（最稳：直接 copy 一份 sample 然后改内容）
- 必备运行时入口：preview 模式 (`?preview=N`) / hash 深链 (`#/N`) / 演讲者窗口 (S 键) / overview 网格 (O 键) / 主题循环 (T 键 — 仅当 deck 有外置 CSS 文件时生效) / 数字键 1-9 跳页
- 演讲者窗口靠 BroadcastChannel 与主窗双向同步；preview iframe 靠 `postMessage({type:'preview-goto', idx:N})` 不刷新切页

### 文件结构
- **每 deck 一份 `content.md`，每主题一份 `spec.md`**——不要把内容/主题写进 component

## When to refuse

- 「让 slide 响应式」「加 build step」「引入 npm runtime」——v2 走"单文件 self-contained HTML"
- 「在 component 里写死颜色」——所有视觉值必须 token 化
- 「在 spec 里加 component 二级 alias」——v2 已经决议删除（B2 路线）
- 「改 palette token 命名」——已对齐 shadcn 锁定

## Bootstrap from scratch

新 deck 最小流程（**必须从已有 sample 复制，不要从空白开始**——sample 已包含全套 chrome runtime / preview / notes / overview / 演讲者窗口 inline 代码，从空白起 AI 必漏）：

1. **复制整个 sample 文件夹**：`cp -r samples/pitch samples/<slug>` 或 `cp -r samples/archive samples/<slug>`，按目标 deck 风格选最近的
2. 改 `samples/<slug>/content.md` 写新大纲（参考原 sample 同结构）
3. 改 `samples/<slug>/<slug>.html`：
   - 顶部 `<style>` 块前半截的 spec CSS 按需调（palette / chrome / 自有 block）
   - 9 个 `<section class="page-shell">` 替换内容；**保留 `data-title="..." `** + **保留 `<div class="notes">…</div>`**（演讲者逐字稿）
   - 底部 inline `<script>` 段**完全不动**（这是 chrome runtime 的 mirror，碰一行就坏）
4. 浏览器打开验证：← → 翻页、URL 应同步 `#/N`、按 O 看 overview、按 S 弹演讲者窗口、`?preview=N` 单页模式

**生成新 deck 的 AI 默认行为**：
- 复制 sample → 改内容；**不要重写 inline CSS+JS**——它就是 `chrome/runtime.md` 的 mirror
- 9 页全部加 `data-title`；至少封面 + 收尾页加 `<div class="notes">` 示例（中间页可留空，由作者后补）
- 不复制 sample 的场景（如改主题），仍保留 9 页同样的 chrome 锚点 + data-title + notes 结构

无 build 工具时全是手工拼装；build 工具落地后这步自动化（PLAN 待决议 #1）。
