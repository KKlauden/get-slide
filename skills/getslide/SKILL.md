---
name: getslide
description: Use for any task involving a getslide v2 HTML slide deck — building a new deck, swapping themes, adding/editing a slide, authoring a new theme spec, debugging chrome runtime (sidebar/present/scaling/print), or chart pattern reference. Triggers on phrases like "make me a deck about X", "build a presentation", "create slides", "switch to <X> theme", "add a new theme", "add a new slide type", "fix the chrome", "thumbnails wrong", "present mode broken", "this slide is too crowded".
---

# getslide v2

构建一份 getslide deck。**4 层架构**：component（视觉砖块）+ spec（主题原语）+ content（deck 大纲）+ sample（build 产物）+ chrome runtime（deck 浏览外壳）。

## Quick context

- **完整架构 / 命名约定 / 设计原则** → 读 `PLAN.md`（必读）
- **当前可用资源**：
  - `framework/` — **契约层**，是 v2 核心；写新 deck 必读
    - `framework/deck.html` — **可直接拷贝的 HTML 骨架**：完整 chrome 运行时（topbar/sidebar/canvas/翻页/演讲者/overview/preview/print）+ 占位 § TOKENS 块 + 占位 § SLIDES 块。新 deck `cp` 一份后填空即可
    - `framework/design.md` — 主题设计契约，教 AI 怎么写 design.md（atmosphere / palette / typography / shape / variants / decorative / red lines schema + §8 Append CSS）
    - `framework/content.md` — 内容结构契约，教 AI 怎么写 content.md（每页 4 子段 schema + notes 三规 + 页节奏 + 模板）
    - `framework/charts/` — chart SVG 转写参考（line / bar / gauge），**不**是 component 库——AI 写 chart 时翻它做参考；普通 UI（card / button / hero / numeral / pill / list）现代 LLM 自己写
  - `samples/<deck>/` — 我们维护的**参考 sample**（pitch / archive）。**当作 case study 读，不是模板抄**——学其视觉决策、变量节奏、abs 钉位 paradigm，**不**直接 cp 整套 §8 Append CSS
    - `samples/pitch/`（含 lite/dark/forest 三 variant + 9 页 YC 风 pitch）
    - `samples/archive/`（黑白工业档案风 + FAULTLINE 虚构地质监测 9 页）

> **重要**：v2 已**砍掉** `components/` + `blocks/` 目录——基础 UI（card / button / hero / numeral / pill / cards-row 等）由现代 LLM 直接写 HTML+CSS 即可，没必要维护 21 份小文档。chart SVG 因为有数学几何（保留 3 份在 `framework/charts/`）。

## Routing

| 任务 | 起点 |
|---|---|
| **看整体架构 / 4 层模型 / token 命名** | `PLAN.md`（必读） |
| **写新 deck** | 0) 跑 **Pre-flight 3 问**（受众/风格/起点）、用户确认 1) 通读 `framework/{deck.html, design.md, content.md}` 三件套 2) 在用户选的位置建 `<my-deck>/` 文件夹 3) 写 `<my-deck>/design.md` + `<my-deck>/content.md` 4) `cp framework/deck.html → <my-deck>/<my-deck>.html` 5) 替换 § TOKENS（按 design.md §8 程序）+ 替换 § SLIDES（按 content.md 每页填）|
| **切换 deck 风格**（不动内容）| 改 deck 的 `content.md` 的 `theme:` 指向另一份 design.md；按 framework/design.md §8 程序重生成 § TOKENS 块 |
| **写新主题** | 严格按 `framework/design.md` §1-§7 schema 写 deck 文件夹下的 `design.md`：Atmosphere 散文 + Palette/Typography/Shape 值 + Variants + Decorative signature + Red lines。**§1-§7 不放 CSS**——CSS 全部进 §8 Append CSS |
| **写新 deck 大纲** | 严格按 `framework/content.md` §1 schema 写 deck 文件夹下的 `content.md`：每页 H2 + 4 子段（block / variant / content / chrome / notes）。notes 必须遵循 §3 三规 |
| **写每页 UI（card / hero / list / pill / cards-row 等）** | 直接在 deck.html 写 HTML+CSS，CSS 只用 `var(--xxx)` 吃 design.md token；**不**翻 components/ blocks/（已删）。需要参考某种排布感时翻 `samples/<theme>/<theme>.html` 看实际写法 |
| **写 chart（line / bar / gauge）** | 翻 `framework/charts/<X>.md` 看 SVG 模板 + CSS pattern；inline 进 deck.html，按 design.md 主题色 override。普通 sparkline 自己写即可 |
| **改 chrome（topbar/sidebar/翻页/演讲者/overview/preview）** | `framework/deck.html` 是 canonical 源头；改完后**所有已生成的 deck `<deck>.html` 都需重新 cp framework/deck.html 重生成**。颜色用 `--chrome-*` namespace（7 个 token），design.md 给默认值 |
| **加 notes / 演讲者逐字稿** | 每个 `<section class="page-shell">` 内嵌 `<div class="notes">…</div>`；CSS 自动 hidden，S 键 presenter 窗口读取展示。写作三规：提示信号不写讲稿 / 150-300 字 / 用口语 |
| **修单页排版/字号/溢出** | 改 content.md 的 styling 段或 deck 里 page 内 inline style |

## Hard rules（不可违反）

### 视觉与命名
- **CSS 只用 `var(--xxx)`**，绝不写死颜色/数值——一切由 spec 注入
- **不设二级 alias**（不要 `--card-bg / --card-padding-x` 这层）。CSS 直接 `var(--card)` / `var(--space-6)`，吃 palette + scale
- **Palette 命名对齐 shadcn**：`--background / --foreground / --card / --card-foreground / --primary / --primary-foreground / --secondary / --secondary-foreground / --muted / --muted-foreground / --accent / --accent-foreground / --destructive / --destructive-foreground / --border`
- **Chrome 用 `--chrome-*` namespace**（独立 7 token：bg / canvas-bg / fg / muted-fg / border / accent / accent-fg），spec 给默认值

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
- 「在 design.md 里加二级 alias」——v2 已经决议删除（B2 路线）
- 「改 palette token 命名」——已对齐 shadcn 锁定

## Pre-flight 3 问（从零做新 deck 必跑）

**触发条件**：用户从零类请求——"做一份 X 的 deck"、"build a deck about Y"、"做个 demo day pitch"、"写份内部分享"等。

**不触发 / 跳过 3 问**：
- 用户已有 deck 文件夹要换皮 / 改单页 / 加新页
- 用户开请求时已经一并给出 audience + 风格 + 起点

**触发后**：**一次性**问完下面 3 题、复述用户答案、用户确认对齐，**才能动笔**写 `design.md` / `content.md` / `<deck>.html`。AI 自己拍板 = 跳过 align = 90% 概率返工。

### Q1 内容 → 决定 content.md 大纲
- **受众**（多选其一或自描述）：VC pitch / 客户 demo / 团队 all-hands / 工程内部分享 / 小红书 / 行业大会 keynote / 其他
- **核心 message**（必填）：要讲的 1-3 个核心点
- **页数候选**：3-5（mini）/ 7-9（标准 9 页 pitch）/ 12-16（详尽）/ 自定
- （可选）时长 / 现场互动还是单向讲

### Q2 风格 → 决定 design.md
- **现成 reference 候选**：
  - **pitch** — YC / TechCrunch 现代科技：单 Cabinet Grotesk + 3 variant 轮换（lite/dark/forest）+ 几何装饰（M18 ambient frames / M20 pill highlight）+ 全 chrome ring。**适合**: 标准 9 页 pitch、产品发布、品牌 deck
  - **archive** — 工业档案 file-aesthetic：Geist + 黑白 2 variant + mono 系统标签 chrome + 大字 hero（≥112px）+ 锐角 `--radius: 0` + abs 钉位。**适合**: 工程 / 系统 / 数据型 deck，工业 / 学术气质
  - **空白起步** — 都不像，从 `framework/design.md` 模板填
- **关键变量**：氛围词（"现代 / 沉稳 / 工业 / 学术 / 极简 / 杂志感 / cyber"）；颜色锚点；字族偏好

### Q3 起点 → sample 当 case study 还是直接发挥

> 默认跟随 Q2：
> - Q2 选 pitch → 读 `samples/pitch/{design.md, content.md, pitch.html}` 全套**当 case study**——学其主题契约（字 / 色 / 形 / chrome 风格 / abs paradigm），按用户主题**重新设计每页 layout + 重写 §8 Append CSS**，不照抄整套 archive-*/pitch-* selector
> - Q2 选 archive → 同上，case study 是 archive
> - Q2 选「空白」/「参考一张图 / 别的视觉素材」 → Path B（见下文 Bootstrap）
>
> **铁律**：sample **不是模板填空**——9 页 abs 坐标 / 30+ archive-* selector 是**为 sample 原文案实测的**，新内容文字长度不一样、节奏不一样，照抄就会撞 chrome / 元素重合 / 视觉灰蒙。AI 必须**按新内容形状**重新设计 layout，sample 提供的是 **DNA**（皮肤气质）不是 **bone**（页结构）

### 答完之后
1. **复述用户答案** + 等用户确认：「我理解你想做一份 受众=X / 风格=Y / 起点=Z / N 页的 deck，核心 message 是 [...]」
2. 用户确认 → 进入下面的 Bootstrap 6 步
3. 用户答得模糊（"现代风格"、"5-9 页都行"），AI **必须**追问具体化，**不准**凭推测开冲

---

## Bootstrap from scratch

**前置**：如果是从零做新 deck，先跑完上面的 [Pre-flight 3 问](#pre-flight-3-问从零做新-deck-必跑)。

**起点**：通读 `framework/deck.html`（HTML 骨架）+ `framework/design.md`（主题契约）+ `framework/content.md`（内容契约）。这三份是 v2 的契约，理解了再开工。

### 起步路径（Pre-flight Q2 答完后决定）

> **核心原则**：**风格是皮肤，内容是骨骼血肉。皮肤要从血肉长出来，不是从 sample 抠出来粘贴。**
> sample 提供 DNA（字 / 色 / 形 / chrome 气质 / 装饰系统），但**每页 layout 必须按新内容形状重新设计**——abs 坐标、卡片宽度、行数都跟原 sample 不一样。

- **Path A: 基于 sample 主题契约 + 自己排版**（Q2 选 **pitch** 或 **archive**）

  1. **读完** `samples/<theme>/{design.md, content.md, <theme>.html}` 全套——重点看 §1 Atmosphere（什么调性）/ §2 Palette / §3 Typography / §6 Decorative signature。**理解主题 DNA**
  2. 在 `<my-deck>/design.md` 里：
     - §1-§7 可以**参考** sample 写法，但 **§1 Atmosphere 必须按新主题重写**（4 段 prose 全换）；§2 Palette 按新色调改；§6 Decorative motif 叙事换
     - §3 Typography / §4 Shape / §5 Variants / §7 Red Lines 一般沿用 sample（除非用户明确要换）
     - **§8 Append CSS 不照搬**：保留 chrome 锚点位置 / page-shell / grid / utility classes 这类**主题级 paradigm**；**删掉** sample 里所有 page-specific selector（如 `archive-page-title / archive-stat-list / archive-coverage-panel` 等 30+ 个）——它们是为 sample 原文案实测的坐标，不是主题契约
  3. content.md 按新主题 9 页节奏自己设计（可参考 sample 节奏）
  4. § SLIDES：每页**按内容形状决定 layout**——基础 UI（card / hero / list / pill）直接写 HTML+CSS，吃 design.md token；abs 坐标按当前页文字长度反推

- **Path B: 完全空白起步**（Q2 选**「空白」** 或 **「参考一张图 / 别的视觉素材」**）

  1. 按 `framework/design.md` 空白模板从零写 §1-§7
  2. 如果用户给了参考图：先 Read 图，写一段 200-300 字「我从图里看到了什么」visual DNA 笔记（色 / 字 / 形 / motif / chrome / 节奏），**再**开始写 design.md。§1 Atmosphere 必须呼应观察笔记
  3. §8 Append CSS 自己设计（chrome 8 锚点位置 + page-shell padding + 是否要 grid + 装饰 motif）
  4. content.md / § SLIDES 同 Path A 第 3-4 步

两条路径都进入下面的「6 步流程」。

### 6 步流程

1. **在用户选的位置建 deck 文件夹** `<my-deck>/`（**不**强制 samples/ 下；samples/ 是我们维护的参考样本，不是 AI 必须放新 deck 的位置）
2. 写 `<my-deck>/design.md`：Path A 用 cp + 局部改；Path B 按 framework/design.md schema 从零写
3. 写 `<my-deck>/content.md` 按 `framework/content.md` §1 schema（Path A 可参照 sample content.md 的页节奏；Path B 自由）
4. **`cp framework/deck.html <my-deck>/<my-deck>.html`**——一次性拿到全部 chrome 运行时
5. 在 `<my-deck>.html` 里只编辑两块：
   - **§ TOKENS** 块（`:root { ... }`）：用 `framework/design.md` §8 程序代入 design.md 的值 + 把 design.md §8 Append CSS 整段贴在 `:root` 块下方（Path A 是从 cp 的 design.md 直接拿；Path B 是自己写的）
   - **§ SLIDES** 块：按 content.md 每页生成一个 `<div class="slide-shell">`，每个 page-shell 必填 `data-title` + `<div class="notes">`
   - `<title>` 改成 deck 名 / topbar 中 `<span class="title">` 改成 deck 名
   - **§ RUNTIME CSS / § RUNTIME JS 块完全不动**——它们是 canonical，碰一行就坏

6. **视觉自检（每页生成完做一次 mental test，发现问题立即改）**：

   - **abs 元素 bbox 不重叠**：每页用 abs 钉位的多个元素，`top + height` 和 `left + width` 反推 bbox，**两两相交直接是 bug**
   - **不撞 chrome**：内容元素 top ≥ chrome-top 锚点 bottom（一般 ≥ 130-280 看 chrome 字号）；bottom ≥ chrome-bottom 锚点 top（一般底部留 96-160px）；左右留 grid-margin-x（一般 64-96）
   - **文字 wrap 后空间够**：`<h1 class="hero">` / 长 body 段，按当前内容字数估行数 × 行高（line-height × font-size），加起来不能超过容器 height；hero 大字（≥88px）特别容易溢出
   - **大字 hero 行间够留白**：`font-size: 88px` 起，`line-height` ≥ 0.95（紧）/ 1.05（松）；多行 hero 之间不能撞
   - **chrome 文字不挤**：archive 主题 chrome top-right 多行 mono 标签时，注意 right 边距和最长那行宽度；pitch 主题 chrome-top-left 的 logo + brand 不能被 hero 大字盖
   - **§8 abs 坐标按当前页文字反推**：Path A 不能照抄 sample 的 archive-* 坐标——sample 坐标是为原文案实测的；新文案字数不一样必须重排

7. **浏览器开 `<my-deck>.html` 实测**（mental test 不替代真测）：
   - ← → 翻页、URL 同步 `#/N`
   - 按 O 看 overview 网格（每页缩略图能不能看）
   - 按 S 弹演讲者窗口（NEXT 卡片用 `data-title`，SCRIPT 卡片用 `<div class="notes">`）
   - `?preview=N` 单页无 chrome 模式
   - **每页视觉过一遍**：是否有重叠 / 撞 chrome / wrap 难看 / 字号不协调 / 颜色灰蒙；任一项 fail，回 step 6 修

**生成新 deck 的 AI 默认行为**：
- 不重写 § RUNTIME CSS / § RUNTIME JS 块——它们是 framework/deck.html 的内容，必须原样保留
- 每页加 `data-title`；至少封面 + 收尾页填 `<div class="notes">`（中间页可留空但 div 必须存在）
- chrome 8 锚点 class 名 (`chrome-top-left/right/-bottom-left/right/-rule`) 不可改
