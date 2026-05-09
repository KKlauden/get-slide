---
name: get-slide
description: Use for any task involving a get-slide v2 HTML slide deck — building a new deck, swapping themes, adding/editing a slide, authoring a new theme spec, debugging chrome runtime (sidebar/present/scaling/print), or chart pattern reference. Triggers on phrases like "make me a deck about X", "build a presentation", "create slides", "switch to <X> theme", "add a new theme", "add a new slide type", "fix the chrome", "thumbnails wrong", "present mode broken", "this slide is too crowded".
---

# get-slide v2

Build a get-slide deck. **4-layer architecture**: framework (chrome runtime + schema contracts) + design (theme primitives, per deck) + content (deck outline, per deck) + product (single-file `<deck>.html` artifact).

## Quick context

- **Currently available resources**:
  - `framework/` — the **contract layer**, the v2 core; required reading when authoring a new deck
    - `framework/deck.html` — **directly copyable HTML skeleton**: full chrome runtime (topbar / sidebar / canvas / paging / presenter / overview / preview / print) + placeholder `§ TOKENS` block + placeholder `§ SLIDES` block. For a new deck, `cp` once and fill in the blanks
    - `framework/design.md` — theme design contract; teaches the AI how to author `design.md` (atmosphere / palette / typography / shape / variants / decorative signature / Do's & Don'ts schema + §8 Append CSS + Known Gaps)
    - `framework/design-blank-template.md` — blank skeleton for new themes; `cp` to start
    - `framework/design-tokens-template.css` — canonical CSS token block template (substitution by §8)
    - `framework/content.md` — content structure contract; teaches the AI how to author `content.md` (per-page 4 sub-fields schema + the §3 three rules for notes + page rhythm + templates)
  - `references/` — **on-demand pattern docs**, NOT part of the framework contract layer; the AI reads these only when a specific authoring task needs them
    - `references/charts/` — chart SVG patterns (line / bar / gauge): **structure + algorithm contracts, NOT visual templates**. Read only when the deck contains a chart. General UI (card / button / hero / numeral / pill / list) is written directly by modern LLMs without consulting any reference doc; chart geometry is the exception because pixel-correct alignment is a math problem, not an aesthetic one
  - `samples/<deck>/` — **reference samples** we maintain (pitch / archive). **Read as case studies, NOT as templates to copy** — learn their visual decisions, variant rhythm, abs-pinning paradigm; **don't** copy the entire §8 Append CSS verbatim
    - `samples/pitch/` (lite/dark/forest variants + 9-page YC-style pitch)
    - `samples/archive/` (industrial-archive black-and-white + 2 variants + FAULTLINE fictional geological-monitoring deck, 9 pages)

> **Important**: v2 has **cut** the `components/` and `blocks/` directories — basic UI (card / button / hero / numeral / pill / cards-row, etc.) is written directly by modern LLMs in HTML+CSS, no need to maintain 21 small docs. Chart SVGs carry mathematical geometry (3 patterns kept under `references/charts/`, read on demand).

## Routing

| Task | Entry point |
|---|---|
| **Author a new deck** | 0) Run **Pre-flight (iterative)** — get the user's 5 sign-offs (audience / decision context / style DNA / outline / starting point) one round at a time 1) Read the `framework/{deck.html, design.md, content.md}` triplet 2) Create `<my-deck>/` at the user's chosen location 3) Write `<my-deck>/design.md`, ⚑ checkpoint, then `<my-deck>/content.md` 4) `cp framework/deck.html → <my-deck>/<my-deck>.html` 5) Substitute `§ TOKENS` (per design.md §8) + substitute `§ SLIDES` (per content.md, page by page) 6) ⚑ Browser verification |
| **Switch a deck's style** (without changing content) | Edit the deck's `content.md` `theme:` to point at another design.md; regenerate the `§ TOKENS` block per `framework/design.md` §8 |
| **Author a new theme** | Strictly follow `framework/design.md` §1–§7 schema in the deck folder's `design.md`: Atmosphere prose + Palette / Typography / Shape values + Variants + Decorative signature + Do's & Don'ts. **§1–§7 contain NO CSS** — all CSS goes into §8 Append CSS |
| **Author a new deck outline** | Strictly follow `framework/content.md` §1 schema in the deck folder's `content.md`: per-page H2 + 4 sub-fields (block / variant / content / chrome / notes). Notes must follow the §3 three rules |
| **Author per-page UI (card / hero / list / pill / cards-row, etc.)** | Write HTML+CSS directly in `deck.html`, using `var(--xxx)` to consume design.md tokens; **don't** consult `components/` or `blocks/` (deleted). For layout-feel reference, look at `samples/<theme>/<theme>.html` for actual implementation |
| **Author a chart (line / bar / gauge)** | Read `references/charts/<X>.md` for the SVG template + CSS pattern; inline into `deck.html`, override colors per design.md theme. Plain sparklines: write yourself |
| **Modify chrome (topbar / sidebar / paging / presenter / overview / preview)** | `framework/deck.html` is the canonical source; **after editing, every previously generated `<deck>.html` must be regenerated by cp-ing `framework/deck.html` again**. Colors use the `--chrome-*` namespace (7 tokens), with defaults supplied by design.md |
| **Add notes / presenter scripts** | Embed `<div class="notes">…</div>` inside each `<section class="page-shell">`; CSS auto-hides it; the S key opens the presenter window which reads the notes. Three writing rules: prompts not full scripts / 150–300 chars (CN) or 150–200 words (EN) / use spoken style |
| **Fix per-page layout / typography / overflow** | Edit the `styling` sub-field in `content.md`, or use inline style on the page in the deck |

## Hard rules (do not violate)

### Visuals and naming
- **Tokens (color / typography / spacing / radius / shadow) use only `var(--xxx)`** — never hard-code these; they are injected by the spec via the `§ TOKENS` block
- **Layout primitives** (chrome 8-anchor positions, page-shell padding, abs page coordinates, decorative-motif geometry) **are written in raw px** inside §8 Append CSS — px is unavoidable on a 1920×1080 abs canvas. **But every coordinate must derive from the current page's content shape** (estimate text length × line-height vs container, see step 7 visual self-check) — **never copy abs coordinates verbatim from a sample**, they were measured for the sample's original copy
- **No secondary aliases** (no `--card-bg / --card-padding-x` layer). CSS uses `var(--card)` / `var(--space-6)` directly, sourced from palette + scale
- **Palette names align with shadcn**: `--background / --foreground / --card / --card-foreground / --primary / --primary-foreground / --secondary / --secondary-foreground / --muted / --muted-foreground / --accent / --accent-foreground / --destructive / --destructive-foreground / --border`
- **Chrome uses the `--chrome-*` namespace** (7 standalone tokens: bg / canvas-bg / fg / muted-fg / border / accent / accent-fg); spec provides defaults

### Slide structure (chrome runtime hard constraints — every new deck must follow)
- **Slide container is 1920×1080 + page-shell** (provided by the spec); `.slide-shell > .page-shell` is a hard convention
- **Every `<section class="page-shell">` must carry `data-title="..."`** — the O-key overview grid and the S-key presenter NEXT card both rely on this; missing it falls back to "Slide N", which looks ugly
- **Every page-shell should embed `<div class="notes">…</div>`** (presenter verbatim). CSS keeps `.slide-shell .notes { display: none }` permanently hidden; the S-key presenter window pulls it out for display
- **Hard rule**: presenter-facing descriptive text ("this slide talks about ...", grey explanation captions) **must** go inside `.notes`; **never** leak into the body `<p>`/`<span>`. Slide bodies hold only audience-facing titles / data / images
- Three rules for writing notes: ① not a script — they are **prompts** (key terms in **bold** + transition sentences as separate paragraphs) ② 150–300 chars (CN) / 150–200 words (EN) per page (≈ 2–3 minute pace) ③ spoken style, not written style
- **Chrome ring 8-anchor element types and class names are fixed**: `.chrome-top-left/-right/-bottom-left/-bottom-right` are `<div>`s, `.chrome-rule` is an `<hr>`. The spec only fills text / SVG inside the anchors

### Runtime behavior (the source of CSS+JS inlined into deck HTML)
- **Every deck's HTML must be cp-ed from `framework/deck.html`** — it carries the canonical `§ RUNTIME CSS` and `§ RUNTIME JS` blocks
- Only two blocks are editable: **`§ TOKENS`** (substituted per design.md) and **`§ SLIDES`** (substituted per content.md); touching any other canonical line breaks the runtime
- Runtime entry points: preview mode (`?preview=N`) / hash deep-link (`#/N`) / presenter window (S key) / overview grid (O key) / theme cycling (T key — only when the deck has external CSS files) / number keys 1–9 jump to page
- The presenter window syncs with the main window via BroadcastChannel; the preview iframe switches pages without reloading via `postMessage({type:'preview-goto', idx:N})`

### File structure
- **One `content.md` per deck, one `design.md` per theme** (formerly `spec.md`) — don't merge content / theme into a component
- **`design.md` strictly follows `framework/design.md` §1–§7 schema**, no CSS code — CSS is generated by `framework/design.md` §8
- **`content.md` strictly follows `framework/content.md` §1 schema**; the 4 sub-fields are required per page; notes follow the three rules

## When to refuse

- "Make the slide responsive" / "add a build step" / "introduce an npm runtime" — v2 is **single-file self-contained HTML**
- "Hard-code a color in a component" — every visual value must be tokenized
- "Add a secondary alias to design.md" — v2 has explicitly removed them (B2 roadmap)
- "Rename a palette token" — names are locked to shadcn

## Pre-flight (iterative)

**Trigger**: from-scratch requests — "make me a deck about X" / "build a deck about Y" / "make a demo-day pitch" / etc.

**Skip Pre-flight when**:
- The user already has a deck folder and wants to restyle / edit a single page / add a page
- The opening request already provides all 5 sign-offs (audience + decision context + style DNA + outline + starting point)

**Why iterative, not 3-in-1**: a single 3-question dump forces the user to answer everything in one breath — answers come back vague ("modern style", "9 pages whatever"), forcing the AI to guess or re-ask. **One round at a time + drill into each answer** gets concrete decisions; concrete decisions get aligned decks.

**Hard rule**: by the end of Pre-flight, **5 things** must be signed off — `audience / decision context / style DNA / narrative outline (≤9 lines) / starting point + output location`. Silent AI decisions on any of these = 90% chance of rework.

### Round 1 — Audience + decision context

Ask audience first, then **immediately drill** into the decision behind it:
- "Who's the audience?" — VC pitch / customer demo / team all-hands / engineering deep-dive / industry keynote / Xiaohongshu / other
- Drill: "What decision are they making after seeing this? How long do they have? One-way or interactive?"

Drill-down matters: "VC pitch" → 10-min slot, decide a follow-up meeting; "customer demo" → 30-min slot, decide a pilot. Shapes narrative pace and density downstream — don't skip.

If the user supplied a source document (Markdown / report / spec), **read it now** before Round 2.

### Round 2 — AI proposes narrative + style + page count

The AI does the heavy lift here, NOT the user. Output all three in one message:

1. **Narrative outline** (≤9 lines, one per page) — the through-line / what each page is arguing. Phrase as one-line page titles. Example: `P1 cover · P2 core judgment · P3 data reality · P4 algo path · P5 product architecture · P6 compliance · P7 IP risk · P8 milestones · P9 trade-off`. Outline = bones; once locked, content.md just translates each line into block / variant / content / chrome / notes.

2. **Style candidates** (2–3 ranked):
   - **pitch** — YC / TechCrunch modern-tech: Cabinet Grotesk + 3 variants (lite/dark/forest) + geometric decoration (M18 ambient frames / M20 pill highlight) + full chrome ring. Fits standard pitches / product launches / brand decks.
   - **archive** — industrial archive: Geist + B&W 2 variants + mono system-label chrome + large hero (≥112px) + `--radius: 0` + abs-pinning. Fits engineering / system / data-driven decks.
   - **blank** — neither fits; new theme from `framework/design-blank-template.md`.
   - **If the user supplied a reference image, FIRST verbalize the visual DNA you read** (3–5 lines: dominant colors / type system / signature elements / chrome character / mood) and let the user correct your interpretation BEFORE committing to a style. Image misread is the #1 source of design.md rework.

3. **Page count** — based on source-doc density (if any) + audience time slot. Argue for a number; don't default to 9 reflexively.

User picks / edits. If they hand-edit the outline, lock it verbatim — it becomes the spine of content.md.

### Round 3 — Starting point + output location

After Round 2 confirms style:
- Style = **pitch** / **archive** → confirm "use `samples/<theme>/` as DNA reference (layout will be redesigned for new content shape)?" → Path A in Bootstrap. Sample provides **DNA** (skin / mood) not **bones** (page structure); its abs coordinates / 30+ `<theme>-*` selectors were measured for the sample's original copy and **must NOT be copied verbatim**.
- Style = **blank** → Path B in Bootstrap.
- **Output location**: where to create `<my-deck>/`? Default = the user's current working directory (NOT under `samples/` — that's reserved for maintained reference samples).

### After Round 3

1. **Paraphrase all 5 sign-offs back** + wait for confirmation: "audience=X / decision context=Y / style DNA=Z / 9-line outline=[…] / starting point=Path A or B + output at `<path>`".
2. User confirms → enter the [Bootstrap flow](#bootstrap-from-scratch). **From here, execution is streaming** — no interruptions until the design.md ⚑ checkpoint.
3. If the user stays vague at any round ("modern style" / "5–9 pages, whatever") — push for specifics; do NOT proceed on guesses.

---

## Bootstrap from scratch

**Prerequisite**: when authoring a new deck from scratch, finish the [Pre-flight (iterative)](#pre-flight-iterative) above first.

**Starting point**: read `framework/deck.html` (HTML skeleton) + `framework/design.md` (theme contract) + `framework/content.md` (content contract). These three are the v2 contracts; understand them before starting.

### Starting paths (decided after Pre-flight Round 2)

> **Core principle**: **Style is the skin, content is the bones and flesh. Skin must grow FROM the flesh — not be lifted off a sample and pasted on.**
> A sample provides DNA (type / color / shape / chrome character / decorative system), but **every page's layout must be redesigned for the new content shape** — abs coordinates, card widths, row counts will all differ from the original sample.

- **Path A: build on a sample's theme contract + design your own layout** (Round 2 style = **pitch** or **archive**)

  1. **Read the full** `samples/<theme>/{design.md, content.md, <theme>.html}` set — focus on §1 Atmosphere (the mood) / §2 Palette / §3 Typography / §6 Decorative signature. **Internalize the theme DNA**
  2. In `<my-deck>/design.md`:
     - §1–§7 may **reference** the sample's wording, but **§1 Atmosphere must be rewritten for the new theme** (all 4 prose paragraphs); §2 Palette adjusts to the new color tone; §6 Decorative motif gets a new narrative
     - §3 Typography / §4 Shape / §5 Variants / §7 Do's & Don'ts can usually carry over from the sample (unless the user explicitly asks to change)
     - **§8 Append CSS does NOT carry over verbatim**: keep theme-level paradigms (chrome anchor positions / page-shell / grid / utility classes); **delete** all page-specific selectors from the sample (e.g. `archive-page-title / archive-stat-list / archive-coverage-panel`, 30+ of them) — they are coordinates measured for the sample's original copy, NOT theme contract
  3. Write `content.md` for the new theme's 9-page rhythm yourself (sample rhythm is reference, not law)
  4. `§ SLIDES`: design each page's layout **per its content shape** — basic UI (card / hero / list / pill) is HTML+CSS that consumes design.md tokens; abs coordinates are derived from the current page's text length

- **Path B: blank-slate start** (Round 2 style = **blank** or **"reference an image / other visual material"**)

  1. Fill in §1–§7 from scratch using the `framework/design-blank-template.md` skeleton
  2. If the user supplies a reference image: first Read the image and write a 200–300 word "what I see" visual-DNA note (color / type / shape / motif / chrome / rhythm), **then** start writing design.md. §1 Atmosphere must echo the observation note
  3. §8 Append CSS is your own design (chrome 8-anchor positions + page-shell padding + whether to use grid + decorative motifs)
  4. content.md / `§ SLIDES`: same as Path A steps 3–4

Both paths feed into the **streaming execution flow** below.

### Streaming execution (with ⚑ checkpoints)

**Decision points vs streaming steps**: ⚑ marks a step that **must stop and wait for user confirmation**; all other steps are **streaming** — execute continuously, do NOT interrupt the user. All "is the audience right / is the narrative right / is the style right" decisions belong in Pre-flight; do NOT re-litigate them in execution. By design, only **two** ⚑ checkpoints exist below — after design.md (token-translation gate) and at final browser verification (canonical visual gate).

1. **Create the deck folder at the user's chosen location** `<my-deck>/` (NOT forced under `samples/`; `samples/` is reserved for our maintained reference samples — not the home for new decks).
2. Write `<my-deck>/design.md`: Path A is `cp` + local edits; Path B writes from scratch per `framework/design.md` schema.
3. ⚑ **design.md checkpoint** — present to the user: **(a)** the resolved 15-token table (palette + chrome `--chrome-*`) and **(b)** a 30-second mock of one signature page in plain text (e.g. "Cover: 112px hero in lime on graphite-black, mono label `01·09 · COVER` top-right, page-shell padding 96"). **Wait for user OK** before continuing. Reason: token misread caught here costs ~5 min to fix; caught after step 6 (full slides built), 30+ min.
4. Write `<my-deck>/content.md` per `framework/content.md` §1 schema. **No checkpoint here** — the narrative outline was locked in Pre-flight Round 2; content.md is just translating each outline line into the 4 sub-fields. (Path A may follow the sample content.md's page rhythm; Path B is free.)
5. **`cp framework/deck.html <my-deck>/<my-deck>.html`** — pull in the full chrome runtime once.
6. In `<my-deck>.html` edit only two blocks:
   - **`§ TOKENS`** block (`:root { ... }`): substitute design.md values per `framework/design.md` §8 + paste design.md's §8 Append CSS verbatim below the `:root` block (Path A: copy from the sample's design.md; Path B: your own)
   - **`§ SLIDES`** block: per content.md, generate one `<div class="slide-shell">` per page; every page-shell must include `data-title` + `<div class="notes">`
   - Update `<title>` to the deck name; update topbar `<span class="title">` to the deck name
   - **Don't touch `§ RUNTIME CSS` / `§ RUNTIME JS`** — they are canonical, one line off and they break

7. **Visual self-check (mental test, after every page; fix issues immediately)**:

   - **Abs-element bboxes don't overlap**: for every abs-pinned element on a page, derive bbox from `top + height` and `left + width`; **any pairwise intersection is a bug**
   - **Doesn't hit chrome**: content top ≥ chrome-top anchor bottom (typically ≥ 130–280 depending on chrome size); content bottom ≤ chrome-bottom anchor top (typically leave 96–160px); left/right keeps grid-margin-x (usually 64–96)
   - **Wrapped text fits**: for `<h1 class="hero">` and long body paragraphs, estimate the line count for the current text × line-height (line-height × font-size); the total cannot exceed container height; large hero (≥88px) overflows easily
   - **CJK length cap** (when copy is predominantly Chinese / Japanese / Korean): chars-per-line ≈ `container_width / font_size` for CJK (vs ~2× that for Latin). 1920px container @ 88px hero → ~21 CJK chars max; over that, **deliberately break into 2 lines** at a phrase boundary — auto-wrap lands ugly. Leading also needs +0.05–0.10 over Latin defaults. Full ruleset: `framework/design.md` §3 → CJK considerations
   - **Hero leading has air**: `font-size: 88px` and up, `line-height` ≥ 0.95 (tight) / 1.05 (loose); multi-line hero lines must not collide
   - **Chrome text doesn't crowd**: archive theme's chrome top-right with multi-line mono labels — watch the right margin and the longest row's width; pitch theme's chrome-top-left logo + brand must not be covered by the hero text
   - **§8 abs coordinates derive from current text**: Path A may NOT copy the sample's `archive-*` coordinates verbatim — those were measured for the sample's original copy; new copy with different word count must be re-laid

8. ⚑ **Browser verification** — open `<my-deck>.html` in a browser (mental test does NOT replace it):
   - ← → paging, URL syncs to `#/N`
   - O → overview grid (verify thumbnails are readable)
   - S → presenter window (NEXT card uses `data-title`; SCRIPT card uses `<div class="notes">`)
   - `?preview=N` → single-page chrome-less mode
   - **Visually pass each page**: check for overlap / chrome collision / ugly wrapping / inconsistent type / washed-out colors; on any failure, return to step 7 and fix

### § Post-execution sanity (run after step 8 passes, before declaring done)

These are mechanical checks an AI agent must run before saying the deck is finished. They catch the failure modes that step 7 mental-test and step 8 eyeball-test miss — leftover placeholders, off-by-one page counts, missing `data-title` / `notes`. Treat any failure as a deck bug and fix in place; do NOT report completion.

| Check | Command (run from `<my-deck>/`) | Pass condition |
|---|---|---|
| No `§ TOKENS` / `§ SLIDES` / `placeholder-page` leaked into visible HTML | `grep -nE '§ TOKENS\|§ SLIDES\|placeholder-page' <my-deck>.html` | Only matches inside framework canonical comments (the `§ TOKENS` / `§ SLIDES` markers in the source comments are fine; the `placeholder-page` class block from `framework/deck.html` may also remain as dead CSS but must not appear in any `<section class="page-shell">`) |
| Slide count matches Pre-flight Round 2 outline | `grep -c 'class="slide-shell"' <my-deck>.html` | Equals the page count agreed in outline |
| Every slide has `data-title` | `grep -oE 'class="page-shell"[^>]*' <my-deck>.html \| grep -c 'data-title='` | Equals slide count |
| Every slide has `<div class="notes">` | `grep -c 'class="notes"' <my-deck>.html` | Equals slide count (≥, since runtime CSS may also reference `.notes`) |
| Contact sheet (cross-page consistency gut-check) | playwright / chrome headless: load `?preview=1` … `?preview=N`, screenshot each, then `montage` (ImageMagick) into one PNG | Inspect for typographic drift / color drift / hero overflow / ugly wraps you missed in step 7 |

The contact sheet is the highest-value check here — most layout drift only becomes obvious when 9 pages sit side by side. Burn 30 seconds on it even if step 7 + 8 looked fine.

**AI default behavior when generating a new deck**:
- Don't rewrite `§ RUNTIME CSS` / `§ RUNTIME JS` — they belong to `framework/deck.html` and must stay verbatim
- Every page gets `data-title`; at minimum the cover and closing pages have a filled `<div class="notes">` (intermediate pages may leave it empty, but the div must exist)
- Chrome 8-anchor class names (`chrome-top-left/-right/-bottom-left/-bottom-right/-rule`) cannot be changed
