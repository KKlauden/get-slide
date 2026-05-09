# getslide

> A 4-layer framework for AI to author HTML slide decks — single-file, 1920×1080, self-contained, with a built-in chrome runtime (presenter mode / overview / hash deep-link / preview / print).

## What it is

Split a PPT-style deck into **4 layers**:

| Layer | What it is | Files |
|---|---|---|
| framework | chrome runtime + design.md / content.md schema contracts | `skills/getslide/framework/` |
| design | theme contract (atmosphere / palette / typography / shape / variants) | per-deck `<my-deck>/design.md` |
| content | deck outline (per-page content + chrome + notes) | per-deck `<my-deck>/content.md` |
| product | single-file `deck.html` artifact | per-deck `<my-deck>/<my-deck>.html` |

> Alongside the framework, `skills/getslide/samples/<theme>/` ships maintained reference themes (pitch / archive) — read them as DNA / case studies, NOT as templates to fill.

> v2.1 (2026-05-09) cut the `components/` and `blocks/` directories — basic UI (card / button / hero / numeral / pill etc.) is written directly by modern LLMs. Chart SVGs carry mathematical geometry, so 3 chart patterns stay in `framework/charts/` as reference.

When authoring a new deck, the AI runs **Pre-flight 3 questions** (audience / style / starting point) first, then walks the 6-step Bootstrap: write `design.md` → write `content.md` → `cp framework/deck.html` → substitute `§ TOKENS` / `§ SLIDES` → visual self-check → browser verification.

## Install

This skill is just files — no build, no npm. Pick one path:

### Option A — Global (Claude Code, recommended)

Install once, works in every project:

```bash
git clone https://github.com/KKlauden/get-slide.git
mkdir -p ~/.claude/skills
cp -r get-slide/skills/getslide ~/.claude/skills/getslide
```

Restart Claude Code; the skill auto-discovers via SKILL.md frontmatter.

> Note: the GitHub repo is `get-slide` (kebab-case URL), but the skill bundle inside is `getslide` (no hyphen, matching SKILL.md `name:`). The cp target must be `~/.claude/skills/getslide` — Claude Code matches the directory name to the SKILL frontmatter.

Update later: `cd get-slide && git pull && cp -r skills/getslide ~/.claude/skills/`.

(Or symlink instead of cp: `ln -s "$PWD/get-slide/skills/getslide" ~/.claude/skills/getslide` — `git pull` then propagates without re-cp.)

### Option B — Project-local (Codex / Cursor / Gemini / pinned CC version)

Install into a deck project (useful for non-CC agents, or to pin a getslide version per project):

```bash
cd my-deck-project
git clone https://github.com/KKlauden/get-slide.git ../get-slide
mkdir -p .claude/skills
cp -r ../get-slide/skills/getslide .claude/skills/getslide
```

Then add to your project's agent protocol file (`AGENTS.md` / `GEMINI.md` / `.cursor/rules/`):

```
When asked to build a slide deck, read ./.claude/skills/getslide/SKILL.md and follow it.
```

(Claude Code reads `.claude/skills/` automatically — for CC users, the protocol line is optional.)

## Build your first deck

After installing, make a folder for your decks and run your AI agent from there — otherwise the AI will create deck folders wherever you happen to `cd`:

```bash
mkdir -p ~/decks && cd ~/decks
```

Open Claude Code (or another agent set up via Option B) and say:

> Build me a deck about &lt;topic&gt;.

The skill auto-triggers and walks you through **Pre-flight 3 questions** (audience / style / starting point), then generates `<my-deck>/` containing `design.md`, `content.md`, and `<my-deck>.html`.

## Using a deck

Open `<my-deck>/<my-deck>.html` in any browser. Self-contained — no server, no build, no external CDN.

Keyboard shortcuts (handled by the chrome runtime):

| Keys | Effect |
|---|---|
| ← → ↑ ↓ PgUp PgDn Space | page navigation |
| `1`–`9` | jump directly to page N |
| `F` | fullscreen / presenter mode |
| `S` | presenter window (NEXT slide / SCRIPT / TIMER cards) |
| `O` | overview grid |
| `Esc` | close overlay |
| `Cmd/Ctrl + P` | print to PDF — one slide per page |
| `?preview=N` URL parameter | single-page chrome-less mode (used by presenter iframe) |

Decks are **self-contained snapshots**. Updating the skill (`git pull` + re-cp) does NOT affect already-generated decks — they keep the chrome runtime they were built with. Only newly-generated decks pick up framework updates.

## Repo structure

```
getslide/
├── CLAUDE.md                  ← project hard rules + skill routing (for AIs working in this repo)
├── README.md                  ← you are reading this (GitHub landing)
├── PLAN.md                    ← architecture decisions / roadmap (Chinese — internal)
├── old/                       ← v1 archive
└── skills/getslide/           ← skill bundle (the distribution unit)
    ├── SKILL.md               ← skill canonical entry (Claude Code auto-discovers)
    ├── framework/
    │   ├── deck.html          ← canonical chrome runtime HTML skeleton
    │   ├── design.md          ← theme design contract schema
    │   ├── content.md         ← content structure contract schema
    │   └── charts/            ← 3 chart SVG patterns (line / bar / gauge) — referenced when writing charts
    └── samples/
        ├── pitch/             ← YC / TechCrunch-style 9-page sample
        └── archive/           ← industrial-archive-style FAULTLINE 9-page sample
```

## Status

- ✅ chrome runtime (topbar / sidebar / paging / presenter / overview / hash / preview / print)
- ✅ framework triplet (deck.html / design.md / content.md schemas)
- ✅ pitch + archive — two complete samples
- ✅ Pre-flight 3 questions gate (top of SKILL.md)
- ⏳ Submit to Anthropic plugin marketplace (plugin conversion TODO)

Detailed roadmap: [`PLAN.md`](./PLAN.md) (Chinese — internal decisions log).

## Design principles

1. **Single-file deck** — a deck is an archivable document, not a dynamic app
2. **Token names locked to shadcn** — avoids naming drift
3. **Chrome runtime is canonical** — every deck shares the same runtime
4. **`design.md` is the theme contract** — CSS is generated by §8; raw CSS does not live in `design.md` (except the §8 Append block)
5. **`content.md` is the content outline** — every page has 4 sub-blocks (block / variant / content / chrome / notes)

## License

[MIT](./LICENSE) © 2026 KKlauden.

Issues / contributions welcome — [GitHub Issues](https://github.com/KKlauden/get-slide/issues).
