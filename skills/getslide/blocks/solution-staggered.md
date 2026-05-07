---
name: solution-staggered
deps: []
tokens:
  - --card
  - --foreground
  - --muted-foreground
  - --border
  - --grid-margin-x
  - --grid-margin-bottom
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（M07 stagger pattern + M08 hairline）"
---

# Solution Staggered

5 张塔形错位卡片，**贴接无 gap**，**底部对齐 chrome-rule**——pitch deck "solution slide" 标志性视觉。

塔形高度 **480/340/220/340/480**（中矮两边高，对称），让卡片顶端勾出"五山式"轮廓——P5 asymmetric balance 通过 stagger 实现。

## Template

```html
<div class="solution-staggered" data-slot="solution-staggered">
  <div class="solution-staggered-card" data-slot="solution-staggered-card">
    <div class="solution-staggered-label" data-slot="solution-staggered-label">{短题}</div>
    <div class="solution-staggered-body"  data-slot="solution-staggered-body">{副文}</div>
  </div>
  <!-- 5 张同结构 -->
</div>
```

固定 5 张——少于 5 张破坏对称塔形；多于 5 张让中间矮卡视觉权重不足。

## Style

```css
.solution-staggered {
  position: absolute;
  left:   var(--grid-margin-x);
  right:  var(--grid-margin-x);
  bottom: var(--grid-margin-bottom);
  display: flex;
  align-items: flex-end;       /* 卡片底部对齐 chrome-rule */
  gap: 0;                      /* 贴接 — 靠 border 区隔 */
  height: 480px;
  z-index: 1;
}

.solution-staggered-card {
  flex: 1 1 0;
  padding: 32px 24px;
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 20px 20px 0 0;  /* 顶圆角 / 底直角 — 卡片底贴 chrome-rule */
  display: flex;
  flex-direction: column;
  text-align: center;
}

/* 塔形高度：中矮两边高，对称 */
.solution-staggered-card:nth-child(1),
.solution-staggered-card:nth-child(5) { height: 480px; }
.solution-staggered-card:nth-child(2),
.solution-staggered-card:nth-child(4) { height: 340px; }
.solution-staggered-card:nth-child(3)                       { height: 220px; }

.solution-staggered-label {
  font-size: 26px;
  font-weight: var(--weight-semibold);
  line-height: 1.25;
  color: var(--foreground);
}

.solution-staggered-body {
  font-size: 18px;
  line-height: 1.4;
  color: var(--muted-foreground);
  margin-top: auto;            /* 推到卡片底部 */
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `solution-staggered`       | 是 | 容器，放 5 张 card |
| `solution-staggered-card`  | 是 | 内置 5 张，固定数量 |
| `solution-staggered-label` | 是 | 卡片顶部短题，26px 600 |
| `solution-staggered-body`  | 是 | 卡片底部副文，18px muted |

## Constraints

- 必须 5 张卡（少了破坏对称塔形）
- 父元素是 page-shell（block 自身 absolute，钉到 page-shell 的 chrome-rule 上方）
- **不要**放在 page-content (flow) 内 — block 自己 abs 钉位，跟 page-content 流式排无关
- z-index: 1 — 让 ambient-frames (z-index 0) 在背景，title (z-index 默认) 在前

## Why

- **塔形高度硬编码 px**：480/340/220 是 v1 沉淀的设计常量（不是 spacing scale 的派生值），不抽 token——抽了反而让 block 内部 magic number 散到 spec
- **gap: 0 + border 区隔**：卡片贴接 + 1px hairline 分隔，比 gap 24px 留缝更紧凑——P5 stagger 美学要求"形成连续轮廓"
- **背景 `var(--card)`**：forest variant 下自动是 `rgba(0, 0, 0, 0.24)` 半透明黑，跟 v1 一致；其它 variant 跟随 spec
- **底直角 + bottom 对齐 chrome-rule**：卡片视觉感像"从 chrome-rule 长出的塔"，不是浮在中间的 cards
- **不抽 component**：单张 solution-staggered-card 脱离 stagger 容器没语义（视觉只在塔形 + 贴接 + 文字居中组合下成立）
