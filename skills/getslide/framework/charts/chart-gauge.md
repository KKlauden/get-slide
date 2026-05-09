---
name: chart-gauge
kind: pattern-contract
purpose: "线性 gauge：N 个离散 bar 进度 + label 区间 + 三角 marker。结构 + 几何契约，不是 CSS 模板。"
---

# Chart Gauge Pattern

> **这是结构 + 几何契约，不是 CSS 模板。** AI 翻这份学：gauge 由什么部件组成 / marker 几何怎么对齐。视觉细节（颜色 / marker 形状 / label 字 / 圆角）**必须**按当前 deck 的 design.md token 自己写。

## 适用场景

**单一进度值**可视化（"X% coverage" / "X% completion" / "X% adoption"）。靠 N 个离散 bar 颗粒 + marker 三角指当前位置。

**不**适用：
- 真实数据 chart（每条 bar 反映独立值）→ 用 `bar-chart.md`
- 圆形 / 半圆 gauge → SVG arc，自己写

## 必填结构

```text
<container.chart-gauge>
  ├── label 区间（顶或左右标 0% / 中 / 100%）
  ├── <gauge 主体>
  │    ├── N 个离散 bar（默认 50，可 25-100）
  │    │    ├── 亮 bars × M     ← 当前进度之前
  │    │    └── 暗 bars × (N-M) ← 当前进度之后
  │    └── marker（三角 / 圆点 / 竖线 等，指 marker_pos%）
  └── 当前数值大字（可选，钉在 marker_x 之上）
```

## 几何对齐铁律

```text
total_bars       = 50（推荐；≥ 25 才有颗粒感，≤ 100 才不糊）
marker_pos_pct   = data_value / max_value × 100         例：73 / 100 = 73%

bright_bars      = round(total_bars × marker_pos_pct / 100)   例：73% × 50 = 36.5 → 37
dim_bars         = total_bars - bright_bars                   例：50 - 37 = 13

marker_x_offset  = marker_pos_pct % of track_width
```

**视觉对齐铁律**：marker 三角必须落在「亮/暗 cutover」位置 ± 1 个 bar 宽度内。如果 bright_bars 算出 37 但 marker 视觉落在 33%，算错或 css 漂移。

## 尺寸算法

```text
gauge_track_height  = 48-72px（按页内空间）
gap                 = 1-2px
bar_width           = (track_width - (total_bars - 1) × gap) / total_bars
                      ≈ 1-3px（足够多 bar 时颗粒感强）
```

## 必须按 design.md 决定的视觉

| 决策 | 查 design.md |
|---|---|
| 亮 bar **颜色** | §2 Palette `var(--accent)` / `var(--primary)` / `var(--foreground)` |
| 暗 bar **颜色** | `var(--muted)` 或亮 bar 同色 + `opacity: 0.3` |
| marker **颜色** | 跟亮 bar 同 token（视觉锚一致） |
| marker **形状** | 三角（默认 CSS triangle）/ 圆点 / 竖线 / 锚点 ——按主题装饰系统选 |
| label **字** | §3 Typography（archive 风 = mono uppercase；CHASSAN 风 = serif italic + sans 混排） |
| label **排布** | flex `space-between`（左 0% / 中 / 右 100%），或竖排在 gauge 右侧 |
| bar **圆角** | §4 Shape `--radius-sm` 或 0（archive 锐角 = 0；圆润主题 1-2px） |
| **数值大字（如有）** | §3 Typography hero size，钉在 marker_x 位置上方 |

## 反例

❌ marker 位置跟 bright/dim cutover **数学不对齐** —— gauge 失去意义

❌ bars 数量 < 20 —— 颗粒太粗，看不出 gauge 感

❌ 用 1 个连续渐变 bar 代替 N 离散 bar —— 那是 progress bar，不是 gauge

❌ 抄 archive coverage-panel 的 mono uppercase + 白字 + 黑底配色 —— 那是 archive deck-specific 决策

## Why

- gauge 视觉效力来自"颗粒数"——观众能数出 73 vs 75 的区别
- marker = 数据真实位置；bars 颗粒 ≠ marker（颗粒是离散近似），所以两者必须**视觉上对齐**
- N=50 是 archive sample 实测的甜点数（颗粒清晰 + bar 不会太细）；可调
