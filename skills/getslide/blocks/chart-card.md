---
name: chart-card
deps: []
tokens:
  - --foreground
  - --muted-foreground
  - --border
  - --grid-margin-x
  - --grid-margin-bottom
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（含 legend 的图表外壳）"
related: ["chart-line-default (components/, reference 样板)"]
---

# Chart Card

图表外壳 — abs 钉到 chrome-rule 上方 24px，自带 legend 行 + slot 给 inline SVG（照 `components/chart-line-default.md` 转写规范写）。

## Template

```html
<div class="chart-card" data-slot="chart-card">
  <div class="chart-legend" data-slot="chart-legend">
    <span class="chart-legend-item legend-primary">Primary series 名</span>
    <span class="chart-legend-item legend-secondary">Secondary series 名</span>
  </div>
  <!-- inline SVG（照 components/chart-line-default.md 样板写）：viewBox 1664×400 -->
  <svg viewBox="0 0 1664 400" preserveAspectRatio="xMidYMid meet">…</svg>
</div>
```

## Style

```css
.chart-card {
  position: absolute;
  left:   var(--grid-margin-x);
  right:  var(--grid-margin-x);
  bottom: 120px;        /* 96 chrome-rule + 24 breathing */
  padding: 20px 32px 16px;
  background: color-mix(in srgb, var(--foreground) 4%, transparent);
  border: 1px solid var(--border);
  border-radius: 20px;
}
.chart-card > svg {
  display: block;
  width: 100%;
  height: 400px;
}

.chart-legend {
  display: flex;
  gap: 32px;
  margin-bottom: 4px;
  font-size: 14px;
  color: var(--muted-foreground);
}
.chart-legend-item {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}
.chart-legend-item::before {
  content: "";
  width: 24px;
  height: 2px;
}
.chart-legend-item.legend-primary::before   { background: var(--foreground); }
.chart-legend-item.legend-secondary::before { background: var(--muted-foreground); }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `chart-card`   | 是 | 容器 |
| `chart-legend` | 否 | legend 行（含 N 个 `.chart-legend-item.legend-{primary,secondary}`） |
| `<svg>`        | 是 | inline SVG（照 `components/chart-line-default.md` 转写规范） |

## Constraints

- **chart-card 必须直接是 `.page-shell` 的子元素**——不可塞进 `.page-content` 内。原因：`.page-content` 默认无 `position: relative`，chart-card 的 `position: absolute` 会跳过它落到 `.page-shell`，造成 DOM 嵌套语义跟视觉分层不一致；另外 page-content 的 `data-layout="strict"` 网格也不该把 abs 块算入
- SVG 容器固定 1664 × 400（width 1664 = 1920 - 96×2 - 32×2 padding；height 400 = block 内固定档位）
- legend 仅 2 项（primary + secondary）—— 多于 2 项需求该用 bar-chart 而非 chart-line-default

## Why

- 把"chart 视觉外壳"（card + legend）从"chart 数据可视化"（SVG）解耦：外壳是 block，SVG 直接 inline 在 deck sample 里
- SVG 照 `components/chart-line-default.md` 样板写（viewBox 1664×400），不带 padding / border / legend — 这些归 chart-card 包装
- background `var(--foreground) 4%` 半透明叠层：lite/dark/forest 三 variant 下都形成"略凹"的 chart 容器
