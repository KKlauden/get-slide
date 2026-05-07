---
name: compare-stagger
deps: []
tokens:
  - --foreground
  - --muted-foreground
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（4 卡 zig-zag stagger）"
---

# Compare Stagger

4 张固定尺寸的对比卡，**绝对定位 + zig-zag 错位** — 视觉上读为"对照轴"，每张 num/label/body。卡片自身 abs 钉 page-shell coords，不参与流式排版。

## Template

```html
<div class="compare-card" data-slot="compare-card" style="left: 96px; bottom: 128px;">
  <div class="compare-card-num"   data-slot="compare-card-num">01</div>
  <div class="compare-card-label" data-slot="compare-card-label">{对比维度名}</div>
  <div class="compare-card-body"  data-slot="compare-card-body">{对比陈述}</div>
</div>
<!-- 4 张，每张自带 inline left/right + bottom 钉位 -->
```

**block 不提供容器** — 4 张 compare-card 直接是 page-shell 的子元素。每张 inline 定位，形成 zig-zag。

推荐 4 张钉位（v1 沉淀，配 1920×1080）：

| 卡 | 钉位 | 含义 |
|---|---|---|
| 01 | `left: 96; bottom: 128`  | 左下 |
| 02 | `left: 680; bottom: 258` | 中间偏上 |
| 03 | `right: 96; bottom: 388` | 右上 |
| 04 | `right: 96; bottom: 128` | 右下 |

01→04 zig-zag 形成 N 字轨迹。

## Style

```css
.compare-card {
  position: absolute;
  width: 560px;            /* = 4 H 列 (4 × 122 + 3 × 24 gutter) */
  height: 200px;           /* 固定不随 body 长度变 */
  padding: 32px 28px;
  background: rgba(0, 0, 0, 0.24);   /* 半透明黑（forest variant 上正好叠成"凹陷面"） */
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  z-index: 1;
}
.compare-card-num {
  font-size: 22px;
  font-weight: var(--weight-semibold);
  color: var(--muted-foreground);
  font-variant-numeric: tabular-nums;
}
.compare-card-label {
  font-size: 24px;
  font-weight: var(--weight-semibold);
  line-height: 1.2;
  color: var(--foreground);
}
.compare-card-body {
  font-size: 18px;
  line-height: 1.4;
  color: var(--muted-foreground);
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `compare-card-num`   | 是 | 22px muted 编号 |
| `compare-card-label` | 是 | 24px 600 短标 |
| `compare-card-body`  | 是 | 18px muted 一行陈述 |

## Constraints

- 必须 4 张（zig-zag 需要至少 4 个点形成 N 字）
- 卡尺寸固定 560×200 — 不随内容自适应
- 父是 page-shell（abs 钉位）；左半屏留给 page-content flow 写 title + body
- 每张卡 inline `left/right + bottom` — block 不抽容器决定钉位（v1 设计意图就是 4 张独立 abs）

## Why

- 不抽 wrapper container — 4 张独立 abs 让设计师能 N 字 / Z 字 / V 字自由排，container 反而限制
- 560×200 固定尺寸 = 一张 compare-card 占 4 H 列 × 200 高 = 1/12 slide 面积，4 张总和不超 1/3 — 给 chrome + title + 副文留空间
- 半透明黑底 (`rgba(0,0,0,0.24)`) 是为 forest variant 量身定制（深绿底叠黑 = 凹陷面）；其它 variant 会变浅灰，可接受
