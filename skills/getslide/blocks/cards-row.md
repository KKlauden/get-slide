---
name: cards-row
deps:
  - card
tokens:
  - --space-8
source: "原创（shadcn 用 Tailwind grid utility，我们没有 utility class 故提取为 block）"
---

# Cards Row

把 N 张 card 平铺成一行。负责三件事：**列数**、**间距**、**等高**。

## Template

```html
<div class="cards-row" data-slot="cards-row" data-cols="3">
  <div class="card" data-slot="card">…</div>
  <div class="card" data-slot="card">…</div>
  <div class="card" data-slot="card">…</div>
</div>
```

`data-cols` 属性指定列数（2/3/4/5/6）。

## Style

```css
.cards-row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-8);
  align-items: stretch;
  align-self: start;     /* 在父 grid cell 内不被 stretch 高度，cards 维持 min-height */
}

.cards-row[data-cols="2"] { grid-template-columns: repeat(2, 1fr); }
.cards-row[data-cols="3"] { grid-template-columns: repeat(3, 1fr); }
.cards-row[data-cols="4"] { grid-template-columns: repeat(4, 1fr); }
.cards-row[data-cols="5"] { grid-template-columns: repeat(5, 1fr); }
.cards-row[data-cols="6"] { grid-template-columns: repeat(6, 1fr); }

/* 等高：让 card-content 自动撑开 */
.cards-row > [data-slot="card"] > [data-slot="card-content"] {
  flex: 1;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| children | 是 | N 个 `[data-slot="card"]` 子元素 |

## Constraints

- 子元素**只能是** card（或其同形态衍生品），不能塞段落/图片
- 列数限定 2-6；超过 6 列考虑用 `cards-grid`（多行）block
- 同行 card 自动等高，无需在 card 上加 `flex: 1`

## Why

- card.md 不写 `flex: 1`，因为 card 不知道自己在哪里被用——shadcn 也是这样
- 等高规则归 cards-row 这个 block，因为它知道自己是行容器
- 这是 block 越过自身 selector 给 component 加规则的合法用法（用后代选择器 `> [data-slot="card-content"]`，作用域受限）
- gap 直接吃 `--space-8`，不另设 `--cards-row-gap`——主题想改间距，spec 里写 `.cards-row { gap: var(--space-12) }` 覆盖
