# AGENTS.md

This repo packages the **getslide** skill — a 4-layer framework for building HTML slide decks (1920×1080, single-file, self-contained, full chrome runtime: presenter mode / overview / hash deep-link / preview / print).

## Hard rules（任何 AI 在这个 repo 里干活都遵守）

- 一份 deck = **一个** `<deck>.html` 文件。无外部资源，必要时 data-URI
- 1920×1080 逻辑画布；尺寸用像素，**不准** `rem` / `vw` / `%`
- 写新 deck 起点恒为 `skills/getslide/framework/deck.html`（chrome runtime canonical），**不**重写 chrome runtime
- 4 层架构：component（原子）/ block（组合）/ design.md（主题契约）/ deck.html（sample 产物）
- token 命名对齐 [shadcn](https://ui.shadcn.com/docs/theming)（15 个标准名锁死）；**不**设 component 二级 alias
- 不引 npm runtime / 不加 build step / 不引外部 CDN；deck 必须能 `file://` 直接打开
- 每个 `<section class="page-shell">` 必填 `data-title="..."`，内嵌 `<div class="notes">`（演讲者逐字稿）

## Skill 路由

| 任务 | 入口 |
|---|---|
| 做新 deck（从零） | `skills/getslide/SKILL.md` 顶部 **Pre-flight 3 问** → Bootstrap 6 步 |
| 已有 deck 改样 / 加页 / 修 chrome | `skills/getslide/SKILL.md` Routing 表 |
| 写新 component / block / theme | 同上 |
| 改 chrome runtime canonical | `skills/getslide/framework/deck.html`（改完所有已生成 deck 需重 cp） |
| 改架构 / 决策 / 路线 | `skills/getslide/PLAN.md`（必读） |

## 怎么挂这个 skill

- **Claude Code**：`cp -r skills/getslide ~/.claude/skills/getslide`，重启 Claude Code
- **Codex CLI / Gemini CLI / Cursor**：在你 project 的 `AGENTS.md` / `GEMINI.md` / `.cursor/rules/` 里加一段「写 deck 时参考 `<path-to>/getslide/skills/getslide/SKILL.md`」
- **未来**：`npx getslide init` 一键 scaffold 到新项目（待做）

详细安装见 [README.md](./README.md)。

## Repo 结构

```
getslide/
├── AGENTS.md                  ← 你正在读这份
├── CLAUDE.md                  ← stub「See AGENTS.md」
├── README.md                  ← 人读 + 安装
├── old/                       ← v1 archive，不动
└── skills/getslide/           ← skill bundle（cp 到 ~/.claude/skills/getslide）
    ├── SKILL.md               ← canonical skill 入口
    ├── PLAN.md                ← 架构 / 路线
    ├── framework/             ← deck.html / design.md / content.md 三件套
    ├── components/            ← 10 个原子
    ├── blocks/                ← 11 个组合
    └── samples/               ← pitch / archive 两份样板
```

Keep this file short——hard rules + 路由 only。Deeper guidance 在 `skills/getslide/SKILL.md` 和 `skills/getslide/PLAN.md`。
