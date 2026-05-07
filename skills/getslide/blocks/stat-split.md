---
name: stat-split
deps: []
tokens:
  - --foreground
  - --muted-foreground
  - --weight-semibold
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（M07 oversized-numeral）"
---

# Stat Split

垂直堆叠的多个 stat-block — 每块 = 88px 巨大数字 + 副文。**两块为限**（两块以上画面停止"锚定"，违反 P4）。

跟 page-content flow 配合：left 半屏放 title + body，right 半屏放 stat-split。

## Template

```html
<div class="stat-split" data-slot="stat-split">
  <div class="stat-block" data-slot="stat-block">
    <div class="stat-block-num"     data-slot="stat-block-num">88</div>
    <div class="stat-block-caption" data-slot="stat-block-caption">{副文 ≤ 480px 宽}</div>
  </div>
  <div class="stat-block" data-slot="stat-block">
    <div class="stat-block-num"     data-slot="stat-block-num">7×</div>
    <div class="stat-block-caption" data-slot="stat-block-caption">{副文}</div>
  </div>
</div>
```

`-num` 不限于阿拉伯数字 — 文字短词（"Mix" / "Chrome"）也行，只要能撑得起 88px 排版张力。

## Style

```css
.stat-split {
  display: flex;
  flex-direction: column;
  gap: 0;     /* stat-block 之间 56px 由 .stat-block + .stat-block 控制 */
}
.stat-block {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.stat-block + .stat-block { margin-top: 56px; }
.stat-block-num {
  font-size: 88px;
  font-weight: var(--weight-semibold);
  line-height: 1;
  letter-spacing: -0.02em;
  color: var(--foreground);
}
.stat-block-caption {
  font-size: 20px;
  line-height: 1.4;
  color: var(--muted-foreground);
  max-width: 480px;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `stat-block-num`     | 是 | 88px 巨大数字 / 短词 |
| `stat-block-caption` | 否 | 副文 ≤ 480px 宽 |

## Constraints

- 最多 2 个 stat-block（v1 P4 原则：3 个以上失锚）
- num 字符 ≤ 4 个（"100×" 还行，"1000+" 太挤）
- 父建议是 `.h8-12` 或 `.h7-12`（半屏右侧），左侧给 title

## Why

- v1 P5 的 M07 oversized-numeral 落地点 — 88px 是 h1 (76) 之上但不至于浮夸
- gap 56px between blocks = 大于段落间距、小于章节断行，让两块视觉是"并列陈述"而非"独立小节"
