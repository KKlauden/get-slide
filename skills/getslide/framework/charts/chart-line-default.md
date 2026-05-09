---
name: chart-line-default
deps: []
tokens:
  - --foreground
  - --muted-foreground
  - --background
  - --border
categories:
  - charts
  - charts-line
kind: reference
source: "@shadcn/chart-line-default — adapted: React+Recharts → SVG-only (v2 self-contained 约束)"
---

# Chart Line Default

> **定位**：v2 chart 转写规范的样板（reference），不是 slot-fillable 砖块。写新 chart 时翻这份当模板，**直接 inline 到 deck sample HTML 里**，不抽组件——除非某 chart 跨 ≥2 deck 反复用，再提取。

`@shadcn/chart-line-default` 的 v2 适配版。shadcn 原版基于 Recharts (React 库) 不能 self-contained，本实现保留**命名 + 类别 + 数据语义**，实现层换成纯 SVG。

折线图 — 输出 1664×400 viewBox 的纯 SVG，由 `chart-card` block 包装。包含网格线 / x-y 轴 label / primary + secondary 折线 / 面积填充 / 数据点 / 注释 pill。

## Template

```html
<svg viewBox="0 0 1664 400" preserveAspectRatio="xMidYMid meet" aria-label="{标题}">
  <!-- horizontal grid · hairline 1px dashed -->
  <g stroke="var(--border)" stroke-dasharray="3 5">
    <line x1="60" y1="24"  x2="1640" y2="24"  />
    <line x1="60" y1="106" x2="1640" y2="106" />
    <line x1="60" y1="188" x2="1640" y2="188" />
    <line x1="60" y1="270" x2="1640" y2="270" />
    <line x1="60" y1="352" x2="1640" y2="352" />
  </g>

  <!-- y-axis labels -->
  <g fill="var(--muted-foreground)" font-size="14" text-anchor="end">
    <text x="48" y="28">100</text>
    <text x="48" y="110">75</text>
    <text x="48" y="192">50</text>
    <text x="48" y="274">25</text>
    <text x="48" y="356">0</text>
  </g>

  <!-- x-axis labels -->
  <g fill="var(--muted-foreground)" font-size="14" text-anchor="middle">
    <text x="60"   y="380">Q1</text>
    <text x="286"  y="380">Q2</text>
    <text x="511"  y="380">Q3</text>
    <text x="737"  y="380">Q4</text>
    <text x="963"  y="380">Q5</text>
    <text x="1189" y="380">Q6</text>
    <text x="1414" y="380">Q7</text>
    <text x="1640" y="380">Q8</text>
  </g>

  <!-- area fill under primary · fg @ 0.08 -->
  <path d="M 60 326 L 286 306 L 511 280 L 737 247 L 963 201 L 1189 149 L 1414 90 L 1640 40 L 1640 352 L 60 352 Z"
        fill="var(--foreground)" fill-opacity="0.08" />

  <!-- secondary line · muted 2px -->
  <path d="M 60 286 L 286 280 L 511 270 L 737 260 L 963 247 L 1189 234 L 1414 221 L 1640 204"
        stroke="var(--muted-foreground)" stroke-width="2" fill="none" />

  <!-- primary line · fg 2px -->
  <path d="M 60 326 L 286 306 L 511 280 L 737 247 L 963 201 L 1189 149 L 1414 90 L 1640 40"
        stroke="var(--foreground)" stroke-width="2" fill="none" />

  <!-- data points · fg 5px circles, slide-bg ring -->
  <g fill="var(--foreground)" stroke="var(--background)" stroke-width="2">
    <circle cx="60"   cy="326" r="5" />
    <circle cx="286"  cy="306" r="5" />
    <circle cx="511"  cy="280" r="5" />
    <circle cx="737"  cy="247" r="5" />
    <circle cx="963"  cy="201" r="5" />
    <circle cx="1189" cy="149" r="5" />
    <circle cx="1414" cy="90"  r="5" />
    <circle cx="1640" cy="40"  r="5" />
  </g>

  <!-- annotation pill at inflection point -->
  <g>
    <line x1="1414" y1="90" x2="1414" y2="56"
          stroke="var(--border)" stroke-dasharray="2 3" />
    <rect x="1349" y="20" width="130" height="32" rx="16"
          fill="var(--foreground)" fill-opacity="0.14"
          stroke="var(--border)" />
    <text x="1414" y="41" fill="var(--foreground)"
          font-size="14" text-anchor="middle">↗ 7× baseline</text>
  </g>
</svg>
```

## Style

无独立 CSS — SVG 内联 `var()` 引用 token，渲染由 chart-card / page-shell variant 决定。

## Constraints

- viewBox 必须 1664×400（chart-card 容器尺寸约束）
- 8 个数据点 × 2 系列（primary + secondary）
- 数据值范围 0-100（y 轴档位 0/25/50/75/100）
- 注释 pill 单个（多个会让眼睛分心）
- 颜色全用 `var(--xxx)` —— variant 切换时图表自动适配

## v2 chart 转写规范（用这份当模板）

写新 chart（bar / pie / area / radar 等）时照下面规则：

1. **viewBox 固定 1664×400**——`chart-card` 容器约束
2. **`preserveAspectRatio="xMidYMid meet"`** + `aria-label="{标题}"`
3. **颜色全部 `var(--xxx)`**：`--foreground`（primary）/ `--muted-foreground`（secondary）/ `--border`（grid/dash）/ `--background`（data point ring）；不写死颜色
4. **网格线**：水平 dashed `stroke-dasharray="3 5"` + `var(--border)`
5. **轴 label**：`font-size="14"` + `var(--muted-foreground)`；y `text-anchor="end"`，x `text-anchor="middle"`
6. **数据点**（line/area）：`circle r="5"` + `fill="var(--foreground)"` + `stroke="var(--background)" stroke-width="2"`（背景色 ring 防底色冲突）
7. **面积填充**：`fill="var(--foreground)" fill-opacity="0.08"`
8. **注释 pill**（可选，单个）：`rect rx="16"` + `fill="var(--foreground)" fill-opacity="0.14"` + 引导虚线
9. **不内嵌 padding/border/legend**——这些归 chart-card 包装
10. **数据 hard-code 在 SVG 里**：v2 没有 build 工具，坐标手算（或 Python 一次性生成后贴）

## Why

- chart 数据 hard-code 在 SVG 里 — 没有 build 工具时这是唯一可行做法
- 只输出 SVG（无 padding/border/legend），让 chart-card block 包装；解耦"数据"和"容器视觉"
- 8 数据点 / 2 系列是 pitch deck 的"单页可读图表"上限（更多点画面拥挤）
- 此文件作为 v2 chart 的转写规范样板：每写一种新 chart，都照上面 10 条规则——保证不同 chart 在同一 deck 视觉一致
