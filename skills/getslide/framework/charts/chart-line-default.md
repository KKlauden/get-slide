---
name: chart-line-default
kind: pattern-contract
purpose: "Line chart：1-2 series + dot + area-fill + annotation pill。SVG 几何契约 + 数据→坐标算法，不是视觉模板。"
source: "@shadcn/chart-line-default 适配 SVG-only（v2 self-contained 约束）"
---

# Chart Line Default Pattern

> **这是 SVG 几何 + 算法契约，不是视觉模板。** AI 翻这份学：viewBox / 坐标系 / 数据点位置算法。颜色 / 字 / 网格风格 / annotation pill 形状 **必须**按当前 deck 的 design.md 自己写——直接抄 `var(--foreground) fill-opacity 0.08` 这种数字会带来主题脱节。

## 适用场景

**时间 / 顺序序列**（Q1-Q8 月度趋势 / 周度数据 / 实验进展）的 line chart。1-2 series，1 个关键 inflection point 注释。

**不**适用：
- bar 数据（每条独立值）→ `bar-chart.md`
- 单一进度值 → `chart-gauge.md`
- 散点 / 雷达 / 饼图 → 自己写 SVG，参考此份算法和 token 用法

## 必填结构（SVG 内部）

```text
<svg viewBox="0 0 W H">
  ├── grid（水平 dashed N 条横线）
  ├── y 轴 label（左侧，0 / 25 / 50 / 75 / 100 等）
  ├── x 轴 label（底部，时间 / 顺序点）
  ├── area-fill path（primary 之下，半透明面积）   ← 可选
  ├── secondary line path（如有 2nd series，先画）
  ├── primary line path
  ├── data points（圆点 + 背景色 ring，画在 line 之上）
  └── annotation pill（可选，1 个 inflection point）
       ├── 引导虚线（pill ↔ data point）
       ├── pill 背景 + 描边
       └── pill 文字
```

## SVG 几何契约（推荐 1664 × 400 viewBox）

```text
viewBox       = "0 0 1664 400"        ← chart-card 容器约束（archive / pitch 实测）
                                          可改 1280×320 / 2000×500 等，按页内空间

x_axis_left   = 60                    ← 给 y label 留位
x_axis_right  = 1640                  ← 右 padding
plot_width    = 1640 - 60 = 1580

y_axis_top    = 24                    ← 给 annotation pill 留位
y_axis_bottom = 352                   ← 给 x label 留位
plot_height   = 352 - 24 = 328
```

## 数据 → 坐标算法

```text
N points (每 series)              ← 推荐 8 点（pitch P7 实测：单页可读上限）

对每个 data_point i (0-indexed):
  x = x_axis_left + i × (plot_width / (N - 1))
  y = y_axis_bottom - (data_value / max_value) × plot_height

  if value < 0 (allow_negative):
      mid_y      = (y_axis_top + y_axis_bottom) / 2
      half_h     = plot_height / 2
      y          = mid_y - (data_value / max_abs_value) × half_h

grid 横线（推荐 5 条等距）：
  y = y_axis_top + i × plot_height / 4    (i = 0..4)
                                          → 24, 106, 188, 270, 352
```

**例（v3 缪子如果用 line chart）**：
- max = 1000，6 points（注：line chart 8 点最佳，6 点稍稀）
- (10km / 1000) → x=60,    y=24
- (8km  / 560)  → x=376,   y=168
- (0km  / 55)   → x=1640,  y=334

## 必须按 design.md 决定的视觉

| 决策 | 查 design.md |
|---|---|
| primary line **颜色** | §2 Palette `var(--accent)` / `var(--foreground)` |
| primary line **粗细** | 2-3px（archive 锐角风 2px；圆润主题 3-4px） |
| secondary line **颜色** | `var(--muted-foreground)` |
| **area-fill** | `fill="var(--accent)" fill-opacity="?"`（**透明度按主题，不要写死** 0.08——archive 暗底要 0.14，CHASSAN 暖底可能要 0.05-0.08） |
| data point **半径** | 4-7px（按 line 粗细调） |
| data point **ring** | `stroke="var(--background)" stroke-width="2"`（防底色冲突） |
| **grid 线** | dashed `stroke-dasharray="3 5"` + `var(--border)`（archive 风密；CHASSAN 风可省略 grid 只留 hairline baseline） |
| **轴 label 字** | §3 Typography 系统标签字 |
| **轴 label 大小** | 14-16px |
| **annotation pill 形状** | rect rx 跟 §4 Shape `--radius-full` 或 `--radius` 一致 |
| **annotation pill 文字** | 一句不超过 6 词；按主题调性，serif italic / sans uppercase / mono 各异 |

## 反例

❌ **写死颜色**
```svg
<path stroke="#000" />              <!-- 写死 hex -->
<rect fill="#f3f3f3" />             <!-- 写死 grey -->
```

❌ **照抄 `fill-opacity="0.08"` 不思考主题** —— 0.08 在 archive 暗底太弱，CHASSAN 米底可能要 0.05-0.10，按主题调

❌ **8 点之外硬塞** —— 6/10/12 点都行（按数据决定），但**不要**把 12 点挤进 1664 viewBox（拥挤），考虑加宽 viewBox

❌ **多个 annotation pill** —— 1 个就够，多个互相竞争注意力

❌ **省略 data points** —— 只画 line 不画 dot 是 minimal trend chart 风（适合极简主题，但要主题契合）；多数 deck 需要 dot 让数据可读

## Why

- 8 数据点 / 2 series 是 deck 单页 chart 上限——更多点画面拥挤
- v2 没 build 工具，数据手算坐标贴 SVG——文档保证算法对一次
- chart 视觉跟主题走：archive 风 vs CHASSAN 风的 line chart 长相完全不同，文档不预设默认值

## v2 写新 chart（非 line）的转写规范

写新 chart（pie / area / radar / scatter 等）时照下面通用规则：

1. **viewBox 选 1664×400 或同长宽比** —— 跟 line chart 容器一致
2. **`preserveAspectRatio="xMidYMid meet"`** + `aria-label="{标题}"`
3. **颜色全用 `var(--xxx)`**：primary `var(--accent)` / secondary `var(--muted-foreground)` / grid `var(--border)` / data point ring `var(--background)`；**不写死颜色**
4. **轴 label**：`font-size="14-16"` + `var(--muted-foreground)`；y `text-anchor="end"`，x `text-anchor="middle"`
5. **数据点**（line / area）：`circle r="5"` + `fill="var(--accent)"` + `stroke="var(--background)" stroke-width="2"`
6. **不内嵌 padding / border / legend** —— 这些归外层 chart-card / page-shell 包装
7. **数据 hard-code 在 SVG 里** —— v2 没 build 工具，坐标手算（或 Python 一次性生成后贴）
