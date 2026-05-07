---
name: button
deps: []
tokens:
  - --primary
  - --primary-foreground
  - --secondary
  - --secondary-foreground
  - --destructive
  - --destructive-foreground
  - --foreground
  - --muted-foreground
  - --border
  - --radius-sm
  - --radius-full
  - --space-2
  - --space-3
  - --space-4
  - --space-6
  - --text-sm
  - --text-xs
  - --weight-medium
source: "@shadcn/button + 自加 pill variant"
---

# Button

按钮。支持 6 个 shadcn variant + 1 个 slide 场景常用的 `pill` 变体。

## Template

```html
<button class="button"
        data-slot="button"
        data-variant="default"
        data-size="default">
  {text}
</button>
```

`data-variant`: `default | destructive | outline | secondary | ghost | link | pill`
`data-size`:    `default | xs | sm | lg | icon`

## Style

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  gap: var(--space-2);
  border: 1px solid transparent;
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  white-space: nowrap;
  cursor: pointer;
  transition: all 0.15s ease;
}
.button:disabled { pointer-events: none; opacity: 0.5; }
.button > svg { pointer-events: none; flex-shrink: 0; width: 16px; height: 16px; }

/* ─── variants ─── */
.button[data-variant="default"] {
  background: var(--primary);
  color: var(--primary-foreground);
}
.button[data-variant="default"]:hover { opacity: 0.9; }

.button[data-variant="destructive"] {
  background: var(--destructive);
  color: var(--destructive-foreground);
}
.button[data-variant="destructive"]:hover { opacity: 0.9; }

.button[data-variant="outline"] {
  background: transparent;
  color: var(--foreground);
  border-color: var(--border);
}
.button[data-variant="outline"]:hover {
  background: var(--secondary);
}

.button[data-variant="secondary"] {
  background: var(--secondary);
  color: var(--secondary-foreground);
}
.button[data-variant="secondary"]:hover { opacity: 0.85; }

.button[data-variant="ghost"] { background: transparent; color: var(--foreground); }
.button[data-variant="ghost"]:hover { background: var(--secondary); }

.button[data-variant="link"] {
  background: transparent;
  color: var(--primary);
  padding: 0;
  height: auto;
}
.button[data-variant="link"]:hover { text-decoration: underline; text-underline-offset: 4px; }

/* pill：自加变体，圆角全开 + outline 风格 + muted 色 */
.button[data-variant="pill"] {
  background: transparent;
  color: var(--muted-foreground);
  border-color: var(--border);
  border-radius: var(--radius-full);
}
.button[data-variant="pill"]:hover { color: var(--foreground); border-color: var(--foreground); }

/* ─── sizes ─── */
.button[data-size="default"] { height: 36px; padding-inline: var(--space-4); }
.button[data-size="xs"] {
  height: 24px;
  padding-inline: var(--space-2);
  font-size: var(--text-xs);
}
.button[data-size="sm"]   { height: 32px; padding-inline: var(--space-3); }
.button[data-size="lg"]   { height: 40px; padding-inline: var(--space-6); }
.button[data-size="icon"] { width: 36px; height: 36px; padding: 0; }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| text | 否 | 按钮文本 |
| svg  | 否 | 前置/后置图标，自动 16×16 |

## Constraints

- 用原生 `<button>` 元素以保留 a11y / 键盘焦点
- 链接形态用 `<a class="button" data-variant="link">`，但 `<a>` 默认无 `cursor: pointer` 行为差异要注意

## Variants

| variant | 用途 |
|---|---|
| `default`     | 主要 CTA |
| `destructive` | 删除、危险动作（注意：monochrome 主题下视觉等同 default） |
| `outline`     | 次要操作 |
| `secondary`   | 次要 CTA |
| `ghost`       | 工具栏、隐藏感操作 |
| `link`        | 行内链接形态 |
| `pill`        | slide 场景的"轻量胶囊按钮"（卡片右上角"详情"等） |
