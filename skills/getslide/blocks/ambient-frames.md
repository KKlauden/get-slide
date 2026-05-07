---
name: ambient-frames
deps: []
tokens:
  - --foreground
  - --font-display
  - --weight-bold
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（M18 motif：deck initial letter as identity stamp）"
---

# Ambient Frames

Pitch 主题特色装饰：将 deck 首字母以**极端字号**（1560px）渲染两次（一个 filled 半透明、一个 outlined 描边），错位叠加，**故意溢出 slide 边界**——形成"identity stamp from outside the page"的几何点缀。

> v1 注释原话：*"the deck's own initial letter, set in the body grotesk at extreme size, doubled (one filled + one outlined) and offset so the eye reads it as a faint identity stamp, not as content."*

## Template

封面（cover）用法——容器 `top:-300`，字母 P 从 slide 上沿溢出：

```html
<div class="ambient-frames" data-slot="ambient-frames" aria-hidden="true"
     style="top:-300px; right:-237px; width:1100px; height:1380px;">
  <svg viewBox="0 0 1100 1380" width="1100" height="1380">
    <text class="filled"   x="0"   y="1284" font-size="1560">P</text>
    <text class="outlined" x="140" y="1212" font-size="1560">P</text>
  </svg>
</div>
```

封底（closing）用法——容器 `top:180`（cover 整体下移 480px），字母 P 从 slide 下沿溢出：

```html
<div class="ambient-frames" data-slot="ambient-frames" aria-hidden="true"
     style="top:180px; right:-237px; width:1100px; height:1380px;">
  <svg viewBox="0 0 1100 1380" width="1100" height="1380">
    <text class="filled"   x="0"   y="1284" font-size="1560">P</text>
    <text class="outlined" x="140" y="1212" font-size="1560">P</text>
  </svg>
</div>
```

字母由 deck 决定（`P` for pitch / `B` for brand 等），其余数值（`x` `y` `font-size` `width` `height`）是设计常量，不暴露。

## Style

```css
.ambient-frames {
  position: absolute;
  pointer-events: none;
  z-index: 0;
}
.ambient-frames svg {
  display: block;
}
.ambient-frames text {
  font-family:    var(--font-display);
  font-weight:    var(--weight-bold);
  letter-spacing: -0.04em;
  paint-order:    stroke fill;
}
.ambient-frames text.filled {
  fill:   color-mix(in srgb, var(--foreground) 10%, transparent);
  stroke: none;
}
.ambient-frames text.outlined {
  fill:   none;
  stroke: var(--foreground);
  stroke-width: 3;
  stroke-opacity: 0.8;
  stroke-linejoin: round;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `ambient-frames` | 是 | 容器本身。inline `style` 控位置（top/right/width/height） |

SVG 内的 `<text>` 字符由 deck 直接写入，不暴露 slot。

## Constraints

- 必须在 `position: relative` 的父容器内（page-shell 默认就是）
- 容器自己 `position: absolute`，**不要**用 grid 工具类（`.h7-12 .v1-8` 等）——v1 的设计依赖于"溢出 slide 边界"，grid 会把它框住
- z-index 0 — hero-block (z-index 2) 会盖在上面
- filled / outlined 顺序固定：filled 在前（DOM 先），outlined 后画（避免被 fill 半遮）
- letter 永远是**单个大写拉丁字母**——长字符串/中文会破坏"印章"感
- dark / forest variant 自动跟随 `var(--foreground)`，无需重写

## Variants

| 用途 | 容器 inline style |
|---|---|
| Cover（字母从顶部溢出） | `top:-300; right:-237; width:1100; height:1380` |
| Closing（字母从底部溢出） | `top:180; right:-237; width:1100; height:1380` |

中间页 P2-P8 不用 ambient-frames（cover/closing 专属）。

## Why

- pitch 主题最具识别度的装饰——比 3 个圆角矩形（v2 旧实现）更有"印章感"
- 字母印章 = deck 自身的 brand initial 重复使用，呼应 chrome-top-left 的 `.logo-mark`
- 用 SVG `<text>` 而非位图：跟随 `--foreground` 自动适配 lite/dark/forest 三 variant
- "溢出 slide 边界"是设计核心——容器 `top` 为负值是有意为之，不要"修正"
