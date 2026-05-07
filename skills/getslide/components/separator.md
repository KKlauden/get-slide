---
name: separator
deps: []
tokens:
  - --border
source: "@shadcn/separator"
---

# Separator

一根分隔线。水平 / 垂直两种朝向。

## Template

```html
<div class="separator" data-slot="separator" data-orientation="horizontal" role="separator"></div>
<div class="separator" data-slot="separator" data-orientation="vertical"   role="separator"></div>
```

`data-orientation` 必填，缺省按 `horizontal` 处理。

## Style

```css
.separator {
  flex-shrink: 0;
  background: var(--border);
}
.separator[data-orientation="horizontal"] { height: 1px;  width: 100%; }
.separator[data-orientation="vertical"]   { width:  1px;  height: 100%; }
```

## Constraints

- 在 flex / grid 容器中使用。垂直 separator 需要父容器有明确高度
- 装饰性分隔写 `role="separator"` 且无 aria-orientation；语义分隔（如菜单分组）应额外加 `aria-orientation`

## Why

- shadcn 用 radix-ui 的 `<Separator>`（处理 a11y 语义），我们直接出 div + role
- 不用 `border-top`/`border-left` 而用 `background` + 1px 维度——避免横竖切换时切换属性，统一为"面积"
- 直接吃 `--border`，不另设 `--separator-color`——所有分割线在主题里风格一致
