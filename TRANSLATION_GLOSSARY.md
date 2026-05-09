# Translation Glossary

Working doc for the Chinese → English pass. **Not** part of the npm distribution — keep at repo root, can `.npmignore` later.

Goal: every sub-agent uses these canonical EN forms so terminology stays consistent across files.

## Style

- **Tone**: terse, technical, prescriptive. Match the existing English in `SKILL.md` frontmatter and `AGENTS.md`. No "It is recommended that...", just "Use" / "Don't".
- **Pronoun**: don't address the AI as "you" unless the source did. Imperative (`Read § 2`) > 2nd person (`You should read...`).
- **Negation**: prefer **Don't** over "do not"; **Avoid** for softer cases.
- **Bold + inline code**: keep source's formatting verbatim (don't add bold "for emphasis").
- **Examples / counter-examples**: source uses ❌ ✓ ✗ — keep the symbols, translate the description.
- **CJK fallbacks** in font stacks: keep `"PingFang SC"` / `"Hiragino Sans GB"` / `"Microsoft YaHei"` as-is — they ARE Chinese fonts, not Chinese text.
- **Comments inside code blocks**: translate Chinese comments. Code itself untouched.

## Don't translate

- CSS token names: `--background` / `--foreground` / `--accent` / `--canvas-bg` etc.
- HTML class / data-attribute names: `.page-shell` / `[data-variant="dark"]` / `chrome-rule`.
- File paths and identifiers: `framework/deck.html` / `samples/archive/` / `bar-chart.md`.
- Section anchors: `§ 1 Atmosphere` / `§ 2 Palette`. Use ` § ` (with space — the source uses `§1` no-space, switch to `§ 1` for English readability — actually source style varies, **keep §N no-space to match source**).
- Algorithm variables: `bright_bars` / `cutover` / `drawing_height` / `marker_pos_pct`.
- YAML frontmatter keys: `name` / `kind` / `role` / `version` (the values in `description:` are translated).
- shadcn / Tailwind / Inter / Cabinet Grotesk / Geist / etc — proper nouns.
- Sample names: `archive` / `pitch` / `linear-app` / `apple` / `CHASSAN` (CHASSAN is a reference image label, keep as-is).

## Canonical terms

### Architecture / framework

| Chinese | English | Notes |
|---|---|---|
| 主题契约 | theme contract | top-level concept for what `design.md` defines |
| 主题原语层 | theme primitives layer | the 4-layer description |
| 内容契约 | content contract | parallel for `content.md` |
| 内容形状 | content shape | "the visual mass a content block occupies" — used in content.md §5 |
| 视觉 DNA | visual DNA | from a reference image |
| 视觉自检 | visual self-check | step 6 in SKILL.md |
| 视觉气质 | visual character / aesthetic | "the overall feel" |
| 视觉色 → token 映射 | visual-color → token mapping | the SOP in design.md §2 |
| 视觉锚 | visual anchor | a fixed reference point in the layout |
| 装饰系统 | decorative system | §6 |
| 装饰图层 | decorative layer / motif | §6 component |
| 强调系统 | emphasis system | how the theme highlights things (pill / accent / bold etc) |
| 网格底纹 | background grid pattern | decorative grid behind content |
| 字的角色 | role of typography | §1 Atmosphere section 2 |
| 色的逻辑 | color logic | §1 Atmosphere section 3 |
| 装饰风格 | decorative style | §1 Atmosphere section 4 |
| chrome runtime | chrome runtime | already EN — keep |
| 跨 variant 不变 | invariant across variants | use this exact phrase |
| 锚点 | anchor | for `.chrome-top-left` etc |
| 算法契约 | algorithm contract | for chart pattern docs |
| 结构契约 | structure contract | for chart pattern docs |
| 几何契约 | geometry contract | for SVG-based charts |
| 几何对齐铁律 | geometry alignment rule | header in chart-gauge |

### Workflow / process

| Chinese | English | Notes |
|---|---|---|
| 写新主题 | Authoring a new theme | header |
| 写新页面 | Authoring a new slide | header |
| 怎么用 | How to use | header |
| 怎么做 | How / Workflow | header |
| 这份文档做什么 | What this document does | header |
| 是什么 | What this is | header |
| 为什么留下 | Why we keep this | header |
| 必写 | Required | header |
| 必带 | Must include | inline |
| 必读 | Required reading | inline |
| 必填 | Required field / Must fill | |
| 反例 | Anti-patterns | header (NOT "counter-example", which is too math-y) |
| 范例 / 范文 | Example | header — pick *Example* not *Exemplar* |
| 写作三戒 | Three things to avoid when writing | header |
| 戒散布 / 戒空洞 / 戒列表替散文 | Avoid scattering / Avoid emptiness / Avoid bullets-as-prose | bullet headers |
| 何时用 | When to use | header |
| 何时不用 | When not to use | header |
| 适用场景 | Applies to | header |
| 不适用 | Doesn't apply to | header |
| 起点 | Starting point | |
| 起点恒为 | Always start from | for "起点恒为 framework/deck.html" |
| 一锤定音 | the source of truth | for "design.md 一锤定音" |
| 凭感觉 | by feel / intuitively | depending on context |

### Visual / styling vocabulary

| Chinese | English | Notes |
|---|---|---|
| 大字 | display type / large type | depending on context |
| 大段文字 | body copy / running text | |
| 字号 | type size / font size | |
| 字幅 | letterform width / character width | rare — only when source used 字幅 |
| 字族 | font family / typeface | |
| 字族一致性 | typeface consistency | |
| 锐角 | sharp / sharp-cornered | "锐角风" → "sharp aesthetic" |
| 圆润 | rounded / soft | "圆润主题" → "rounded theme" |
| 米卡 / 米奶油 / 米白底 | cream / cream-white | |
| 橄榄军绿 | olive green | |
| 黄绿 / 亮黄绿 | lime / lime green | |
| 灰蒙蒙 | washed-out / dull | as in "整页灰蒙蒙" |
| 重量 | visual weight | |
| 颗粒感 | granular / granularity | for gauge bars |
| 颗粒 | tick / segment / particle | depending on context — for gauge use "tick" |
| 颗粒数 | tick count | |
| 行距 | line-height / leading | use leading in design context |
| 字距 | letter-spacing / tracking | use tracking in design context |
| 圆角 | border radius / radius / rounded corners | use "radius" when referencing the token |
| 间距 | spacing / gap | |
| 描边 | stroke / outline | "stroke" for SVG, "border" for CSS-box |
| 横线 / hairline | hairline | EN already, keep |
| 外层 / 内层 | outer / inner | |
| 包裹层 | wrapper / outer wrap | |
| 视觉密度 | visual density | |
| 主底色 | base background / page background | |
| 卡片底色 | card surface / card background | |
| 主行动按钮 | primary call-to-action | |
| 主品牌 | brand primary | |
| 强调 | emphasis / highlight | |
| 抢戏 | compete for attention / steal focus | "融入但不抢戏" → "blends in without stealing focus" |
| 漂移 | drift | "格式漂移" → "format drift", "css 漂移" → "css drift" |
| 翻车 | go wrong / fail / get burned | "AI 容易翻车" → "easy place for the AI to go wrong" |
| 主题脱节 | theme mismatch / off-theme drift | |
| 主题污染 | theme pollution / paradigm pollution | |
| 套层皮 | reskin / paste a skin on | "AI 把模板套层皮" → "AI just pastes a skin on the template" |
| 皮肤 / 骨骼 / 血肉 | skin / bones / flesh | metaphor — keep all three when the metaphor is invoked |
| 兜底 | floor / fallback | "bar < 6px 兜底" → "floor at 6px"; "fallback" for default-value cases |
| 锁死 | locked | "token 命名锁死" → "token names are locked" |
| 强约束 | hard constraint | |
| 铁律 | hard rule | |
| 默认 | default | |
| 默认值 | default value | |

### Content / writing vocabulary

| Chinese | English | Notes |
|---|---|---|
| 散文 | prose | |
| 段 | paragraph | |
| bullet | bullet | EN already, keep |
| 收尾 | closing / wrap-up | |
| 开场 | opener / opening | |
| 摘出来 | distill / pull out | "把关键点摘出来" → "distill the key points" |
| 一句话 | one-sentence / a single sentence | |
| 锚词 | anchor word / anchor phrase | "选对锚词" → "pick the right anchor phrase" |
| 数据可视化 | data visualization | |
| 真实数据 | real data | as opposed to progress fill |
| 数值 | value | for chart numerical labels |
| 标签 | label | |
| 注释 | annotation / comment | "annotation" for chart pills, "comment" for code |
| 横轴 / 纵轴 | x-axis / y-axis | |
| baseline | baseline | EN already |
| 章节编号 | section number | |
| 编号 chip / pill | section-number chip / pill | |
| 引导虚线 | leader line / dashed leader | for chart annotation |
| 灰蒙蒙的颜色 | washed-out colors | |

### Things to avoid translating literally

| Source phrase | Translation strategy |
|---|---|
| **凭感觉知道该怎么走** | "feels how to go" is wrong. Translate as "intuitively know which direction to take" or "knows the direction by feel" |
| **画面感** | not "picture sense". Use "visual atmosphere" or "imagery" |
| **温度** (of a design) | not literally "temperature". Use "warmth" / "tone" / "mood" depending on context |
| **气质** | not "temperament". Use "character" or "feel" |
| **felt sense** | source already uses EN — keep |
| **正确 / 错误 / 不对** in tables | "Correct" / "Wrong" / "Incorrect" — caps optional, match source |

### Phrases that show up often

| Chinese (verbatim) | English (canonical) |
|---|---|
| 这是什么 | What this is |
| 怎么用 | How to use |
| 为什么 | Why |
| 必须按 design.md 决定 | Decide via design.md |
| 不要写死 | Don't hard-code |
| 自己写 | Write it yourself |
| 直接抄会带来主题脱节 | Copying directly causes theme mismatch |
| 文档保证它们对一次 | The doc gets it right once and for all |
| AI 自己写没问题 | Modern LLMs handle this fine |
| 不能凭感觉 | Don't eyeball it |
| 严格按 schema 写 | Follow the schema strictly |
| 一节一节填值 | Fill section by section |
| 一字不差 | verbatim / character-for-character |
| 拼 deck HTML 时 | when assembling the deck HTML |

## Sample reference: how to translate a paragraph

Source:
> 设计层是 4 层架构里的**主题原语层**。一份 deck 的视觉气质（用什么色、什么字、什么形、装饰系统怎么组织）由 design.md 一锤定音。

Translation:
> The design layer is the **theme primitives layer** in the 4-layer architecture. The visual character of a deck — color, type, shape, how the decorative system is organized — is settled once and for all by `design.md`.

Notes:
- "主题原语层" → keep the term **theme primitives layer** verbatim (canonical)
- "视觉气质" → "visual character" (not "aesthetic" — too vague here)
- "一锤定音" → "settled once and for all" (idiomatic, not literal)
- markdown bold preserved; backticks added on `design.md` (already-typeset filename)
