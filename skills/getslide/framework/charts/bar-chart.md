---
name: bar-chart
kind: pattern-contract
purpose: "垂直 bars chart 的结构 + 数据→像素算法。不是 CSS 模板——视觉决策按 design.md 来。"
---

# Bar Chart Pattern

> **这是结构 + 算法契约，不是 CSS 模板。** AI 翻这份学：用什么结构 / 怎么算高度。视觉细节（圆角 / 字 / 色 / 宽度）**必须**按当前 deck 的 design.md token 自己写——直接抄会带来主题脱节（archive 风 chart 会跟 CHASSAN 风 deck 视觉冲突）。

## 适用场景

每条 bar 高度反映**一个数据值**——真实数据可视化（如缪子海拔存活率 / 月销售额 / 故障次数）。

**不**适用：
- progress / fill 进度图（每条 bar 同高，靠百分比 fill 表示进度）→ 用 `chart-gauge.md`
- "rhythm" 装饰条（archive P4 那种 widths varying 细 bar 行）—— 自己写，不需要算法

## 必填结构

```text
<container.bar-chart>
  ├── chart 标题 + sub（按主题语调写）
  ├── <bars 区>
  │    ├── bars 容器（flex 横向 / grid）
  │    │    └── N 个 bar 单元
  │    │         ├── 单条 bar（高度按下面算法）
  │    │         ├── 数值 label（顶部，可选但推荐）
  │    │         └── 横轴 label（底部）
  │    ├── 横轴 baseline hairline（1px var(--border)）
  │    └── 纵轴 marker（可选：max / mid / 0 标）
  └── caption（数据说明 / 来源 / 解读，按主题语调写）
```

**最少**：bars 区 + 横轴 label + chart 标题或 caption（**不能光秃 N 根 bar**）。

## 数据 → 像素算法（铁律）

```text
container_height = 280-360px（按页内空间决定）
padding_top      ≥ 32px       ← 给数值 label
padding_bottom   ≥ 36px       ← 给横轴 label
drawing_height   = container_height - padding_top - padding_bottom

max_value        = max(data_values)

对每条 bar：
  bar_height = (data_value / max_value) × drawing_height
  if bar_height < 6px:
      bar_height = 6px           ← 视觉兜底，否则数据点消失
```

**例**：data = `[1000, 560, 310, 175, 98, 55]`，container 320px，padding 36+36
→ drawing 248px，max 1000
→ heights = `[248, 138.88, 76.88, 43.4, 24.3, 13.64]` px

**视觉验证**：最矮 / 最高 = 13.64 / 248 = 5.5% 等于 55/1000 = 5.5% ✓ 严格反映数据。

## 必须按 design.md 决定的视觉（**不要在文档抄默认值**）

| 决策 | 查 design.md |
|---|---|
| bar **圆角** | §4 Shape `--radius` / `--radius-sm`（**不要**默认 `border-radius: 6px`——CHASSAN 风可能要 12-20，archive 风要 0） |
| bar **颜色** | §2 Palette `var(--accent)` / `var(--primary)` / `var(--foreground)` 看主题强调系统 |
| bar **宽度** | 跟横轴 label 字宽匹配（32-80px）；6 条粗 bar vs 12 条窄 bar 表达重量不同 |
| 数值 label **字** | §3 Typography 主显示字（hero 同 family，serif / sans 看主题） |
| 数值 label **大小** | 14-22px，跟整页 body 协调 |
| 横轴 label **字** | §3 Typography 系统标签字（archive 风 = mono uppercase；CHASSAN 风 = serif italic 或 sans tracking-wide） |
| **bar 间距** | `flex justify-content: space-between` 或 `gap`，按视觉密度调 |
| **baseline / 网格** | 1px hairline + `var(--border)`；密图可加水平 dashed |

## 反例（**不要这么做**）

❌ 照抄 `--bar-fill: 28%` percent var —— 那是 progress 场景，真实数据每条独立算 px

❌ 写死视觉值
```css
.bar { border-radius: 6px; }              /* 6 是凭空数字，跟主题脱节 */
.bar-x { font-family: "Geist Mono"; }     /* archive 风污染 */
.bar { background: #2563eb; }             /* 破坏 token 体系 */
```

❌ 直接 cp 别的 deck 的 `.bar-chart` CSS 整套 —— 会带 archive / pitch 的视觉决策；从空白写，或 cp 完**逐行**用 design.md token 重写

❌ 省略数值 label 或横轴 label —— 真实数据 chart 没 label = 不知道是什么

❌ bar < 6px 不兜底 —— 视觉消失，数据丢失

## Why

- chart 是**数据可视化**，比例必须严格反映数据，算法不能凭感觉
- chart **视觉跟主题走**——CHASSAN 风的 chart 跟 archive 风的 chart 长相完全不同，文档不预设
- 算法（高度计算 / 兜底）AI 容易算错，文档保证它们对一次
