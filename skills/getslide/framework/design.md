---
name: design
role: framework-skeleton
description: 主题设计契约——教 AI 怎么写新的 design 文档（per-deck 一份）
---

# Design — 主题设计契约

> framework 三件套之一：**runtime / design / content**。这份文档不是某个具体主题，是"怎么写主题"的标准。
>
> 每份新 deck 在自己的项目文件夹下放一份 `design.md`（**不**强制放 `samples/` 下；AI 在用户选的位置建文件夹），**严格按本文 §1-§7 schema 写**。CSS 不在 design.md 里手写——拼 deck HTML 时由本文 §8 程序自动生成 + framework/deck.html 的 § TOKENS 块替换。

## 这份文档做什么

设计层是 4 层架构里的**主题原语层**。一份 deck 的视觉气质（用什么色、什么字、什么形、装饰系统怎么组织）由 design.md 一锤定音。

每份 deck 的 `design.md`（与 `content.md` 和 `<deck>.html` 同文件夹）包含：
- 一段 **Atmosphere** 散文（让 AI 在做新页面时"凭感觉"知道该怎么走）
- **值表**（palette / type / shape，纯数字纯字符串，不写 CSS）
- **Decorative signature**（主题特有视觉系统的规则描述）
- **Red lines**（主题禁忌）

## 怎么用

**写新主题**：
1. 通读本文 §1-§7（schema 知道每节填什么）
2. 通读末尾"氛围写作指南"（学怎么写好 §1）
3. 把末尾"空白模板"复制为 `<my-deck>/design.md`（在你的新 deck 文件夹下）
4. 一节一节填值
5. 拼 deck HTML 时：cp framework/deck.html → `<my-deck>/<my-deck>.html`，然后用本文 §8 程序代入 design.md 值 → 替换 deck.html 的 § TOKENS 块

**写新页面**：先读本 deck 的 design.md §1 Atmosphere——这决定页面"长什么样"——再翻 §2-§6 取值。

---

## §1 Atmosphere（氛围）

3-4 段散文，让读者"感觉到"主题的温度，不是"读懂"参数。末尾收 5-10 条 Key Characteristics 当 quick reference。

### 必写四面

| 段 | 谈什么 |
|---|---|
| 1 | **整体气质** — 画面感、隐喻、情绪、时代感。把读者放进设计的"温度"里 |
| 2 | **字的角色** — 选了什么 font family、为什么；font 怎么工作（weight / tracking / 字幅） |
| 3 | **色的逻辑** — monochrome / 双色 / 多色？accent 克制还是奔放？深浅分布 |
| 4 | **装饰风格**（可选）— 装饰元素、空间逻辑、特殊技法（grid / shadow / 玻璃 / 网格底纹） |

末尾 **Key Characteristics**：5-10 条 bullet，把散文里的关键点摘出来，**给后续 AI 用做 quick reference**。

### 写作三戒

- **戒散布** — 每段聚焦一个主题，不交叉
- **戒空洞** — 必须给具体值。不是"略带蓝调"，是"barely-blue (`#08090a`)，蓝调几乎察觉不到"
- **戒列表替散文** — bullet 是收尾不是开场。先用句子把感觉讲出来，再 bullet 总结

### 范文（linear-app 节选）

> Linear's website is a masterclass in dark-mode-first product design — a near-black canvas (`#08090a`) where content emerges from darkness like starlight. The overall impression is one of extreme precision engineering: every element exists in a carefully calibrated hierarchy of luminance, from barely-visible borders (`rgba(255,255,255,0.05)`) to soft, luminous text (`#f7f8f8`).
>
> The typography system is built entirely on Inter Variable with OpenType features `"cv01"` and `"ss03"` enabled globally, giving the typeface a cleaner, more geometric character. Inter is used at a remarkable range of weights — from 300 (light body) through 510 (medium, Linear's signature weight) to 590 (semibold emphasis). The 510 weight is particularly distinctive: it sits between regular and medium, creating a subtle emphasis that doesn't shout.

为什么这段好：**画面感（"starlight"）+ 具体值（`#08090a`）+ 设计意图（"precision engineering"）+ 实施细节（"OpenType cv01"）** 都在散文里。读者既感受到了气质，又拿到了精确数据。

### 范文（apple 节选）

> Apple's website embodies a refined, premium aesthetic that prioritizes content over chrome — a near-pure-white canvas (`rgb(251, 251, 253)`) with deep black text creating maximum contrast and readability. The design philosophy is one of "design is how it works" — invisible craftsmanship where every element serves the content.

为什么这段好：用一句"design is how it works"slogan 做骨，包住整段哲学。**不需要堆形容词**——选对锚词比堆 10 个形容词强。

### 反例（别这么写）

❌ **bullet-only**：
> - Dark mode native
> - Inter font
> - Indigo accent

读者不知道为什么、什么时候、有多 dark。

❌ **抽象散文**：
> The theme feels modern and clean, with a sense of professionalism and elegance.

"modern / clean / professional / elegant" 是空话——任何主题都能套。**没有信息量。**

---

## §2 Palette values（色板）

按**角色**分区列 token。每个 token 一行，列出每个 variant 的 hex + 一句话用途。

### 读图 → token 映射 SOP（用户给参考图 / 描述时**必读**）

参考图里的视觉色不能 1:1 套到 `--background` —— **要先判断每个色在视觉里担任什么"角色"**，再映射到对应 token。常见误判：把"主色调"写到 `--background`，结果整页都是主色铺底（应该是 `--canvas-bg` 在 chrome 外层显示 + page 内是中性底色）。

按这个决策树挑：

| 参考图里看到的 | 担任什么角色 | 映射到 token |
|---|---|---|
| **整张图最大面积、外层框、画布底** | 视觉环境 / 包裹层 | `--canvas-bg`（chrome runtime token，外层 viewport） |
| **每页 slide 的底色**（chrome 4 角文字所在的卡） | 页面主底 | `--background`（page bg） |
| **页内卡片 / panel / 信息块的底色** | 卡片层 | `--card`（页内卡片） |
| **次级面板 / 内嵌区域底** | 二级表面 | `--secondary`（很少用） |
| **大段文字色 / hero 大字色** | 主要文字 | `--foreground` |
| **caption / 次要文字 / 注释** | 弱文字 | `--muted-foreground` |
| **品牌主色 / logo 色 / 主行动按钮底色** | 主品牌 | `--primary` |
| **小亮点：pill / chip / accent dot / 强调描边** | 强调点缀 | `--accent` |
| **错误提示 / 警告色（≠ accent）** | 状态告警 | `--destructive` |
| **分隔线 / hairline / 卡片描边** | 视觉切分 | `--border` |

### 例：CHASSAN 参考图（橄榄绿 + 米卡 + 黄绿 dot）的正确映射

```
图里看到                          错误（V2 subagent 犯过）            正确
──────────────────────────────────────────────────────────────────
橄榄军绿（外层大面积）             --background: #7a8a2e ❌            --canvas-bg: #7a8a2e ✓
                                  （结果：整页绿底压住米卡）          （绿色作为 chrome 外层 viewport）
米奶油（slide 底 / chrome 所在）   --card: #efe7c8 ❌                  --background: #efe7c8 ✓
深色卡（小标签 / pull box）        --card: #1a1f12 ✓                   --card: #1a1f12 ✓
亮黄绿（dot / pill / 强调块）       --accent: #cfe14a ✓                 --accent: #cfe14a ✓
```

**铁律**：参考图的"主色"看面积，**最大面积通常是 `--canvas-bg`，不是 `--background`**。只有"整页全部铺主色"的极端 minimal 主题（如 archive dark variant）才让 `--background` = 主色。

### 必写五区

1. **Background Surfaces** — page canvas / panel / card / elevated / overlay
2. **Text & Content** — primary / secondary（muted-foreground）
3. **Brand & Accent** — primary / accent / accent-foreground
4. **Status** — destructive / success / warning（用得到才写）
5. **Border** — default border / muted border

### 必写格式

token 命名**对齐 shadcn**——15 个标准 token 名锁死：

```
--background  --foreground
--card        --card-foreground
--primary     --primary-foreground
--secondary   --secondary-foreground
--muted       --muted-foreground
--accent      --accent-foreground
--destructive --destructive-foreground
--border
```

每个 token 必须列出**所有 variant 的值**。例：

```markdown
### Background Surfaces

- `--background` — page canvas（slide 底色）
  - lite:   `#f0f0f0`
  - dark:   `#0a0a0a`
  - forest: `#0d2a25`
- `--card` — content container
  - lite:   `#f0f0f0`              （透明叠在 background 上）
  - dark:   `rgba(0, 0, 0, 0.30)`  （半透明叠黑，深底再压一层）
  - forest: `rgba(0, 0, 0, 0.24)`
```

写完所有 5 区 × 15 token × N variants 后，**chrome runtime 7 token 的默认值也在这里写**：

```markdown
### Chrome Runtime（跨 variant 不变）

- `--canvas-bg`        — `#dcdcdc`            （外层画布，sidebar 外）
- `--chrome-bg`        — `#f0f0f0`            （sidebar / topbar 底）
- `--chrome-fg`        — `var(--foreground)`  （指向 palette）
- `--chrome-muted-fg`  — `var(--muted-foreground)`
- `--chrome-border`    — `var(--border)`
- `--chrome-accent`    — `var(--accent)`
- `--chrome-accent-fg` — `var(--accent-foreground)`
```

### 必须遵守

- 全 15 token 全 variant 必须列；缺一个 §8 程序生成的 CSS 就漏一行
- 写 CSS 函数（`rgba` / `color-mix`）也算合法值
- 注释（括号里的解释）放在值后面

---

## §3 Typography values（字体方案）

### 必写

#### Font Family

```markdown
- **Primary** (display + body 通常同族): `Cabinet Grotesk`, `Geist`, `Inter`, `-apple-system`, `BlinkMacSystemFont`, `"Segoe UI"`, `"PingFang SC"`, `"Hiragino Sans GB"`, `"Microsoft YaHei"`, sans-serif
- **Mono** (代码 / 技术 label): `ui-monospace`, `"JetBrains Mono"`, `"SF Mono"`, Menlo, monospace
- **OpenType features** (可选): `"cv01", "ss03"` 全局开启——alternate lowercase a + cleaner letterforms
```

中文 fallback chain **必须**包含 `"PingFang SC"` / `"Hiragino Sans GB"` / `"Microsoft YaHei"` 至少一个。

#### Size Scale + use case

```markdown
- `--text-xs`    16px  — caption / label / micro UI
- `--text-sm`    18px  — body small
- `--text-md`    22px  — body
- `--text-lg`    32px  — h3 / card title
- `--text-xl`    44px  — h2
- `--text-2xl`   76px  — display small
- `--text-3xl`   88px  — display
- `--text-4xl`  120px  — hero / cover only
```

每档**必带 use case**——告诉后续 AI 哪页用哪档。

#### Weight Scale

```markdown
- `--weight-normal`     400  — body 默认
- `--weight-medium`     500  — UI label / nav
- `--weight-semibold`   600  — title / strong emphasis
- `--weight-bold`       700  — display only
```

#### Leading + Tracking Scale

```markdown
**Leading**（line-height）
- `--leading-tight`   1.05  — display 大字
- `--leading-snug`    1.15  — h2-h3
- `--leading-normal`  1.4   — UI 文字
- `--leading-body`    1.55  — 正文段

**Tracking**（letter-spacing）
- `--tracking-tight`  -0.02em  — 大字号自动收紧
- `--tracking-snug`   -0.015em — 中字号微收
- `--tracking-normal`  0       — 默认
- `--tracking-wide`    0.22em  — eyebrow / kicker
```

### Principles 必写 3-5 条

讲**这套 scale 怎么用**，不是讲值。例如：

```markdown
- **compress at scale** — 字号越大 tracking 越紧（-0.02em @ 120px，0 @ 22px）。display 用紧 letter-spacing 强化"工程感"
- **single weight system** — 大部分 UI 用 medium（500）；强调用 semibold（600）。bold（700）仅 hero 用，避免"weight noise"
- **leading 跟字号反比** — display 1.05（紧）→ body 1.55（宽）。大字本身已强调，行距压紧；小字给空气
```

---

## §4 Shape values（圆角 + 间距）

### Radius Scale

```markdown
- `--radius-sm`     16px   — tag / pill 小元素
- `--radius`        20px   — default（card / panel）
- `--radius-full`   999px  — pill 全圆角
```

### Space Scale

按 4-base 等差：

```markdown
- `--space-1`     4px
- `--space-2`     8px
- `--space-3`    12px
- `--space-4`    16px
- `--space-6`    24px
- `--space-8`    32px
- `--space-12`   48px
- `--space-16`   64px
- `--space-24`   96px
- `--space-32`  128px
```

### Grid（如果主题有结构网格）

```markdown
- columns: 12
- rows: 8
- margin x: 96px
- margin top: 96px
- margin bottom: 96px
- gutter: 24px
- row gap: 24px

**Track 命名**（page-content data-layout="strict" 用）：
- `--track-eyebrow`: 1 / 4
- `--track-title`:   1 / 9
- `--track-body`:    1 / 9
- `--track-hero`:    1 / 9
- `--track-stat`:    7 / -1
- `--track-aside`:   9 / -1
```

### Shadow

```markdown
- `--shadow-sm`: none           （主题禁用阴影 → 都置 none）
- `--shadow`:    none
```

或

```markdown
- `--shadow-sm`: 0 1px 2px rgba(0, 0, 0, 0.06)
- `--shadow`:    0 4px 12px rgba(0, 0, 0, 0.10)
```

---

## §5 Variants（变体）

### 必写

1. **变体清单** — 哪几个、各自语义角色（一句话）
2. **何时用** — cover / body / closing / 数据页 各用哪个
3. **跨 variant 不变的 token** — 比如固定浅色 chrome（即使 slide 是 dark）

### 范例

```markdown
- **lite** (default) — 米白底，monochrome 主体；body 页默认
- **dark**           — 真黑底；cover / 重要数据 / 收尾页强调
- **forest**         — 深绿底；problem / market 主题色页

**何时用哪个**：
- cover P1: dark（强冲击）
- body 大多数: lite（中性默认）
- problem P2: dark（情绪色彩）
- market sub-page P8: dark + bg-mode="market"（叠加渐变）
- closing P9: lite（收束回平静）

**跨 variant 不变**：chrome runtime 永远浅色 chrome（`--chrome-bg: #f0f0f0`）——
即使 slide 是 dark，sidebar 仍浅色。理由：编辑器风一致性。
```

### 多变体设计原则

- **palette token 全 variant 必填**——`[data-variant="dark"] { ... }` 必须给全 15 token 完整覆盖，不能只覆盖 `--background`
- **typography / shape / grid 跨 variant 不变**——只 palette 切换；字号 / 圆角 / 间距 / 网格永远一致
- **variant 不是色彩主题**——是**情绪角色**。每个 variant 有自己的"用法"，不是简单的"日间/夜间"

---

## §6 Decorative signature（装饰系统）

主题特有的视觉语言——**chrome ring / 装饰图层 / 强调模式 / 网格底纹** 等。每一项**给规则不给截图**。

### 每项必写四面

1. **是什么** — 名字 + 分类（chrome ring / overlay / accent system / etc）
2. **几何参数** — 位置、尺寸、间距、颜色
3. **用法** — 何时用、何时不用、跨 variant 怎么变
4. **HTML 模板**（可选）— 如果 spec 决定 HTML 结构而不是 chrome runtime

### 范例

```markdown
### Chrome Ring (8 锚点)

- 每页都有，**不可省略**——cover 也不能跳过
- 锚点位置（chrome runtime 强约束）：
  - `chrome-top-left` — logo / brand mark
  - `chrome-top-right` — copyright / 当前页号
  - `chrome-bottom-left` — footer / brand secondary
  - `chrome-bottom-right` — page number / brand letter
  - `chrome-rule` — 顶部 96px 处 hairline `<hr>`
- 视觉：1px hairline，颜色 `var(--chrome-border)`，跨 variant 自动跟随

### M18 Ambient Frames

- 圆角矩形 SVG，背景装饰
- 仅 cover 和 closing 页用
- 尺寸：1208×920 + 1572×120 双层叠加
- 颜色：`var(--foreground)` 8% opacity——融入但不抢戏
- HTML 模板：`<div class="ambient-frames" data-variant="...">…</div>`

### M20 Pill Highlight

- 强调系统：pitch 拒绝 `--accent` 染色，强调靠 pill bg
- 几何：`padding: 4px 12px`、`border-radius: --radius-full`
- 用法：number / 关键词包 pill，title 不包
- 跨 variant：`background: var(--secondary)`，自动适配
```

---

## §7 Red Lines（禁忌）

主题特有的禁忌——做了就破气质。每条**理由**写清。

### 范例

```markdown
- **No italic** — Cabinet Grotesk 不带 italic 字面，用 system fallback 会破字族一致性
- **No chromatic accent** — `--accent` 锁 `--foreground`；强调靠 pill highlight 不靠颜色
- **No shadow** — `--shadow: none`，扁平美学；主题想表达"卡片悬停"用 border 不用 shadow
- **No rounded > 24px** — 圆角超过 24px 进入"软风"领域，pitch 是 grotesk 主义，硬挺为美
```

写禁忌的好处：**未来 AI 想"加点 italic 强调"会被 §7 阻拦**——main slide 视觉一致。

---

## §8 CSS 生成程序（framework 提供，每份 design.md 不重复）

拼 sample.html 时，把 design.md 的 §2-§5 值代入下面模板，生成 :root + variant 的 inline `<style>` 段。

### 模板

```css
/* ============ Tokens (auto-generated from design.md) ============ */
:root {
  /* ─────────── 非 variant 共享 token ─────────── */
  --canvas-bg: {{chrome.canvas-bg}};
  --chrome-bg: {{chrome.chrome-bg}};

  /* ─────────── typography (scale) ─────────── */
  --font-display: {{type.family.display}};
  --font-body:    {{type.family.body}};
  --font-mono:    {{type.family.mono}};

  --text-xs:   {{type.size.xs}};
  --text-sm:   {{type.size.sm}};
  --text-md:   {{type.size.md}};
  --text-lg:   {{type.size.lg}};
  --text-xl:   {{type.size.xl}};
  --text-2xl:  {{type.size.2xl}};
  --text-3xl:  {{type.size.3xl}};
  --text-4xl:  {{type.size.4xl}};

  --weight-normal:    {{type.weight.normal}};
  --weight-medium:    {{type.weight.medium}};
  --weight-semibold:  {{type.weight.semibold}};
  --weight-bold:      {{type.weight.bold}};

  --leading-tight:   {{type.leading.tight}};
  --leading-snug:    {{type.leading.snug}};
  --leading-normal:  {{type.leading.normal}};
  --leading-body:    {{type.leading.body}};

  --tracking-tight:   {{type.tracking.tight}};
  --tracking-snug:    {{type.tracking.snug}};
  --tracking-normal:  {{type.tracking.normal}};
  --tracking-wide:    {{type.tracking.wide}};

  /* ─────────── shape (scale) ─────────── */
  --radius-sm:   {{shape.radius.sm}};
  --radius:      {{shape.radius.default}};
  --radius-full: {{shape.radius.full}};

  --space-1:    {{shape.space.1}};
  --space-2:    {{shape.space.2}};
  --space-3:    {{shape.space.3}};
  --space-4:    {{shape.space.4}};
  --space-6:    {{shape.space.6}};
  --space-8:    {{shape.space.8}};
  --space-12:   {{shape.space.12}};
  --space-16:   {{shape.space.16}};
  --space-24:   {{shape.space.24}};
  --space-32:   {{shape.space.32}};

  /* ─────────── grid (如果主题有) ─────────── */
  --grid-columns:        {{shape.grid.columns}};
  --grid-rows:           {{shape.grid.rows}};
  --grid-gutter:         {{shape.grid.gutter}};
  --grid-row-gap:        {{shape.grid.row-gap}};
  --grid-margin-x:       {{shape.grid.margin-x}};
  --grid-margin-top:     {{shape.grid.margin-top}};
  --grid-margin-bottom:  {{shape.grid.margin-bottom}};
  /* + track-* 按需 */

  /* ─────────── elevation ─────────── */
  --shadow-sm: {{shape.shadow.sm}};
  --shadow:    {{shape.shadow.default}};

  /* ─────────── 默认 variant tokens（= 第一个 variant 的值） ─────────── */
  --background:             {{palette.background.<default-variant>}};
  --foreground:             {{palette.foreground.<default-variant>}};
  --muted:                  {{palette.muted.<default-variant>}};
  --muted-foreground:       {{palette.muted-foreground.<default-variant>}};
  --border:                 {{palette.border.<default-variant>}};
  --card:                   {{palette.card.<default-variant>}};
  --card-foreground:        {{palette.card-foreground.<default-variant>}};
  --primary:                {{palette.primary.<default-variant>}};
  --primary-foreground:     {{palette.primary-foreground.<default-variant>}};
  --secondary:              {{palette.secondary.<default-variant>}};
  --secondary-foreground:   {{palette.secondary-foreground.<default-variant>}};
  --accent:                 {{palette.accent.<default-variant>}};
  --accent-foreground:      {{palette.accent-foreground.<default-variant>}};
  --destructive:            {{palette.destructive.<default-variant>}};
  --destructive-foreground: {{palette.destructive-foreground.<default-variant>}};

  /* ─────────── chrome runtime ─────────── */
  --chrome-fg:        {{chrome.fg}};
  --chrome-muted-fg:  {{chrome.muted-fg}};
  --chrome-border:    {{chrome.border}};
  --chrome-accent:    {{chrome.accent}};
  --chrome-accent-fg: {{chrome.accent-fg}};
}

/* ─────────── 显式 variant 覆盖 ─────────── */
{{#each variants}}
[data-variant="{{name}}"] {
  --background:             {{palette.background.{{name}}}};
  --foreground:             {{palette.foreground.{{name}}}};
  --muted:                  {{palette.muted.{{name}}}};
  --muted-foreground:       {{palette.muted-foreground.{{name}}}};
  --border:                 {{palette.border.{{name}}}};
  --card:                   {{palette.card.{{name}}}};
  --card-foreground:        {{palette.card-foreground.{{name}}}};
  --primary:                {{palette.primary.{{name}}}};
  --primary-foreground:     {{palette.primary-foreground.{{name}}}};
  --secondary:              {{palette.secondary.{{name}}}};
  --secondary-foreground:   {{palette.secondary-foreground.{{name}}}};
  --accent:                 {{palette.accent.{{name}}}};
  --accent-foreground:      {{palette.accent-foreground.{{name}}}};
  --destructive:            {{palette.destructive.{{name}}}};
  --destructive-foreground: {{palette.destructive-foreground.{{name}}}};
}
{{/each}}

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

### 替换规则

- `{{palette.X.Y}}` → §2 token X 在 variant Y 的值
- `{{type.size.X}}` → §3 size scale 的 X 档值
- `{{type.family.X}}` → §3 family 整条字符串（含完整 fallback chain）
- `{{shape.radius.X}}` → §4 radius X
- `{{shape.space.N}}` → §4 space N
- `{{shape.grid.X}}` → §4 grid X
- `{{shape.shadow.X}}` → §4 shadow X
- `{{chrome.X}}` → §2 Chrome Runtime 区段的 X
- `{{#each variants}}...{{/each}}` → 对每个 variant 重复渲染块

### Append CSS（每份 design.md 必须 append，手抄不模板化）

§8 模板只生成 token + base reset，下面这些层是**主题级布局原语**——每份主题的位置 / 网格 / 装饰都不同，framework 不替你抽。design.md 末尾用 **一个** ` ```css ` fenced block 写完，AI 拼 sample.html 时**一字不差**贴在生成的 `:root` 块下方。

按这个顺序写：

1. **Chrome 8 锚点位置 CSS**（如果想 chrome ring 可见）
   `.chrome-top-left / -top-right / -bottom-left / -bottom-right` 四条 + `.chrome-rule` 一条。来自 §6 Chrome Ring 决议。framework/deck.html 已经给 `.chrome-rule` 一个 `border: 0; margin: 0; height: 0` 的兜底，想画线就在这里 override 回来
2. **`.page-shell` 内边距 / overflow**
   framework/deck.html 已经给了 1920×1080 + 居中 transform，design.md 在这里只补 `padding / position / overflow` 等
3. **`.page-content` 容器 + grid 模式**（如果 §4 有 Grid）
   12×N 严格 grid + flow mode 双轨（参考 pitch）。某些主题用 flexbox 单流就够，可省
4. **Utility classes**（可选）
   如 `.h1-9` / `.v3-8` 这类 grid-column / row 工具类，仅当主题用 strict grid 才需要
5. **Component-level overrides**（如果 §6 Decorative signature 有声明）
   主题级 component bend，如 pitch `[data-variant="dark"] .pitch-card { background: #0d2a25; }`
6. **Decorative motif inline CSS**（如果 §6 用了 M18/M20 等装饰）
   如 `.ambient-frame` SVG 容器、`.pill-highlight` background 等

**铁律**：**只**这一段是 design.md 里的 raw CSS。其他章节（§1-§7）都不能写 CSS——值是值，散文是散文。

---

## design.md 空白模板

新主题直接 cp 这份骨架到 `<my-deck>/design.md` 开始填（`<my-deck>` 是你为这份 deck 选的文件夹）：

```markdown
---
name: <slug>
description: "<一句话主题定位>"
version: 0.1.0
---

# <Theme Name>

> [一句 tagline，如 "YC / TechCrunch 风格的 pitch deck 主题"]

## §1 Atmosphere

[第一段：整体气质 — 画面感、隐喻、情绪、时代感]

[第二段：字的角色 — font family、为什么、weight 用法]

[第三段：色的逻辑 — monochrome / 双色 / 多色？accent 克制还是奔放？]

[第四段（可选）：装饰风格 — 装饰元素、空间逻辑、特殊技法]

**Key Characteristics**:
- [5-10 条特征 bullet]

## §2 Palette values

### Background Surfaces
- `--background` — page canvas
  - <variant1>: `<hex>`
  - <variant2>: `<hex>`
- ...

### Text & Content
- `--foreground` — primary text
  - ...
- `--muted-foreground` — secondary text
  - ...

### Brand & Accent
- `--primary` — ...
- `--accent` — ...

### Status
- `--destructive` — ...

### Border
- `--border` — ...

### Chrome Runtime（跨 variant 不变）
- `--canvas-bg` — `<hex>`
- `--chrome-bg` — `<hex>`
- `--chrome-fg` — `var(--foreground)`
- `--chrome-muted-fg` — `var(--muted-foreground)`
- `--chrome-border` — `var(--border)`
- `--chrome-accent` — `var(--accent)`
- `--chrome-accent-fg` — `var(--accent-foreground)`

## §3 Typography values

### Font Family
- **Primary**: `<family>`, [fallback chain 含中文]
- **Mono**: `<family>`, [fallback chain]

### Size Scale
- `--text-xs`   `<value>`  — [use case]
- `--text-sm`   `<value>`  — [use case]
- `--text-md`   `<value>`  — [use case]
- `--text-lg`   `<value>`  — [use case]
- `--text-xl`   `<value>`  — [use case]
- `--text-2xl`  `<value>`  — [use case]
- `--text-3xl`  `<value>`  — [use case]
- `--text-4xl`  `<value>`  — [use case]

### Weight Scale
- `--weight-normal`    `<n>`
- `--weight-medium`    `<n>`
- `--weight-semibold`  `<n>`
- `--weight-bold`      `<n>`

### Leading + Tracking
- Leading: tight / snug / normal / body — `<v>` / ...
- Tracking: tight / snug / normal / wide — `<v>` / ...

### Principles
- [3-5 条 use 原则]

## §4 Shape values

### Radius
- `--radius-sm`     `<v>`
- `--radius`        `<v>`
- `--radius-full`   `<v>`

### Space
- `--space-1`..`--space-32` — `<v>`...

### Grid（如果主题有）
- columns / rows / margins / gutters / track-*

### Shadow
- `--shadow-sm` / `--shadow` — `<v>`

## §5 Variants
- **<variant1>** — [何时用]
- **<variant2>** — [何时用]
- **<variantN>** — [何时用]

## §6 Decorative signature

### <名字 1>
- 是什么 / 几何 / 用法 / HTML 模板（如果有）

### <名字 N>
- ...

## §7 Red Lines
- **<禁忌>** — [理由]

## §8 Append CSS

```css
/* 1. Chrome 8 锚点位置（如果 §6 chrome ring 想可见） */
.chrome-top-left    { /* top / left / font / color */ }
.chrome-top-right   { /* top / right / font / color */ }
.chrome-rule        { /* border-top / margin / height */ }
.chrome-bottom-left { /* bottom / left / font / color */ }
.chrome-bottom-right{ /* bottom / right / font / color */ }

/* 2. .page-shell 内边距 / overflow */
.page-shell { /* position / padding / overflow */ }

/* 3. .page-content 容器 + grid 模式（如果 §4 有 Grid） */
.page-content { /* display: grid; columns/rows/gap */ }
.page-content[data-layout="flow"] { /* flow override */ }

/* 4. Utility classes（可选 — 仅当用 strict grid） */
/* .h1-9 { grid-column: 1 / 9; }
   .v3-8 { grid-row: 3 / 9; } */

/* 5. Component-level overrides（如果 §6 Decorative 有声明） */

/* 6. Decorative motif inline CSS（如果 §6 用了 M18/M20 等装饰） */
```
```

---

## Constraints（不可违反）

- **design.md 只有一处放 CSS** —— §8 Append CSS 段。其他章节（§1-§7）只放值 + prose
- **token 命名对齐 shadcn**（15 个标准名锁死）
- **不设 component 二级 alias**（不要 `--card-bg / --card-padding-x`）
- **每段 Atmosphere 必须有具体值锚定**（不允许 floating prose 像"现代清爽"）
- **palette 必须列全 variant × token 矩阵**——chrome 模板有 14 token × N variant，缺 1 行 CSS 漏 1 行
- **chrome runtime 7 token 在 §2 末尾必填**（含 `--canvas-bg / --chrome-*`）
- **§8 Append CSS 段顺序固定 6 槽**（chrome 锚点 → page-shell → page-content → utility → component overrides → motif），按需填，无内容的留空注释

## Why

- **散文 + 表格** 让 AI 既能"凭感觉"模仿主题写新页面（读 §1 Atmosphere），也能精确取值（读 §2-§4）
- **CSS 不在 design.md** 防止"手改值但忘改 CSS"漂移；§8 是单一变换路径
- **Atmosphere 强制 4 段** 逼 AI 不能糊弄，必须把 felt sense 画完整
- **Decorative signature 单独章节**——以前散落在 motif/redlines 里的视觉系统，现在系统化讲解
- **空白模板**给所有新主题一致起点，避免每次"从空白开始"的格式漂移
