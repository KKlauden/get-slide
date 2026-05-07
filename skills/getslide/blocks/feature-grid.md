---
name: feature-grid
deps:
  - pitch-card
tokens:
  - --grid-margin-x
  - --grid-margin-bottom
  - --space-8
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（5 卡 2x3 错位网格）"
---

# Feature Grid

5 张 pitch-card 排成 **2 × 3 错位网格**（第一行 3 槽用 2，第二行 3 槽用 3），形成 P5 asymmetric-balance。abs 钉 chrome-rule 上方。

## Template

```html
<div class="feature-grid" data-slot="feature-grid">
  <div class="pitch-card" data-slot="pitch-card">
    <div class="pitch-card-num"   data-slot="pitch-card-num">01</div>
    <svg class="pitch-card-icon"  data-slot="pitch-card-icon" viewBox="0 0 32 32">…</svg>
    <div class="pitch-card-body"  data-slot="pitch-card-body">{副文}</div>
    <div class="pitch-card-label" data-slot="pitch-card-label">{特性名}</div>
  </div>
  <!-- 5 张 pitch-card -->
</div>
```

布局 grid-area:
```
"a b ."
"c d e"
```

## Style

```css
.feature-grid {
  position: absolute;
  left:   var(--grid-margin-x);
  right:  var(--grid-margin-x);
  bottom: calc(var(--grid-margin-bottom) + var(--space-8));   /* 96 + 32 = 128 */
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows:    1fr 1fr;
  grid-template-areas:
    "a b ."
    "c d e";
  gap: var(--space-8);
}
.feature-grid > .pitch-card:nth-child(1) { grid-area: a; }
.feature-grid > .pitch-card:nth-child(2) { grid-area: b; }
.feature-grid > .pitch-card:nth-child(3) { grid-area: c; }
.feature-grid > .pitch-card:nth-child(4) { grid-area: d; }
.feature-grid > .pitch-card:nth-child(5) { grid-area: e; }

/* feature-grid 上下文里 pitch-card 强制填 forest green，不论 variant
   v1 设计意图：lite slide 上一组深色卡 = 强烈"内容块"信号 */
.feature-grid > .pitch-card {
  min-height: 240px;
  background: #0d2a25;
  color: #faf9f5;
  border-color: transparent;
}
.feature-grid > .pitch-card .pitch-card-body  { color: rgba(255, 255, 255, 0.55); }
.feature-grid > .pitch-card .pitch-card-icon  { stroke: #faf9f5; }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `feature-grid` | 是 | 容器 |
| `pitch-card`   | 是 | 5 张 pitch-card 子元素 |

## Constraints

- 必须 5 张（schema 写死 grid-area）
- 父是 page-shell（block 自身 abs 钉位）
- pitch-card 在此上下文强制深绿底白字（v1 override，无视 spec variant）

## Why

- 跟 P2 cards-row（3 列等宽）的差别是"错位 2x3 形成 P5 asymmetric"，刻意 row 1 第 3 槽留空
- 5 张 = 一行 2 + 一行 3 的最小 asymmetric 网格；少了不成 grid，多了破坏 chrome 上方空间
- pitch-card 的 forest green 填充是 block override（spec selector 已经在 dark variant 给同样规则；这里 lite/forest 都强制深色块）
