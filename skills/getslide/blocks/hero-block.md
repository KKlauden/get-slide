---
name: hero-block
deps: []
tokens:
  - --foreground
  - --text-4xl
  - --text-lg
  - --weight-semibold
  - --weight-normal
  - --tracking-tight
  - --leading-tight
  - --leading-normal
  - --space-6
source: "原创（cover/closing 共享的"display title + lede"复合块）"
---

# Hero Block

Display 大标题 + lede 副标的复合块。常用于 cover 与 closing。

## Template

```html
<div class="hero-block" data-slot="hero-block">
  <h1 class="hero-block-title" data-slot="hero-block-title">{title with optional <br>}</h1>
  <p  class="hero-block-lede"  data-slot="hero-block-lede">{lede}</p>      <!-- 可选 -->
</div>
```

支持 `<br>` 强制换行；多行 hero 标题用 `<br>` 表达，不是单独 slot。

**Closing 变体**——加 modifier `.hero-block--top` 把内容钉在 cell 顶部（默认是 `align-self: end`）：

```html
<div class="hero-block hero-block--top" data-slot="hero-block">
  <h1 class="hero-block-title" data-slot="hero-block-title">{title}</h1>
  <!-- 这里可以放 contact-list 取代 lede（见 components/contact-list.md） -->
</div>
```

## Style

```css
.hero-block {
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
  position: relative;
  z-index: 2;
  align-self: end;            /* 默认：cover——内容靠 cell 底部 */
}
.hero-block--top {
  align-self: start;          /* closing：内容靠 cell 顶部 */
}
.hero-block-title {
  font-size:        var(--text-4xl);
  font-weight:      var(--weight-semibold);
  letter-spacing:   var(--tracking-tight);
  line-height:      var(--leading-tight);
  color:            var(--foreground);
  margin: 0;
}
.hero-block-lede {
  font-size:    var(--text-lg);
  font-weight:  var(--weight-normal);
  line-height:  var(--leading-normal);
  color:        var(--foreground);
  margin: 0;
  max-width: 800px;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `hero-block-title` | 是 | 大标题。可含 `<br>` 强制换行；可在内部嵌 `<mark class="pill-highlight">` 强调某词 |
| `hero-block-lede`  | 否 | 副标，单行或短段 |

## Constraints

- 父容器决定位置——cover 通常放在 grid `.h1-9 .v6-7`（左下三分之一）；closing 镜像放 `.h1-9 .v3-5`
- title 不带句尾标点（除非是问号/感叹号）——pitch 风格
- hero-block 自身有 `z-index: 2 + position: relative`，让它**浮在 ambient-frames 之上**
- 不引入 component 二级 alias——字号、字重、间距直接吃 spec 的 scale token

## Why

- shadcn 无对应 component（pitch slide 场景特有）
- 拆成"title + lede"两个 slot 的合理性：cover/closing 永远是这种二元结构，提取为 block 比每页 inline 写更稳定
- 不区分 cover/closing 两个 variant——位置由 content/grid 决定，hero-block 本身只管视觉层
