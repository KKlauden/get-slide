---
name: contact-list
deps: []
tokens:
  - --foreground
  - --text-lg
  - --weight-normal
  - --leading-normal
  - --space-3
source: "原创（pitch closing 收尾常见模式：3-4 行联系方式 / 调用动作）"
---

# Contact List

封底（closing）专用——3-4 行平直 fg 色文本，常用于列联系方式 / 调用动作 / 渠道。

通常作为 hero-block 的子元素，**取代 lede**。

## Template

```html
<ul class="contact-list" data-slot="contact-list">
  <li>github.com/getslide</li>
  <li>npx getslide install</li>
  <li>open the file. that's it.</li>
</ul>
```

放在 hero-block（`hero-block--top`）内：

```html
<div class="hero-block hero-block--top" data-slot="hero-block">
  <h1 class="hero-block-title" data-slot="hero-block-title">The Future is Here.</h1>
  <ul class="contact-list" data-slot="contact-list">
    <li>hello@brand.com</li>
    <li>+1 (415) 555-0100</li>
    <li>www.brand.com</li>
  </ul>
</div>
```

## Style

```css
.contact-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  font-size:   var(--text-lg);
  font-weight: var(--weight-normal);
  line-height: var(--leading-normal);
  color: var(--foreground);
}
.contact-list > li { margin: 0; padding: 0; }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `contact-list` | 是 | 容器 `<ul>`。子元素必须是 `<li>` |

## Constraints

- 行数 3-4 行最佳；超过 5 行用普通段落
- 每行内容简短（1-6 字 / 1 行 URL / 1 个动作）；长句用 lede 而非 contact-list
- 不在 hero-block 外独立使用——脱离 hero-block 后字号 / 位置都会显得空洞
- 不带 bullet——用 `list-style: none` 把项目符号去掉

## Why

- shadcn 无对应 component（pitch / 大多数 deck closing 的常见结构）
- `<ul>+<li>` 比 v1 的 `<div>+<div>` 更语义化（contact 本质是列表，不是叙述段）
- 字号刻意比 hero-title 小一档（`--text-lg` = 32px）但比 body 大——平衡 hero 收尾的"宣言感"和"实用信息"
