# framework/charts/

**Chart pattern 契约层**——3 份 chart pattern：bar / gauge / line。

## 这是什么

不是「component 库」也不是「CSS 模板」——是 **结构 + 算法契约**：

- **结构**：chart 由什么部件组成（bars 区 / 数值 label / 横轴 label / baseline / caption 等）
- **算法**：数据 → 像素的精确换算（按 max 等比 / bar 高度兜底 / marker 几何对齐）
- **必须按 design.md 决定的视觉清单**（圆角 / 颜色 / 字 / 宽度——查 design.md token，不在文档里抄默认值）

## 为什么留下

components / blocks 库（card / button / hero / numeral 等）已经全删——基础 UI 现代 LLM 自己写没问题。但 **chart 不一样**：

- chart 是**数据可视化**——比例错了就是 bug（V3 测试时 AI 算高度差 0.1px 也行，但要点是必须按数据等比，不能凭感觉）
- chart 几何（SVG viewBox / data → 坐标公式 / gauge marker 对齐）AI 容易翻车
- chart 容易**被旧 paradigm 污染**（V3 测试发现 AI 翻 archive paradigm 的 bar-chart.md，下意识把 archive 的 6px 圆角 / mono labels 带到 CHASSAN 风 deck）

所以这 3 份 pattern 重写为「**只讲结构和算法，不讲视觉**」——AI 翻完学到怎么算 / 用什么部件，**视觉决策必须按当前 deck 的 design.md 自己写**。

## 3 份 pattern

| 文件 | 用什么 | 不用什么 |
|---|---|---|
| `bar-chart.md` | 真实数据可视化（每条 bar 高度反映独立值） | progress / fill 进度图（用 gauge）/ rhythm 装饰条（自己写） |
| `chart-gauge.md` | 单一进度值（"X% coverage"）+ N 个离散 bar 颗粒 + marker | 真实数据 chart（用 bar-chart）/ 圆形 / 半圆 gauge（自己写 SVG arc） |
| `chart-line-default.md` | 时间 / 顺序序列（1-2 series + dot + area-fill + annotation） | bar 数据（bar-chart）/ 散点 / 雷达 / 饼图（自己写） |

## 怎么用

1. **读 pattern**：理解结构 / 算法 / 必填元素清单
2. **算数据**：按 pattern 给的算法把 data → height / 坐标
3. **写 HTML + CSS**：HTML 结构跟 pattern 一致；CSS **按 design.md 自己写**——查 §2 Palette / §3 Typography / §4 Shape token，不要照抄 archive / pitch 任何 chart CSS
4. **验证**：bar 高度比例 = 数据比例？marker 跟 cutover 对齐？colors 跟整 deck 视觉一致？

## 不在这里的东西

- **基础 UI**（card / button / hero / numeral / pill / cards-row 等）—— AI 自己写
- **chart 的 CSS / SVG 视觉模板** —— 它们带主题污染（V3 测试痛点），AI 必须按 design.md 重新写视觉
- **新 chart 类型**（pie / radar / scatter 等）—— 参考 chart-line-default 末尾「v2 写新 chart 的转写规范」自己写
