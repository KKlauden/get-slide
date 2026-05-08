---
name: bar-chart
deps: []
tokens:
  - --foreground
source: "原创（从 archive sample 抽取——P4 widths-varying / P9 heights-varying 两种用法都 cover）"
related: ["chart-gauge (composes bar-chart for linear gauge)"]
---

# Bar Chart

垂直 bars 横向 row。**纯骨架**——宽度/高度/数量/颜色全部由消费者通过 CSS var 或 inline 控制；零风格预设，跨主题通用。

## Template

```html
<!-- 案例 1：widths varying（"rhythm"），heights uniform -->
<div class="bar-chart">
  <div class="bar" style="width: 12px"></div>
  <div class="bar" style="width: 12px"></div>
  <div class="bar" style="width: 6px"></div>
  <!-- ... -->
</div>

<!-- 案例 2：heights varying，widths uniform（用 --bar-w 一次设全） -->
<div class="bar-chart" style="--bar-w: 10px; --bar-fill: 28%">
  <div class="bar"></div>
  <!-- ... × N -->
</div>

<!-- 案例 3：dim modifier 区分 active/inactive bars（如 gauge 之前/之后） -->
<div class="bar-chart" style="--bar-w: 2px">
  <div class="bar"></div>          <!-- 亮 -->
  <div class="bar dim"></div>      <!-- 暗 -->
</div>
```

## Style

```css
.bar-chart {
  display: flex;
  align-items: flex-end;                   /* 底对齐——heights 不同时一律从底往上长 */
  justify-content: space-between;          /* bars 撑满容器全宽 */
  height: var(--bar-chart-h, 80px);
}
.bar-chart .bar {
  width: var(--bar-w, 6px);
  height: var(--bar-fill, 100%);
  background: var(--bar-color, var(--foreground));
}
.bar-chart .bar.dim {
  opacity: 0.3;                            /* 通用暗淡 modifier */
}
```

## Slots / Vars

| 名字 | 类型 | 默认 | 说明 |
|---|---|---|---|
| `bar-chart`     | class    | — | 容器 |
| `bar`           | class    | — | 单条 bar（直接 `.bar-chart` 子元素） |
| `bar.dim`       | modifier | — | 暗淡 bar（用于 gauge / past-vs-current 区分） |
| `--bar-chart-h` | CSS var  | `80px` | 容器高度 |
| `--bar-w`       | CSS var  | `6px`  | bar 宽度（容器级设；bar inline `style="width:"` 优先级更高） |
| `--bar-fill`    | CSS var  | `100%` | bar 高度（百分比 of 容器高） |
| `--bar-color`   | CSS var  | `var(--foreground)` | bar 颜色 |

## Constraints

- 容器**必须**有自己的宽度（block flex 需要 known width 才能 space-between 分布 bars）。父级用 grid / abs / fixed-width 都行
- bars 数量由 HTML 里 `.bar` 子元素决定，CSS 不约束。**保持 ≤ 60 条**——再多视觉上单条 < 1px 失真
- `.dim` 默认用 `opacity` 实现；要单独控制 dim 颜色可在主题 spec 加 `.bar-chart .bar.dim { background: ...; opacity: 1; }`

## Why

- **零风格预设**——不带圆角、不带 padding、不带阴影，其他主题（圆角软风、insurance heavy 等）拿来用一样合适
- 三个 var 覆盖了实际用得到的所有 axis（height / width / fill / color），其余视觉细节在主题 spec 用更具体的 selector（如 `.archive-stage-col .bar-chart`）锁定
- bars 用 `flex space-between` 而不是 `gap`——bars 自动撑满容器全宽，无须算 gap 大小（任意 N 条都贴边对齐）
