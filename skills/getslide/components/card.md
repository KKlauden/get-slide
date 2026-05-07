---
name: card
deps: []
tokens:
  - --card
  - --card-foreground
  - --border
  - --radius
  - --space-2
  - --space-6
  - --shadow-sm
  - --weight-semibold
  - --leading-tight
  - --text-sm
  - --muted-foreground
source: "@shadcn/card"
---

# Card

矩形内容块。最常用的容器，由 header / title / description / content / footer 等可选 slot 组成。

## Template

```html
<div class="card" data-slot="card">
  <div class="card-header" data-slot="card-header">
    <div class="card-title" data-slot="card-title">{title}</div>
    <div class="card-description" data-slot="card-description">{description}</div>
    <div class="card-action" data-slot="card-action">{action}</div>  <!-- 可选 -->
  </div>
  <div class="card-content" data-slot="card-content">{content}</div>
  <div class="card-footer" data-slot="card-footer">{footer}</div>     <!-- 可选 -->
</div>
```

`data-slot` 属性保留 shadcn 的语义标识方式，便于跨主题选择器命中。

## Style

```css
.card {
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
  padding-block: var(--space-6);
  background: var(--card);
  color: var(--card-foreground);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--shadow-sm);
}

.card-header {
  display: grid;
  grid-template-rows: auto auto;
  align-items: start;
  gap: var(--space-2);
  padding-inline: var(--space-6);
}

.card-header:has([data-slot="card-action"]) {
  grid-template-columns: 1fr auto;
}

.card-title       { font-weight: var(--weight-semibold); line-height: var(--leading-tight); }
.card-description { font-size: var(--text-sm); color: var(--muted-foreground); }

.card-action {
  grid-column-start: 2;
  grid-row: 1 / span 2;
  align-self: start;
  justify-self: end;
}

.card-content { padding-inline: var(--space-6); }

.card-footer {
  display: flex;
  align-items: center;
  padding-inline: var(--space-6);
  font-size: var(--text-sm);
  color: var(--muted-foreground);
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `title`       | 否 | 标题，单行，font-weight 600 |
| `description` | 否 | 副标题，14px，muted 色 |
| `action`      | 否 | 标题右侧操作位（按钮/链接），出现时 header 自动两列 |
| `content`     | 是 | 主体内容 |
| `footer`      | 否 | 页脚 |

## Constraints

- 父节点应为 page-shell 内的 grid 容器或某种行容器（如 `cards-row` / `feature-grid`）
- 同一行卡片应高度一致（用 `min-height` 或 grid `align-items: stretch`，由父 block 控制）
- 不要在 card 里嵌 card——更深层级用 block

## Why

- shadcn 把 card 拆成 7 个子组件（Card / CardHeader / CardTitle / CardDescription / CardAction / CardContent / CardFooter），所有子组件平权
- 我们保留这套结构，但简化为单一 `.card` 容器 + 子 div，不引入 React 组件语义
- `data-slot` 属性是 shadcn 的核心约定，**跨主题命中要靠它**——比如某主题想加 hover 效果，应写 `[data-slot="card"]:hover {…}` 而不是 `.card:hover`，避免类名冲突
- padding/gap/radius/shadow 直接引用 spec 的 scale token（`--space-6` / `--radius` / `--shadow-sm`），不设 component 二级 alias——主题想改卡片视觉，在 spec 里改 `--card` 值或写 `.card { padding-block: 32px }` 覆盖
