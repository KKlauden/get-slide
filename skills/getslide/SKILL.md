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
  - `framework/` — **三件套骨架**（runtime + design + content），是 v2 的契约层；写新 deck 必读
    - `framework/deck.html` — **可直接拷贝的 HTML 骨架**：完整 chrome 运行时（topbar/sidebar/canvas/翻页/演讲者/overview/preview/print）+ 占位 § TOKENS 块 + 占位 § SLIDES 块。新 deck `cp` 一份后填空即可
    - `framework/design.md` — 主题设计契约，教 AI 怎么写新 design 文档（atmosphere / palette / typography / shape / variants / decorative / red lines schema + CSS 生成程序）
    - `framework/content.md` — 内容结构契约，教 AI 怎么写新 content 文档（每页 4 子段 schema + notes 写作三规 + 页数节奏 + 常用页模板）
  - `samples/<deck>/` — 我们维护的**参考 sample**（pitch / archive），不是 AI 必须放新 deck 的位置；新 deck 在用户选的位置建文件夹即可
    - `samples/pitch/`（含 lite/dark/forest 三 variant + 9 页完整 sample）—— **TODO**: spec.md 待按 framework/design.md 新格式重写
    - `samples/archive/`（工业档案风 + FAULTLINE 虚构地质监测 deck，9 页）—— **TODO**: 同上
- **shadcn 桥** → `npx shadcn@latest view @shadcn/<name>` 拿源码，转写为 `.md`（参考已有 component 的 frontmatter+sections schema）

## Routing

| 任务 | 起点 |
|---|---|
| **看整体架构 / 4 层模型 / token 命名** | `PLAN.md`（必读） |
| **写新 deck** | 1) 通读 `framework/{deck.html, design.md, content.md}` 三件套 2) 在用户选的位置建 `<my-deck>/` 文件夹 3) 写 `<my-deck>/design.md` + `<my-deck>/content.md` 4) `cp framework/deck.html → <my-deck>/<my-deck>.html` 5) 替换 § TOKENS（按 design.md §8 程序）+ 替换 § SLIDES（按 content.md 每页填）|
| **切换 deck 风格**（不动内容）| 改 deck 的 `content.md` 的 `theme:` 指向另一份 design.md；按 framework/design.md §8 程序重生成 § TOKENS 块 |
| **写新主题** | 严格按 `framework/design.md` §1-§7 schema 写 deck 文件夹下的 `design.md`：Atmosphere 散文 + Palette/Typography/Shape 值 + Variants + Decorative signature + Red lines。**design.md 不放 CSS**——CSS 由 framework/design.md §8 程序生成 |
| **写新 deck 大纲** | 严格按 `framework/content.md` §1 schema 写 deck 文件夹下的 `content.md`：每页 H2 + 4 子段（block / variant / content / chrome / notes）。notes 必须遵循 §3 三规 |
| **写新 component** | `components/<name>.md`：frontmatter（name/deps/tokens/source）+ Template + Style + Slots + Constraints + Variants + Why。CSS 只用 `var(--xxx)`，**直接吃 palette + scale**，不设二级 alias |
| **写新 block** | `blocks/<name>.md`，同上 schema。block 可越界给 component 加规则（用 `> [data-slot="..."]` 后代选择器） |
| **写新 chart** | 翻 `components/chart-line-default.md` 末尾"v2 chart 转写规范"10 条；`npx shadcn@latest view @shadcn/chart-<X>-<variant>` 拿源；SVG 直接 inline 到 deck sample HTML 里，**不抽组件**。某 chart 跨 ≥2 deck 反复用再提取 |
| **改 chrome（topbar/sidebar/翻页/演讲者/overview/preview）** | `framework/deck.html` 是 canonical 源头；改完后**所有已生成的 deck `<deck>.html` 都需重新 cp framework/deck.html 重生成**。颜色用 `--chrome-*` namespace（7 个 token），design.md 给默认值 |
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
- **每 deck 的 HTML 必须 cp 自 `framework/deck.html`**——它包含全套 § RUNTIME CSS + § RUNTIME JS canonical 块
- 只能改的两个块：**§ TOKENS**（按 design.md 替换）和 **§ SLIDES**（按 content.md 替换）；其他 canonical 块碰一行就破
- 必备运行时入口：preview 模式 (`?preview=N`) / hash 深链 (`#/N`) / 演讲者窗口 (S 键) / overview 网格 (O 键) / 主题循环 (T 键 — 仅当 deck 有外置 CSS 文件时生效) / 数字键 1-9 跳页
- 演讲者窗口靠 BroadcastChannel 与主窗双向同步；preview iframe 靠 `postMessage({type:'preview-goto', idx:N})` 不刷新切页

### 文件结构
- **每 deck 一份 `content.md`，每主题一份 `design.md`**（曾叫 spec.md）——不要把内容/主题写进 component
- **design.md 严格按 framework/design.md §1-§7 schema 写**，不放 CSS 代码——CSS 由 framework/design.md §8 程序生成
- **content.md 严格按 framework/content.md §1 schema 写**，每页 4 子段必填，notes 遵循三规

## When to refuse

- 「让 slide 响应式」「加 build step」「引入 npm runtime」——v2 走"单文件 self-contained HTML"
- 「在 component 里写死颜色」——所有视觉值必须 token 化
- 「在 spec 里加 component 二级 alias」——v2 已经决议删除（B2 路线）
- 「改 palette token 命名」——已对齐 shadcn 锁定

## Bootstrap from scratch

**起点**：通读 `framework/deck.html`（HTML 骨架）+ `framework/design.md`（主题契约）+ `framework/content.md`（内容契约）。这三份是 v2 的契约，理解了再开工。

新 deck 最小流程：

1. **在用户选的位置建 deck 文件夹** `<my-deck>/`（**不**强制 samples/ 下；samples/ 是我们维护的参考样本，不是 AI 必须放新 deck 的位置）
2. 写 `<my-deck>/design.md` 按 `framework/design.md` §1-§7 schema
3. 写 `<my-deck>/content.md` 按 `framework/content.md` §1 schema
4. **`cp framework/deck.html <my-deck>/<my-deck>.html`**——一次性拿到全部 chrome 运行时
5. 在 `<my-deck>.html` 里只编辑两块：
   - **§ TOKENS** 块（`:root { ... }`）：用 `framework/design.md` §8 程序代入 design.md 的值
   - **§ SLIDES** 块：按 content.md 每页生成一个 `<div class="slide-shell">`，每个 page-shell 必填 `data-title` + `<div class="notes">`
   - `<title>` 改成 deck 名 / topbar 中 `<span class="title">` 改成 deck 名
   - **§ RUNTIME CSS / § RUNTIME JS 块完全不动**——它们是 canonical，碰一行就坏
6. 浏览器打开 `<my-deck>.html` 验证：← → 翻页、URL 同步 `#/N`、按 O 看 overview、按 S 弹演讲者窗口、`?preview=N` 单页模式

**生成新 deck 的 AI 默认行为**：
- 不重写 § RUNTIME CSS / § RUNTIME JS 块——它们是 framework/deck.html 的内容，必须原样保留
- 每页加 `data-title`；至少封面 + 收尾页填 `<div class="notes">`（中间页可留空但 div 必须存在）
- chrome 8 锚点 class 名 (`chrome-top-left/right/-bottom-left/right/-rule`) 不可改
