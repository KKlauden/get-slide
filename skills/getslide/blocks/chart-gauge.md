---
name: chart-gauge
deps: ["bar-chart"]
tokens:
  - --foreground
source: "原创（从 archive P6 coverage-gauge 抽取并 token 化清理——剥离 mono caps + 白色硬编码）"
related: ["bar-chart (composed inside)"]
---

# Chart Gauge

线性进度量表：`0%` / `中` / `100%` 三 label + 离散 bars 进度条 + 三角 marker 指示当前值。常用于 "X% coverage" / "X% completion" 数据点。

**风格中立**：bars 颜色、marker 颜色、label 字体都用 token，跟随主题。

## Template

```html
<div class="chart-gauge" style="--marker-pos: 73%">
  <div class="gauge-labels">
    <span>0%</span>
    <span>50%</span>
    <span>100%</span>
  </div>
  <div class="gauge-track">
    <div class="bar-chart" style="--bar-w: 2px">
      <div class="bar"></div>          <!-- 亮 bars × N（marker 之前） -->
      <div class="bar dim"></div>      <!-- 暗 bars × M（marker 之后） -->
    </div>
    <div class="gauge-marker"></div>
  </div>
</div>
```

亮/暗 bar 数量根据 marker 位置手动分配（如 73% 对应 50 条里 37 亮 + 13 暗）。

## Style

```css
.chart-gauge {
  width: 100%;
}
.chart-gauge .gauge-labels {
  display: flex;
  justify-content: space-between;          /* 0% 顶左、50% 居中、100% 顶右 */
  margin-bottom: 16px;
  /* 字体 / case / tracking 由主题 spec 决定，block 不预设 */
}
.chart-gauge .gauge-track {
  position: relative;
  height: var(--gauge-h, 64px);
}
.chart-gauge .gauge-track .bar-chart {
  height: 100%;
}
.chart-gauge .gauge-marker {
  position: absolute;
  left: var(--marker-pos, 50%);
  bottom: -16px;                            /* 在 bars 下方，三角朝上指 */
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-bottom: 10px solid var(--bar-color, var(--foreground));
}
```

## Slots / Vars

| 名字 | 类型 | 默认 | 说明 |
|---|---|---|---|
| `chart-gauge`    | class   | — | 外容器 |
| `gauge-labels`   | class   | — | 顶部 0%/中/100% 三 span（typography 由主题决定）|
| `gauge-track`    | class   | — | bars + marker 复合区，relative 定位 |
| `gauge-marker`   | class   | — | 三角形 SVG-less marker（CSS triangle）|
| `--marker-pos`   | CSS var | `50%`  | marker 横向位置（左 % 或 px）|
| `--gauge-h`      | CSS var | `64px` | track 高度 |
| `--bar-color`    | CSS var | `var(--foreground)` | bars + marker 颜色 |

## Constraints

- **gauge 内部组合 bar-chart**——bars 数量、亮暗分布由 HTML 控制；marker `--marker-pos` 必须跟亮/暗 cutover 视觉对齐（数学上 marker_pos% × N_total ≈ N_bright）
- bars 必须 ≥ 20 条（< 20 视觉颗粒太粗，看不出 gauge 效果）
- gauge-labels 的 typography（mono / sans / case / tracking）由主题 spec 在更具体 selector 里锁定，**不**在 block 层硬编码
- 跑在深色背景上（如 archive P6 coverage-panel）：外层容器在自己的 selector 里 override `--foreground: #ffffff`，bars + marker 自动跟着翻白

## Why

- 把 archive 原 `archive-coverage-gauge` 里的 mono caps、`#ffffff` 硬编码、`14px tile bars` 等风格化部分**剥到主题 spec**；block 只保留几何骨架（labels space-between / track relative / marker triangle / bar-chart 嵌入）
- bars 没自己实现，**复用 bar-chart**——避免重复 flex 布局逻辑、未来 bar-chart 升级 gauge 自动跟进
- marker 用 CSS triangle 而不是 SVG——节省 inline SVG 噪音，颜色靠 `border-bottom-color` 跟主题 var
