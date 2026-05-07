---
name: pricing-3up
deps: []
tokens:
  - --foreground
  - --background
  - --grid-margin-x
  - --weight-semibold
source: "迁移自 old/skills/getslide/designs/pitch/sample.html（3 列定价卡）"
---

# Pricing 3-up

3 张深色填充定价卡，**绝对定位**钉到 chrome-rule 上方 24px。每张含 plan name / price (含可选 strikethrough) / desc / 4-feature checklist / bottom CTA。

## Template

```html
<div class="pricing-row" data-slot="pricing-row">
  <div class="pricing-card" data-slot="pricing-card">
    <div class="tier-label" data-slot="tier-label">Plan name</div>
    <div class="tier-price" data-slot="tier-price">
      <span class="strike">$10</span>$9.99<span class="per">/Month</span>
    </div>
    <div class="tier-desc"  data-slot="tier-desc">{1 行 plan 描述}</div>
    <ul class="tier-features" data-slot="tier-features">
      <li>Feature 1</li>
      <li>Feature 2</li>
      <li>Feature 3</li>
      <li>Feature 4</li>
    </ul>
    <div class="tier-cta" data-slot="tier-cta">Get Plan <span>↗</span></div>
  </div>
  <!-- 3 张 pricing-card -->
</div>
```

`.strike` 删除线、`.per` 月份后缀都是可选 inline span。

## Style

```css
.pricing-row {
  position: absolute;
  left:   var(--grid-margin-x);
  right:  var(--grid-margin-x);
  bottom: 120px;        /* 96 chrome-rule + 24 breathing */
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}
.pricing-card {
  padding: 40px 36px;
  background: color-mix(in srgb, var(--foreground) 92%, transparent);   /* 几乎纯 fg 色 */
  color: var(--background);
  border-radius: 24px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  min-height: 560px;
}
.pricing-card .tier-label {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.7);
}
.pricing-card .tier-price {
  font-size: 48px;
  font-weight: var(--weight-semibold);
  letter-spacing: -0.02em;
  display: flex;
  align-items: baseline;
  gap: 12px;
}
.pricing-card .tier-price .strike {
  font-size: 22px;
  text-decoration: line-through;
  color: rgba(255, 255, 255, 0.4);
  font-weight: 400;
}
.pricing-card .tier-price .per {
  font-size: 18px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.7);
}
.pricing-card .tier-desc {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.4;
}
.pricing-card .tier-features {
  list-style: none;
  padding: 0;
  margin: 24px 0 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.pricing-card .tier-features li {
  font-size: 18px;
  line-height: 1.3;
  display: flex;
  align-items: flex-start;
  gap: 12px;
}
.pricing-card .tier-features li::before {
  content: "";
  width: 18px;
  height: 18px;
  flex: 0 0 18px;
  border-radius: 999px;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
  background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='10' viewBox='0 0 10 10'><path d='M2 5l2 2 4-4' stroke='white' stroke-width='1.5' fill='none' stroke-linecap='round'/></svg>") center/10px no-repeat;
  margin-top: 3px;
}
.pricing-card .tier-cta {
  margin-top: auto;
  background: var(--background);
  color: var(--foreground);
  padding: 16px 24px;
  border-radius: 999px;
  font-size: 18px;
  font-weight: var(--weight-medium);
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

## Slots

| 名字 | 必需 | 说明 |
|---|---|---|
| `tier-label`    | 是 | plan 名（"Pro Plan" / "Team" 等） |
| `tier-price`    | 是 | 价格 + 可选 `<span class="strike">` + `<span class="per">` |
| `tier-desc`     | 是 | 1 行 plan 描述 |
| `tier-features` | 是 | `<ul>` 含 4 个 `<li>` (用 `::before` 自动加打勾圈) |
| `tier-cta`      | 是 | bottom CTA 按钮（`margin-top: auto` 推到底） |

## Constraints

- 必须 3 张 pricing-card（pricing-3up 名字写明）
- 每张 features 列表推荐 4 项（少于 3 显单薄，超过 5 撑爆 min-height 560）
- 卡片内白文字硬编码 `rgba(255,255,255,*)` — 因为卡片是深色填充，不论 variant 都白文字（这是 v1 沉淀，violation 视觉一致性优先于 variant token）

## Why

- 3 张 = pricing 的"少 / 中 / 多" 经典三档；2 张显简单, 4+ 显眼花
- 卡片用 `var(--foreground) 92%` 几乎纯 fg 色 — 在 lite 上是深绿块，dark 上是浅块，forest 上是浅绿块；总之"反向于 page bg"
- features 用 CSS `::before` 加 SVG 打勾圈 = 不依赖外部 icon 字体
