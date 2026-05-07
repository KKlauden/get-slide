---
name: pitch-card
deps: []
tokens:
  - --card
  - --card-foreground
  - --foreground
  - --muted-foreground
  - --border
  - --radius
  - --space-6
  - --space-12
  - --weight-semibold
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（pitch 主题专属卡片，跟 shadcn card schema 不同）"
---

# Pitch Card

Pitch 主题专属卡片：上 num（编号/标签），下 label（主文，**自动撑底**），中间可选 body / icon。pitch deck 的 problem-numbered / feature-grid 两种布局共享这个 component。

跟 shadcn `card`（header / content / footer 结构）schema 完全不同——不复用，独立 component。

## Template

```html
<div class="pitch-card" data-slot="pitch-card">
  <div class="pitch-card-num"   data-slot="pitch-card-num">01</div>
  <div class="pitch-card-icon"  data-slot="pitch-card-icon">{svg}</div>     <!-- 可选 -->
  <div class="pitch-card-body"  data-slot="pitch-card-body">{副文}</div>     <!-- 可选 -->
  <div class="pitch-card-label" data-slot="pitch-card-label">{主文}</div>
</div>
```

最小用法（problem-numbered，P2 模式）—— 只有 num + label：

```html
<div class="pitch-card" data-slot="pitch-card">
  <div class="pitch-card-num"   data-slot="pitch-card-num">01</div>
  <div class="pitch-card-label" data-slot="pitch-card-label">短句陈述。</div>
</div>
```

完整用法（feature-grid，P4 模式）—— num + icon + body + label：

```html
<div class="pitch-card" data-slot="pitch-card">
  <div class="pitch-card-num"   data-slot="pitch-card-num">01</div>
  <svg class="pitch-card-icon"  data-slot="pitch-card-icon" viewBox="0 0 32 32">…</svg>
  <div class="pitch-card-body"  data-slot="pitch-card-body">副文说明</div>
  <div class="pitch-card-label" data-slot="pitch-card-label">特性标题</div>
</div>
```

## Style

```css
.pitch-card {
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
  padding: var(--space-12);
  background: var(--card);
  color: var(--card-foreground);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  min-height: 360px;
}

.pitch-card-num {
  font-size: 28px;
  font-weight: var(--weight-semibold);
  font-variant-numeric: tabular-nums;
}

.pitch-card-icon {
  width: 32px;
  height: 32px;
  stroke: currentColor;
  stroke-width: 1.6;
  fill: none;
}

.pitch-card-body {
  font-size: 20px;
  color: var(--muted-foreground);
  line-height: 1.4;
}

.pitch-card-label {
  font-size: 28px;
  font-weight: var(--weight-semibold);
  line-height: 1.2;
  margin-top: auto;          /* 关键：把主文推到 cell 底部 */
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `pitch-card-num`   | 否 | 顶部编号或短标签（"01" / "v2.0" 等），28px 600 tabular |
| `pitch-card-icon`  | 否 | 顶部图标（SVG），32×32，stroke 跟 currentColor |
| `pitch-card-body`  | 否 | 副文段落，20px muted，1.4 行高 |
| `pitch-card-label` | 是 | 主文，28px 600，line-height 1.2，**自动钉到卡片底部** |

**用法约定**：num / icon **二选一**（视上下文）：
- **problem-numbered (P2 模式)**：num + label，无 icon
- **feature-grid (P4 模式)**：icon + label + body，无 num
- 同时给 num + icon 在卡片高度受限时会撞 layout（P4 实测 + 12px overflow）

## Constraints

- num + label 组合必须存在；icon / body 可选
- min-height 360px 保证三 / 五 cards-row 同行视觉等高（非密集网格场景）
- padding 用 `--space-12` (48px)——左右略紧的密集卡可在父 block 用 `> .pitch-card { padding: 32px 24px }` override
- 不写死颜色——`background: var(--card)` 跟随 variant；spec 按需在 `[data-variant="dark"]` 等选择器 override

## Variants

通过外层 variant token + spec 选择器 override 切换视觉：

| variant | bg 来源 | 备注 |
|---|---|---|
| lite (默认) | `var(--card)` = page bg 同色 | 靠 border 显边 |
| dark / forest | spec override 给固定品牌色 | 比如 pitch dark 下用 `#0d2a25` (forest green block on black) |

具体 override 见所属 spec.md。

## Why

- shadcn 无对应（shadcn card 是 header/content/footer 结构，跟 pitch deck "num + label 二元 / icon + body 四元" 都对不上）
- 4 slot 设计覆盖 problem-numbered（P2）+ feature-grid（P4）两种用法，避免新建两个相似 component
- `margin-top: auto` 在 label 上是核心机制——cards-row 同行卡片不论内容长短，label 总是落在底边，视觉对齐
- 不引入二级 alias（`--pitch-card-bg` 等）——所有视觉值直吃 spec 的 palette/scale token
