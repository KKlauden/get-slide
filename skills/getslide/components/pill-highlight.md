---
name: pill-highlight
deps: []
tokens:
  - --primary
  - --primary-foreground
  - --radius-full
source: "原创（pitch 主题 M20 motif：pill-highlight-word）"
---

# Pill Highlight

把标题里某一个词或短语包成深色圆角 pill，作为强调。**Pitch 主题的标志性 motif（M20）**。

## Template

```html
<h1>
  Take
  <mark class="pill-highlight" data-slot="pill-highlight">control</mark>
  of your productivity!
</h1>
```

用 `<mark>` 元素（HTML 标准的"高亮"语义）+ class 改样式。

## Style

```css
.pill-highlight {
  display: inline-block;
  background: var(--primary);
  color: var(--primary-foreground);
  border-radius: var(--radius-full);
  padding: 0.04em 0.36em;
  /* 用 em 单位让 padding 随字号缩放 */
  margin: 0;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| text | 是 | 被高亮的词或短语，单行 |

## Constraints

- **每页最多一个** pill-highlight（pitch 主题 red line）——多 pill 互相竞争，强调失效
- 只用于标题（hero / h1 / h2）内部，不用于 body/lede 等正文
- padding 用 em 单位——保证不论标题字号多大，pill 比例都协调
- pitch 主题 monochrome 限制下，pill 的"反差"靠 primary↔primary-foreground 的明暗对比，不靠色相

## Why

- pitch 主题不允许 italic accent / chromatic accent，标题强调只能靠 pill
- 用 `<mark>` 元素而非 `<span>`：HTML 语义化更准确（"标记/高亮"），辅助技术能识别
- 不引入新色彩 token，复用 palette 的 `--primary` / `--primary-foreground`——主题换色，pill 自动跟着换
