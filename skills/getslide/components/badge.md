---
name: badge
deps: []
tokens:
  - --primary
  - --primary-foreground
  - --secondary
  - --secondary-foreground
  - --destructive
  - --destructive-foreground
  - --foreground
  - --border
  - --radius-full
  - --space-1
  - --space-2
  - --text-xs
  - --weight-medium
source: "@shadcn/badge"
---

# Badge

小尺寸圆角标签，常用于状态、分类、计数等。

## Template

```html
<span class="badge" data-slot="badge" data-variant="default">{text}</span>
```

`data-variant` 取值：`default | secondary | destructive | outline | ghost | link`，缺省按 `default`。

## Style

```css
.badge {
  display: inline-flex;
  width: fit-content;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  gap: var(--space-1);
  padding: 2px var(--space-2);
  border: 1px solid transparent;
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  white-space: nowrap;
  overflow: hidden;
}

.badge[data-variant="default"] {
  background: var(--primary);
  color: var(--primary-foreground);
}
.badge[data-variant="secondary"] {
  background: var(--secondary);
  color: var(--secondary-foreground);
}
.badge[data-variant="destructive"] {
  background: var(--destructive);
  color: var(--destructive-foreground);
}
.badge[data-variant="outline"] {
  color: var(--foreground);
  border-color: var(--border);
}
.badge[data-variant="ghost"]  { color: var(--foreground); }
.badge[data-variant="link"]   {
  color: var(--primary);
  text-decoration: underline;
  text-underline-offset: 2px;
}

.badge > svg { width: 12px; height: 12px; pointer-events: none; }
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| text | 是 | badge 主文本 |
| svg  | 否 | 可选前置图标，自动 12×12 |

## Constraints

- 单行，不换行（white-space: nowrap）
- 内置 `width: fit-content`——不会被父容器拉伸

## Variants

| variant | 视觉 | 用途 |
|---|---|---|
| `default`    | primary 底 + primary-foreground | 状态强调、主要分类 |
| `secondary`  | secondary 底 + 主文本色 | 次要分类、计数 |
| `destructive`| 危险色底 + 白字 | 错误、移除、警告 |
| `outline`    | 透明底 + 描边 | 次次要、无强调 |
| `ghost`      | 透明底，无描边 | 极弱标识 |
| `link`       | 主色文字 + 下划线 | 行内链接形态 |
