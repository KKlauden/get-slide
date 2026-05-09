# AGENTS.md

This repo packages the **getslide** skill — a 4-layer framework for building HTML slide decks (1920×1080, single-file, self-contained, full chrome runtime: presenter mode / overview / hash deep-link / preview / print).

## Hard rules (every AI working in this repo must follow)

- One deck = **one** `<deck>.html` file. No external resources; use data-URI when an asset is genuinely needed.
- 1920×1080 logical canvas; sizes in pixels — **no** `rem` / `vw` / `%`.
- New decks always start from `skills/getslide/framework/deck.html` (the canonical chrome runtime); **don't** rewrite the chrome runtime.
- 3-layer architecture: framework (chrome runtime + schema contracts) / `design.md` (theme contract) / `content.md` + `deck.html` (artifact).
- Token naming aligns with [shadcn](https://ui.shadcn.com/docs/theming) (15 standard names locked); **no** component-level secondary aliases.
- No npm runtime, no build step, no external CDN; the deck must open via `file://` directly.
- Every `<section class="page-shell">` must carry `data-title="..."` and embed a `<div class="notes">` (presenter verbatim).

## Skill routing

| Task | Entry point |
|---|---|
| Build a new deck (from scratch) | `skills/getslide/SKILL.md` top — **Pre-flight 3 questions** → 6-step Bootstrap |
| Restyle / add a slide / fix chrome on an existing deck | `skills/getslide/SKILL.md` Routing table |
| Author a new component / block / theme | same as above |
| Modify the chrome runtime canonical | `skills/getslide/framework/deck.html` (after editing, every generated deck must `cp` again) |
| Change architecture / decisions / roadmap | `skills/getslide/PLAN.md` (required reading) |

## How to mount this skill

- **Claude Code**: `cp -r skills/getslide ~/.claude/skills/getslide`, then restart Claude Code
- **Codex CLI / Gemini CLI / Cursor**: in your project's `AGENTS.md` / `GEMINI.md` / `.cursor/rules/`, add a snippet "when authoring a deck, reference `<path-to>/getslide/skills/getslide/SKILL.md`"
- **Future**: `npx getslide init` will scaffold a one-shot install into a new project (TODO)

Detailed install: [README.md](./README.md).

## Repo structure

```
getslide/
├── AGENTS.md                  ← you are reading this
├── CLAUDE.md                  ← stub "See AGENTS.md"
├── README.md                  ← human-readable + install
├── old/                       ← v1 archive — frozen
└── skills/getslide/           ← skill bundle (cp into ~/.claude/skills/getslide)
    ├── SKILL.md               ← canonical skill entry
    ├── PLAN.md                ← architecture / roadmap
    ├── framework/             ← deck.html / design.md / content.md / charts/
    └── samples/               ← pitch / archive — two case studies (NOT templates to copy)
```

Keep this file short — hard rules + routing only. Deeper guidance lives in `skills/getslide/SKILL.md` and `skills/getslide/PLAN.md`.
