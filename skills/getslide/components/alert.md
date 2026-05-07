---
name: alert
deps: []
tokens:
  - --card
  - --foreground
  - --muted-foreground
  - --destructive
  - --border
  - --radius
  - --space-3
  - --space-4
  - --weight-medium
  - --tracking-tight
  - --leading-snug
  - --leading-normal
  - --text-sm
source: "@shadcn/alert（简化 grid container query）"
---

# Alert

提示框，可带左侧图标。两种 variant：默认 / destructive。

## Template

```html
<div class="alert" data-slot="alert" data-variant="default" role="alert">
  <svg class="alert-icon" data-slot="alert-icon">…</svg>           <!-- 可选 -->
  <div class="alert-title" data-slot="alert-title">{title}</div>
  <div class="alert-description" data-slot="alert-description">{description}</div>
</div>
```

无图标版本：直接省略 `<svg>`，title/description 自动占满整行。

## Style

```css
.alert {
  display: grid;
  grid-template-columns: auto 1fr;
  align-items: start;
  column-gap: var(--space-3);
  row-gap: 2px;
  padding: var(--space-3) var(--space-4);
  background: var(--card);
  color: var(--foreground);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  font-size: var(--text-sm);
}
.alert:not(:has(.alert-icon)) { grid-template-columns: 1fr; }

.alert-icon {
  width: 16px;
  height: 16px;
  margin-top: 2px;
  color: currentColor;
  grid-row: 1 / span 2;
}
.alert-title {
  font-weight: var(--weight-medium);
  letter-spacing: var(--tracking-tight);
  line-height: var(--leading-snug);
}
.alert-description {
  color: var(--muted-foreground);
  line-height: var(--leading-normal);
}

.alert[data-variant="destructive"] {
  color: var(--destructive);
}
.alert[data-variant="destructive"] .alert-description {
  color: var(--destructive);
  opacity: 0.9;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `alert-icon`        | 否 | 16×16 图标，左对齐 |
| `alert-title`       | 是 | 单行标题 |
| `alert-description` | 否 | 描述段落 |

## Constraints

- 仅装饰用途的图标加 `aria-hidden="true"`
- destructive variant 用于错误信息；提示信息用 default
- monochrome 主题下 destructive variant 视觉等同 default（`--destructive` = `--foreground`）

## Variants

| variant | 用途 |
|---|---|
| `default`     | 中性提示 |
| `destructive` | 错误/警告（文本变红） |
