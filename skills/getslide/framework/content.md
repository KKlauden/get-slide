---
name: content
role: framework-skeleton
description: 内容结构契约——教 AI 怎么写新的 content 文档（per-deck 一份）
---

# Content — 内容结构契约

> framework 三件套之一：**runtime / design / content**。这份不是某份 deck 的内容，是"怎么写 deck 大纲"的标准。
>
> 每份新 deck 在自己的项目文件夹下放一份 `content.md`（**不**强制 samples/ 下；AI 在用户选的位置建文件夹）。本文 §1 schema、§3 notes 三规、§4 data-title 是**强制**；§2 节奏 / §6 模板是**指南**。

## 这份文档做什么

内容层是 deck 大纲层——一份 deck 有哪几页、各页用什么排布形状、各页填什么文字、演讲者怎么讲。它**不**决定主题视觉（那是 design.md 的事），**不**决定基础 UI 怎么写（那是 AI 直接写 HTML+CSS 吃 design.md token 的事，参考 sample 学 abs paradigm）。

每份 deck 的 `content.md` 是**给 AI 填 framework/deck.html 的指令清单**：
- frontmatter：deck 元数据 + 主题指针
- 每页一个 H2 段：`block` / `content` / `styling` / `notes` 四子段

---

## §1 Schema（强制）

### Frontmatter

```yaml
---
title:    <deck title>             # 必填
author:   <name>                   # 必填
date:     YYYY-MM-DD               # 必填，写绝对日期不写"今天"
theme:    ./design.md              # 必填，指向同文件夹的 design.md
brand:    "<品牌名>"               # 选填，chrome 锚点用
brand-letter: "<单字母>"           # 选填，logo-mark 用（如 "P"）
copyright: "<版权字符串>"          # 选填，可含 <br>
---
```

### Per-page H2 + 4 子段

每页一个 `## P<N> — <Title>` 标题，下面带 4 子段（**bullet 格式，自由形态**）：

```markdown
## P1 — Cover

- **block**: <block 名>(可链 + chrome 锚点) [+ <子 block>]
- **variant**: <variant 名>           # 对应 design.md §5 variant
- **grid 模式**: strict | flow        # strict = 用 12×8 网格，flow = 自由 abs 钉位
- **content**:
  - title: `<文本>`
  - lede:  `<文本>`
  - <其他字段，按 block 需求>
- **placement**:                      # 仅 strict 模式 / 复杂 abs 时填
  - <element>: `.h1-9 .v6-7` 或 `top:-300; right:-237; ...`
- **chrome**:
  - top-left:     `<填充内容 / SVG / 多行 <br>>`
  - top-right:    `<...>`
  - bottom-left:  `<...>`
  - bottom-right: `<...>`
- **chrome-rule**: default | top | off    # 顶部 hairline 位置，不写 = default
- **notes**: |
    [演讲者逐字稿，150-300 字，遵循 §3 三规]
```

### 子段含义

| 子段 | 必填 | 说明 |
|---|---|---|
| `block` | ✅ | 主 block 名（参考 §5 索引）+ 可选附属 block 链接（"+ ambient-frames"）|
| `variant` | ✅ | 对应 design.md §5 声明的 variant 之一 |
| `grid 模式` | ✅ | strict（12×8 grid 命中 track）/ flow（自由 abs / 普通文档流）|
| `content` | ✅ | 该页 block 需要的文本/数据点；字段名跟 block .md 的 slot 对应 |
| `placement` | ⚪ | strict 模式下用 grid track 简记法（`.h1-9 .v6-7`），flow 模式下用 abs px 写法 |
| `chrome` | ✅ | 8 锚点中实际填充的（不填的写"空"或省略行）|
| `chrome-rule` | ⚪ | default / top / off 三选一，不写 = default |
| `styling` | ⚪ | token 覆盖（`font-size: 64px` 等）；用 design.md token 不写 hex |
| `notes` | ✅ | 演讲者逐字稿，150-300 字 |

### 字段值约定

- **文本字段一律 backtick 包裹**：`` ` `` `<br>` 算文本一部分
- **路径 / 简记法用代码 fmt**：``\`top: 280, left, width 700\``
- **grid track 简记法**：`.h<a>-<b> .v<c>-<d>`（h = horizontal track，v = vertical）
- **abs 钉位**：用 px 整数，多个属性用 `;` 隔开

---

## §2 页数 / 叙事节奏（指南）

### 9 页 pitch deck（标准）

```
1. Cover     —— 主题钩子 + 一行 lede
2. Contents  —— 目录（3-5 项）
3. About     —— 是谁 / 在做什么
4. Problem   —— 问题陈述 + 数据点
5. Solution  —— 方案概览
6. Solution sub —— 方案细节 / 数据
7. Performance —— 关键指标
8. Market    —— 市场 / 全球数据（可叠 dark 渐变）
9. Closing   —— 收尾呼吁
```

### 5 页快讲

```
1. Cover
2. Problem
3. Solution（含数据）
4. Validation / Stats
5. CTA / Closing
```

### 3 页 mini

```
1. Cover
2. Hero point（含数据）
3. Closing
```

### Variant 节奏建议

- **冲击点用 dark / accent variant**——cover、closing、关键数据页
- **常规 body 用 default variant**（lite / 浅色）
- **过渡 / 情绪页用第三 variant**（如 forest）
- 变体不要过密——9 页里 dark 不超过 3 页，否则失冲击

---

## §3 Notes 写作三规（强制）

每页 `notes` 子段写演讲者逐字稿。**三规缺一不可**。

### 规则一：不是讲稿，是提示信号

❌ **错**（写成完整讲稿，演讲者会念）：
> 大家好，我叫张三，今天给大家介绍 FAULTLINE。FAULTLINE 是一家做地质监测的公司，我们的使命是为全球关键基础设施提供实时风险情报。我们已经在 47 个国家部署了超过 14,000 台传感器……

✅ **对**（关键词 + 过渡句，给演讲者钩子）：
> 开场白：今天介绍 **FAULTLINE**——全球关键基础设施的**实时地质风险情报网络**。
>
> 先做个类比：所有大型工程都有传感器，但读数是**孤岛式**的——大坝、桥梁、地铁各自为政，没人做横向关联。
>
> 接下来用三个数据点说明**规模**，然后看产品落地，最后用全球部署收尾。

**核心特征**：
- 关键名词 + 数字 **加粗** 当眼神落点
- 过渡句独立成段（"接下来…"、"先说…"）
- 不写完整句子的中间填充——演讲者自己接

### 规则二：每页 150-300 字（2-3 分钟节奏）

太短演讲者抓不住要点；太长成讲稿。中文 200 字、英文 150 词上下最舒服。

### 规则三：用口语，不用书面语

| ❌ 书面 | ✅ 口语 |
|---|---|
| 因此 | 所以 |
| 该方案 | 这个方案 |
| 综上所述 | 总的来说 |
| 鉴于 | 考虑到 |
| 旨在 | 想 |
| 我们将于…… | 我们打算 / 我们要 |

口语也意味着：短句优先（< 25 字一句），主谓宾清楚不绕。

### 范本

```yaml
notes: |
  收尾页：用三个递进的**数量级**收束——LOCAL 230 台、REGIONAL 14K 台、PLANETARY **1.4M 行星网格**。

  每个数量级背后是不同的技术挑战：LOCAL 解决"有没有"，REGIONAL 解决"互联互通"，PLANETARY 解决"全球统一观测"。

  我们今天处于 REGIONAL 的中段。对 FAULTLINE 在做的事感兴趣的话，下来聊。**谢谢。**
```

---

## §4 data-title 强制

每个 page-shell 必须有 `data-title="..."`——O 键 overview 网格 + S 键 presenter NEXT 卡片靠这个。

content.md H2 标题（`## P1 — Cover`）的 "— " 之后部分**就是** `data-title` 的值。AI 拼 sample 时直接抄。

例：
- `## P1 — Cover` → `data-title="Cover"`
- `## P3 — About FAULTLINE` → `data-title="About FAULTLINE"`
- `## P6 — Sensor Performance` → `data-title="Sensor Performance"`

**不要**写 emoji、过长（>30 字符）、含特殊字符的 title——overview 卡片宽度有限。

---

## §5 `block:` 子段写什么

v2.1 已**砍掉** `components/` + `blocks/` 库——基础 UI（card / button / hero / numeral / pill / cards-row / feature-grid 等）由 AI 直接写 HTML+CSS（吃 design.md token）。

`block:` 子段写**这一页的「内容形状描述」**——给 AI 看出这页要排什么样，不绑定具体 class 名：

| 内容形状 | block: 子段写法举例 |
|---|---|
| 大字 hero + lede | `hero (centered title + lede)` 或 `hero-bottom-anchored (88px title)` |
| 多列对比 | `2-up compare (left frame A vs right frame B)` 或 `4-up zig-zag compare` |
| 编号 list | `numbered-list (3 rows: prefix / label / desc, hairline 分隔)` |
| 大数字 stat | `mega-stat (200px num + caption)` 或 `3-up stat-cards` |
| Quote 页 | `pull-quote-only (centered serif italic, 大字)` |
| Chart | `chart-line (timeline + 2 series)` 或 `chart-gauge (73% with marker)` |
| Cover | `cover (大字 hero + ambient motif + chrome)` |
| Closing | `closing (recap 三句 + contact)` |

**chart 子段**：如果用 chart，参考 `framework/charts/` 里 3 份 SVG pattern。其他 UI AI 自己写，参考 `samples/<theme>/<theme>.html` 看主题怎么实现。

---

## §6 常用页模板（指南）

### Cover
```markdown
- **block**: hero-block + ambient-frames
- **variant**: <封面 variant>
- **grid 模式**: strict
- **content**:
  - title: `<两行 hook>`
  - lede:  `<一行 tagline>`
- **placement**:
  - hero-block: `.h1-9 .v6-7`
- **chrome**:
  - top-left: `<logo + brand>`
  - top-right: `<copyright>`
  - bottom-right: `01`
- **notes**: |
  [开场白 — 介绍主题 + 听众期待]
```

### Contents / TOC
```markdown
- **block**: <toc / cards-row / 自定义>
- **variant**: lite
- **content**:
  - 3-5 项目录条目，每项 num + label
- **chrome**: 同其他页结构
- **notes**: [一句过渡："今天会讲三件事……"]
```

### Stat / Hero number
```markdown
- **block**: stat-split / numeral
- **variant**: <强调 variant，dark/accent>
- **content**:
  - number: `92%`
  - label: `<one-line context>`
- **notes**: [数字背后的一句叙事]
```

### Chart narrative
```markdown
- **block**: chart-card + <chart-line-default 等>
- **variant**: lite or accent
- **content**:
  - title: `<chart title>`
  - data: [12, 18, 27, 38, 51, 64, 76, 86]
  - annotation: `↗ 7× baseline`
- **notes**: [chart 看点 + 解读]
```

### Closing / CTA
```markdown
- **block**: hero-block + contact-list (optional)
- **variant**: lite or accent
- **content**:
  - title: `<收尾钩子>`
  - body:  `<下一步行动>`
- **notes**: [收尾词 — "谢谢" / "下来聊" / 问答邀请]
```

---

## §7 content.md 空白模板

新 deck 直接 cp 这份骨架到 `<my-deck>/content.md` 开始填（`<my-deck>` 是你为这份 deck 选的文件夹）：

```markdown
---
title:        <deck title>
author:       <name>
date:         YYYY-MM-DD
theme:        ./design.md
brand:        "<品牌>"
brand-letter: "<X>"
copyright:    "<copyright string>"
---

# <Deck Name>

[一段叙事 — 这份 deck 是什么、对谁讲、变体节奏]

**Variant 节奏**（N 页）：v1 → v2 → v3 → ...
**叙事线**: cover → ... → closing

---

## P1 — Cover
- **block**: hero-block + ambient-frames
- **variant**: <v>
- **grid 模式**: strict
- **content**:
  - title: `<...>`
  - lede:  `<...>`
- **placement**:
  - hero-block: `.h1-9 .v6-7`
- **chrome**:
  - top-left: `<...>`
  - top-right: `<...>`
  - bottom-right: `01`
- **notes**: |
    [开场白 150-300 字]

## P2 — <Title>
...

## P9 — Closing
...
```

---

## Constraints（不可违反）

- **每页必有 4 子段**：block / variant / content / chrome（chrome 子项可写"空"，但 8 锚点必须显式声明每个的状态）
- **每页必有 notes**——不写默认空字符串，但 H2 段必须有 `notes:` 子段（即使空也得有，提醒后期补）
- **theme 指向 ./design.md**——不写绝对路径不写其他主题文件
- **block 字段不能为空**——每页必须有主 block 名
- **token 覆盖在 styling 子段写**——不在 content 子段混入 CSS
- **H2 标题 = data-title**——拼 sample 时 1:1 抄到 page-shell

## Why

- **每页 4 子段**逼 AI 把"用什么砖、装什么内容、放哪里、怎么讲"一次性想清楚——避免半成品页
- **notes 强制**让演讲者模式 (S 键) 一开始就有数据；没逐字稿可以留空但**段必须存在**
- **H2 = data-title**保证 overview 网格、presenter 卡片字段全 9 页有内容
- **frontmatter theme 指针**让一份 content.md 可换皮——`theme: ./design.md` 改路径就能跑另一套主题
- **页数节奏 / variant 节奏**给的是建议不是死规——每个 deck 自己叙事，本文档负责 **AI 不会写出 0 页 cover 或 12 页全 dark** 这种破节奏
