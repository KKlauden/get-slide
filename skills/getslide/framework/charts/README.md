# framework/charts/

**Chart SVG 转写参考**——不是必须用的 component 库。

## 为什么留下这 3 份

`components/` 和 `blocks/` 全删了——card / button / hero / feature-grid 这种基础 UI 现代 LLM 自己写没问题。但 **chart SVG 是数据可视化**，有数学几何（弧度 / 比例 / marker 位置 / tabular nums 对齐），AI 凭空写容易翻车。

这 3 份当**参考 pattern**留着，AI 写 chart 时翻它们做 reference，不是强制使用：

- **`chart-line-default.md`** — 多 series line chart 转写规范（来自 `@shadcn/chart-line-default`）。包含 dot / area-fill / annotation-pill / 注释箭头等 motif。是写 line chart 时的样板
- **`bar-chart.md`** — 垂直 bars + height-only fill。常见用法：metric panel 横向比较、stage progression 4 列纵向 fill
- **`chart-gauge.md`** — 50 bar gauge + percent marker + 0/50/100% labels。常见用法：覆盖率 / 进度 / 完成度

## 要不要在 deck 里直接用

看情况：

- **直接 inline 进 deck.html** — 复杂主题（archive coverage panel、pitch P7 chart narrative）需要 chart 时这样做。把 chart .md 里的 SVG 模板 + CSS 整段贴到 deck 的 § COMPONENT CSS / inline 里，按 design.md 主题色 override
- **学完自己写** — 简单 sparkline / 单一数据条不需要这些

## 写新 chart 的流程

如果 deck 需要 chart，且这 3 份都不 fit：

1. 用 `npx shadcn@latest view @shadcn/chart-<X>-<variant>` 拿源（React + Tailwind）
2. 转写为 HTML + inline SVG + CSS（CSS 只用 `var(--xxx)`，吃 design.md token）
3. 直接 inline 到 deck.html，**不**抽到 `framework/charts/` ——某 chart 跨 ≥2 deck 反复用再抽

## 不在这里的东西

UI component（card / button / numeral / hero / feature-grid 等）——AI 自己写。
