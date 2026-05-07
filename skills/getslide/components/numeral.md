---
name: numeral
deps: []
tokens:
  - --foreground
  - --muted-foreground
  - --accent
  - --destructive
  - --text-3xl
  - --text-sm
  - --weight-semibold
  - --tracking-tight
  - --leading-tight
  - --space-2
source: "原创（slide 场景特有；shadcn 无对应）"
---

# Numeral

突出展示一个数据点：大数字 + 可选趋势行。常用于 stat / KPI / 引用页核心数字。

## Template

```html
<div class="numeral" data-slot="numeral" data-trend="up">
  <div class="numeral-value" data-slot="numeral-value">{value}</div>
  <div class="numeral-trend" data-slot="numeral-trend">{trend-text}</div>  <!-- 可选 -->
</div>
```

`data-trend`: `up | down | flat | none`，决定 trend 文字色（none 不渲染 trend）。

## Style

```css
.numeral {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
.numeral-value {
  font-size:        var(--text-3xl);
  font-weight:      var(--weight-semibold);
  color:            var(--foreground);
  letter-spacing:   var(--tracking-tight);
  line-height:      var(--leading-tight);
  font-variant-numeric: tabular-nums;
}
.numeral-trend {
  font-size: var(--text-sm);
  color: var(--muted-foreground);
}
.numeral[data-trend="up"]   .numeral-trend { color: var(--accent); }
.numeral[data-trend="down"] .numeral-trend { color: var(--destructive); }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `numeral-value` | 是 | 数字主体（128.4k / 3.42% / 7× 等） |
| `numeral-trend` | 否 | 趋势文本（↗ 12.3% vs 上月） |

## Constraints

- value 行**总是** `tabular-nums`——保证千位对齐，多 numeral 同行时不抖动
- 默认字号 `--text-3xl`；hero 页可在使用处 inline 写 `style="font-size: var(--text-4xl)"` 放大到 88-120px
- 主题想改默认字号：在 spec 里写 `.numeral-value { font-size: var(--text-2xl) }` 覆盖

## Variants（通过字号 inline 调整）

| 用途 | 推荐字号 |
|---|---|
| 卡内 stat | `var(--text-2xl)` |
| 默认 | `var(--text-3xl)` |
| Hero 强调 | `var(--text-4xl)` |
| 全屏数字 | `var(--text-5xl)` |

## Why

- shadcn 无对应组件——slide 场景特有需求（KPI 展示）
- 趋势色绑定到语义（up/down/flat）而非具体颜色——up 用 `--accent`，down 用 `--destructive`，主题可在 palette 层换值
- value/trend 共享 `--text-sm` 等 scale token，主题改 scale 即换风格
