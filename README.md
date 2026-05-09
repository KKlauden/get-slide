# getslide

> AI 写 HTML slide deck 的 4 层框架——一份单文件、1920×1080、self-contained，自带 chrome runtime（演讲者模式 / overview / hash 深链 / preview / print）。

## 是什么

把 PPT 风格的 deck 拆成 **4 层**：

| 层 | 是什么 | 文件 |
|---|---|---|
| framework | chrome runtime + design.md / content.md schema 契约 | `skills/getslide/framework/` |
| sample | 当 case study 读的参考主题（pitch / archive） | `skills/getslide/samples/<theme>/` |
| design | 主题契约（atmosphere / palette / typography / shape / variants） | per-deck `<my-deck>/design.md` |
| content | deck 大纲（每页内容 + chrome + notes） | per-deck `<my-deck>/content.md` |
| product | 单文件 deck.html 产物 | per-deck `<my-deck>/<my-deck>.html` |

> v2.1（2026-05-09）砍掉 `components/` + `blocks/` 目录——基础 UI（card / button / hero / numeral / pill 等）由现代 LLM 直接写。chart SVG 因为有数学几何，3 份 pattern 留在 `framework/charts/` 当参考。

写新 deck 时，AI 先跑 **Pre-flight 3 问**（受众 / 风格 / 起点），再走 6 步 Bootstrap：写 design.md → 写 content.md → cp framework/deck.html → 替换 § TOKENS / § SLIDES → 视觉自检 → 浏览器验证。

## 安装到不同 AI agent

### Claude Code（推荐）

```bash
git clone https://github.com/<owner>/getslide.git
cp -r getslide/skills/getslide ~/.claude/skills/getslide
# 重启 Claude Code，输入 "做一份关于 X 的 deck" 自动触发 skill
```

或符号链接：

```bash
ln -s "$PWD/getslide/skills/getslide" ~/.claude/skills/getslide
```

### Codex CLI / Gemini CLI / Cursor

这些 agent 没有「skill auto-discovery」概念，但都会 session 启动读 `AGENTS.md` / `GEMINI.md` / `.cursor/rules/`。在你 project 根目录的对应文件里加：

```markdown
# AGENTS.md（在你 project 根）

When the user asks to build a slide deck, follow the getslide skill:
- Skill canonical: <absolute path>/getslide/skills/getslide/SKILL.md
- Pre-flight 3 questions: read SKILL.md "Pre-flight" section
- Bootstrap 6 steps: read SKILL.md "Bootstrap from scratch" section
- Hard rules: read this repo's [AGENTS.md](<path>/getslide/AGENTS.md)
```

或者直接在你 project 拷一份 skill bundle：

```bash
cp -r <path>/getslide/skills/getslide ./skills/getslide
# 让 agent 在 cwd 就找到 skill 内容
```

### 一键 scaffolder（待做）

未来：

```bash
npx getslide init my-deck-project
```

会自动 scaffold 项目结构 + 安装 skill bundle + 配好 AGENTS.md / CLAUDE.md。

## Repo 结构

```
getslide/
├── AGENTS.md                  ← 项目 hard rules + skill 路由（短）
├── CLAUDE.md                  ← stub「See AGENTS.md」
├── README.md                  ← 你正在读这份
├── old/                       ← v1 archive
└── skills/getslide/           ← skill bundle
    ├── SKILL.md               ← skill canonical 入口（Claude Code 自动发现）
    ├── PLAN.md                ← 架构决策 / 路线
    ├── framework/
    │   ├── deck.html          ← chrome runtime canonical HTML 骨架
    │   ├── design.md          ← 主题设计契约 schema
    │   └── content.md         ← 内容结构契约 schema
    │   └── charts/            ← 3 份 chart SVG pattern（line / bar / gauge）—— AI 写 chart 时参考
    └── samples/
        ├── pitch/             ← YC / TechCrunch 风 9 页 sample
        └── archive/           ← 工业档案风 FAULTLINE 9 页 sample
```

## 状态

- ✅ chrome runtime（topbar / sidebar / 翻页 / 演讲者 / overview / hash / preview / print）
- ✅ framework/ 三件套（deck.html / design.md / content.md schema）
- ✅ pitch + archive 两份完整 sample
- ✅ Pre-flight 3 问 gate（SKILL.md 顶部）
- ⏳ npx getslide init scaffolder
- ⏳ 提交 Anthropic plugin marketplace（plugin 转换待做）

详细路线见 [`skills/getslide/PLAN.md`](./skills/getslide/PLAN.md)。

## 设计原则

1. **单文件 deck**——deck 是 archivable 文档，不是动态 app
2. **token 锁死 shadcn 命名**——避免命名漂移
3. **chrome runtime 是 canonical**——所有 deck 共享同一份运行时
4. **design.md 是主题契约**——CSS 由 §8 程序生成，不在 design.md 写 raw CSS（除 §8 Append 段）
5. **content.md 是内容大纲**——每页 4 子段（block / variant / content / chrome / notes）

## License

MIT（待加）。
