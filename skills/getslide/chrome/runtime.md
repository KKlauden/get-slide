---
name: runtime
deps: []
tokens:
  - --chrome-bg
  - --canvas-bg
  - --chrome-fg
  - --chrome-muted-fg
  - --chrome-border
  - --chrome-accent
  - --chrome-accent-fg
  - --font-body
source: "迁移自 old/skills/getslide/runtime.html — CSS 视觉抄 v1，token 用 chrome-* 独立 namespace"
---

# Chrome Runtime

deck 级别的运行时外壳——topbar + sidebar 缩略图 + 中央 viewport（自动 scale-to-fit）+ 键盘 / 鼠标翻页 + present 全屏模式。

**原则**：
- **跨主题通用**——chrome 不知道 spec 的视觉细节，只管"deck 怎么浏览"
- **几何参数硬编码**（topbar 高 48px / sidebar 宽 264px / thumb-frame 184×103.5px / padding 12px 等）——chrome 尺寸是它自己的事
- **颜色用 chrome-namespace token**——`--chrome-bg / --chrome-fg / --chrome-muted-fg / --chrome-border / --chrome-accent / --chrome-accent-fg / --canvas-bg`。spec 给默认值（通常指向 palette），主题可单独改 chrome 视觉而不影响 slide 内部

## Token 接口（chrome 暴露给 spec 的 7 个 token）

| chrome token | 控制 | 推荐默认 |
|---|---|---|
| `--chrome-bg`        | topbar / sidebar 底色 | 主题自定（lite 风通常是浅灰） |
| `--canvas-bg`        | canvas 外围底色 | 主题自定 |
| `--chrome-fg`        | topbar 标题、icon-btn 默认文字 | `var(--foreground)` |
| `--chrome-muted-fg`  | PAGES / thumb-num / sidebar-head 次要文字 | `var(--muted-foreground)` |
| `--chrome-border`    | topbar/sidebar/thumb-frame 描边、hairline-v | `var(--border)` |
| `--chrome-accent`    | icon-btn.active / present-btn 底 / thumb.active 高亮 | `var(--accent)` |
| `--chrome-accent-fg` | present-btn 文字 | `var(--accent-foreground)` |

**主题想 chrome 跟 slide 解耦**（比如 slide 是 dark，chrome 用浅色编辑器风）：在 spec 里把 `--chrome-bg / --chrome-fg / --chrome-accent` 等改成不同值即可，不影响 `--background / --foreground` 给 slide 用。

## Template

```html
<!-- body 上声明可选的多 theme 链接（T 键循环） -->
<body
  data-themes="dark,lite,forest"
  data-theme-base="../themes/"
>
<!-- 注：data-themes 是可选的；不写则 T 键无操作。
     若写了，必须同时有 <link id="theme-link" href="..."> 在 head 里。
     适用于多 spec 文件的 deck（外置 CSS 文件）；自包含 deck 不写。 -->

<div class="app">
  <header class="topbar">
    <div class="topbar-left">
      <button class="icon-btn" id="toggleSidebar" title="Toggle sidebar">
        <svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5">
          <rect x="2" y="3" width="12" height="10" rx="1.5"/>
          <line x1="6" y1="3" x2="6" y2="13"/>
        </svg>
      </button>
      <div class="hairline-v"></div>
    </div>
    <div class="topbar-center">
      <span class="title">{deck title}</span>
    </div>
    <div class="topbar-right">
      <button class="icon-btn" id="downloadBtn" title="Export PDF">
        <svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M8 2v8M5 7l3 3 3-3M3 13h10"/>
        </svg>
      </button>
      <div class="hairline-v"></div>
      <button class="present-btn" id="presentBtn">Present <kbd>F</kbd></button>
    </div>
  </header>

  <div class="layout">
    <aside class="sidebar">
      <div class="sidebar-head">PAGES <span id="pageCount"></span></div>
      <div class="thumb-rail" id="thumbRail">
        <!-- thumbnails 由 JS 生成 -->
      </div>
    </aside>

    <main class="canvas" id="canvas">
      <div class="slide-shell" data-slide-index="0">
        <section
          class="page-shell"
          data-variant="lite"
          data-title="Cover"            <!-- O 键 overview / 演讲者 NEXT 卡片用 -->
        >
          <!-- chrome ring 8 锚点（HTML 元素类型 chrome 约束，文字 spec 填） -->
          <div class="chrome-top-left">…</div>      <!-- logo / brand mark -->
          <div class="chrome-top-right">…</div>     <!-- nav / 当前页号 -->
          <hr class="chrome-rule" />                <!-- 顶部 96px 处的 hairline -->
          <div class="chrome-bottom-left">…</div>   <!-- copyright / footer -->
          <div class="chrome-bottom-right">…</div>  <!-- page-num / brand letter -->

          <!-- 页面内容容器：block / component 由 spec + content 决定填什么 -->
          <div class="page-content" data-layout="strict">…</div>

          <!-- 演讲者 notes（CSS hidden；仅 S 键 presenter 窗口读取展示） -->
          <div class="notes">
            这一页要传达的核心是 X。**关键词加粗**作为提示信号，
            过渡到下一页用"接下来我们看 Y"。150-300 字 / 2-3 分钟节奏。
          </div>
        </section>
      </div>
      <!-- 更多 slide-shell -->
    </main>
  </div>
</div>

<!-- Overview overlay（O 键，由 JS 注入） -->
<!-- Presenter window（S 键，window.open 弹出独立窗口，不在主 DOM 里） -->
</body>
```

## Style

```css
:root { --fit-scale: 1; }

* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; overflow: hidden; }
body {
  font-family: var(--font-body);
  color: var(--chrome-fg);
  background: var(--canvas-bg);
  -webkit-font-smoothing: antialiased;
}

/* ============ APP SHELL ============ */
.app { height: 100vh; display: flex; flex-direction: column; }

.topbar {
  height: 48px; flex-shrink: 0;
  background: var(--chrome-bg);
  border-bottom: 1px solid var(--chrome-border);
  display: flex; align-items: center;
  padding: 0 12px; position: relative; z-index: 10;
}
.topbar-left, .topbar-right {
  display: flex; align-items: center; gap: 8px;
  flex: 1; min-width: 0;
}
.topbar-right { justify-content: flex-end; }
.topbar-center {
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none; max-width: 50%;
}
.topbar-center .title {
  font-size: 13px; font-weight: 500; color: var(--chrome-fg);
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  pointer-events: auto;
}
.topbar-center .title .sep { color: var(--chrome-muted-fg); margin: 0 6px; }

/* button reset — 防止 user agent 默认 text-align: center 通过 inheritance
   传给 thumb-inner 内 deep-clone 的 page-shell，导致缩略图文字居中而 main 左对齐 */
button { text-align: inherit; }

.icon-btn {
  width: 28px; height: 28px;
  display: inline-flex; align-items: center; justify-content: center;
  border: 0; background: transparent;
  border-radius: 6px; color: var(--chrome-fg);
  cursor: pointer; font: inherit;
  transition: background 0.12s;
}
.icon-btn:hover { background: rgba(0,0,0,0.05); }
.icon-btn.active { background: rgba(0,0,0,0.08); color: var(--chrome-accent); }
.icon-btn svg { width: 16px; height: 16px; }

.hairline-v {
  width: 1px; height: 20px; background: var(--chrome-border);
  margin: 0 4px;
}

.present-btn {
  height: 28px; padding: 0 12px;
  background: var(--chrome-accent); color: var(--chrome-accent-fg);
  border: 0; border-radius: 6px;
  font-size: 13px; font-weight: 500;
  font-family: inherit;
  display: inline-flex; align-items: center;
  cursor: pointer;
  transition: opacity 0.12s;
}
.present-btn:hover { opacity: 0.9; }
.present-btn kbd {
  font-family: ui-monospace, "SF Mono", Menlo, monospace;
  font-size: 11px;
  padding: 1px 5px; border-radius: 3px;
  /* 基于 chrome-accent-fg（按钮文字色），跨 dark/lite accent 都可见 */
  background: color-mix(in srgb, var(--chrome-accent-fg) 14%, transparent);
  color: color-mix(in srgb, var(--chrome-accent-fg) 80%, transparent);
  margin-left: 6px;
}

/* ============ LAYOUT ============ */
.layout { flex: 1; min-height: 0; display: flex; flex-direction: row; }

.sidebar {
  width: 264px; flex-shrink: 0;
  background: var(--chrome-bg);
  border-right: 1px solid var(--chrome-border);
  display: flex; flex-direction: column;
  overflow: hidden;
}
.sidebar-head {
  height: 36px; padding: 0 16px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: space-between;
  font-size: 11px; font-weight: 600; letter-spacing: 0.16em;
  text-transform: uppercase; color: var(--chrome-muted-fg);
  border-bottom: 1px solid var(--chrome-border);
}
.thumb-rail {
  flex: 1; overflow-y: auto;
  padding: 12px;
  display: flex; flex-direction: column; gap: 8px;
  scrollbar-width: thin;
  scrollbar-color: rgba(0, 0, 0, 0.18) transparent;
}
.thumb-rail::-webkit-scrollbar { width: 8px; height: 8px; }
.thumb-rail::-webkit-scrollbar-track { background: transparent; }
.thumb-rail::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.14);
  border-radius: 4px;
  border: 2px solid transparent;
  background-clip: padding-box;
  transition: background 0.15s;
}
.thumb-rail:hover::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.28);
  background-clip: padding-box;
}
.thumb-rail::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 0, 0, 0.4);
  background-clip: padding-box;
}

/* ============ THUMBNAIL ============ */
.thumb {
  display: flex; align-items: center; gap: 10px;
  padding: 6px;
  border: 0; background: transparent;
  border-radius: 8px;
  cursor: pointer;
  font-family: inherit;
  flex-shrink: 0;
  transition: background 0.12s;
}
.thumb:hover  { background: rgba(0,0,0,0.04); }
.thumb.active { background: color-mix(in srgb, var(--chrome-accent) 8%, transparent); }
.thumb.active .thumb-num   { color: var(--chrome-accent); }
.thumb.active .thumb-frame { box-shadow: 0 0 0 1.5px var(--chrome-accent); }
.thumb-num {
  font-size: 11px; font-weight: 600; color: var(--chrome-muted-fg);
  font-family: ui-monospace, "SF Mono", monospace;
  min-width: 22px; text-align: right;
  flex-shrink: 0;
}
.thumb-frame {
  width: 184px; height: 103.5px;
  background: var(--background);          /* slide 自己的底色 */
  border-radius: 4px;
  overflow: hidden; position: relative;
  box-shadow: 0 0 0 1px var(--chrome-border);
  transition: box-shadow 0.12s;
  flex-shrink: 0;
}
.thumb-inner {
  width: 1920px; height: 1080px;
  transform-origin: top left;
  transform: scale(0.0958);     /* 184/1920 */
  pointer-events: none;
  --fit-scale: 1;               /* 打破继承，thumb 内容不缩放 */
}
.thumb-inner * {
  animation: none !important;
  transition: none !important;
}

/* ============ CANVAS / SLIDE-SHELL ============ */
.canvas {
  flex: 1; min-width: 0; min-height: 0;
  position: relative; overflow: hidden;
  background: var(--canvas-bg);
}
.slide-shell {
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%);
  width:  calc(1920px * var(--fit-scale));
  height: calc(1080px * var(--fit-scale));
  background: var(--background);          /* slide 自己的底色 */
  border-radius: calc(16px * var(--fit-scale));
  box-shadow: 0 8px 24px rgba(0,0,0,0.08), 0 1px 3px rgba(0,0,0,0.06);
  overflow: hidden;
  display: none;
}
.slide-shell.active { display: block; }
.slide-shell > .page-shell {
  transform: scale(var(--fit-scale));
  transform-origin: top left;
}

/* ============ PRESENT MODE ============ */
body.present .topbar,
body.present .sidebar { display: none !important; }
body.present .canvas { background: #000; }
body.present .slide-shell {
  border-radius: 0;
  box-shadow: none;
}

/* sidebar toggle */
body.no-sidebar .sidebar { display: none; }

/* ============ PREVIEW MODE (?preview=N URL 参数) ============ */
/* 用于 presenter 窗口的 iframe：单页 + 无 chrome + 无内边距，像素级一致 */
body.is-preview .topbar,
body.is-preview .sidebar { display: none !important; }
body.is-preview .canvas { background: transparent; padding: 0; }
body.is-preview .slide-shell {
  border-radius: 0;
  box-shadow: none;
}

/* ============ NOTES（演讲者逐字稿，主视图永远隐藏） ============ */
/* 铁律：notes 仅供 S 键 presenter 窗口读取，绝不出现在 slide 视图里 */
.slide-shell .notes,
.thumb-frame .notes,
.overview-frame .notes { display: none !important; }

/* ============ OVERVIEW OVERLAY (O 键) ============ */
.overview-overlay {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.88);
  z-index: 100;
  display: none;
  padding: 48px;
  overflow-y: auto;
  grid-template-columns: repeat(auto-fill, 360px);   /* 固定宽，对齐 transform: scale 360/1920 */
  justify-content: center;
  gap: 24px 28px;
  align-content: start;
}
body.show-overview .overview-overlay { display: grid; }
body.show-overview { overflow: hidden; }

.overview-cell {
  display: flex; flex-direction: column; gap: 8px;
  background: transparent; border: 0; padding: 0;
  cursor: pointer; font: inherit; text-align: left;
  color: rgba(255, 255, 255, 0.85);
}
.overview-cell .overview-num {
  font-size: 11px; font-weight: 600; letter-spacing: 0.16em;
  font-family: ui-monospace, "SF Mono", Menlo, monospace;
  color: rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
}
.overview-cell .overview-title {
  font-size: 13px; font-weight: 500;
  color: rgba(255, 255, 255, 0.85);
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
}
.overview-frame {
  width: 360px; height: 202.5px;                      /* 16:9 @ scale 0.1875 */
  background: var(--background);
  border-radius: 6px; overflow: hidden;
  position: relative;
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1);
  transition: box-shadow 0.12s, transform 0.12s;
}
.overview-cell:hover .overview-frame {
  box-shadow: 0 0 0 2px var(--chrome-accent, #4f8af5);
  transform: translateY(-2px);
}
.overview-cell.active .overview-frame {
  box-shadow: 0 0 0 2px var(--chrome-accent, #4f8af5);
}
.overview-inner {
  width: 1920px; height: 1080px;
  transform: scale(0.1875);     /* 360/1920 */
  transform-origin: top left;
  pointer-events: none;
  --fit-scale: 1;
}
.overview-inner * {
  animation: none !important;
  transition: none !important;
}

/* ============ PRINT (PDF) ============ */
@media print {
  @page { size: 1920px 1080px; margin: 0; }
  /* print-color-adjust 必须挂在 selector 上（裸写会被 drop）——
     否则 dark / accent 主题会被印成白底，颜色全失 */
  * {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }
  html, body {
    height: auto; overflow: visible;
    background: var(--background);
  }
  .topbar, .sidebar, .thumb-rail { display: none !important; }
  .app, .layout, .canvas {
    display: block !important;
    position: static !important;
    height: auto !important; width: auto !important;
    overflow: visible !important;
    background: var(--background) !important;
  }
  /* 关键：强制所有 slide-shell 都显示（不只 .active 的那一个）。
     v2 默认 .slide-shell { display: none } + .active { display: block }，
     不 override 的话打印只输出当前页 */
  .slide-shell {
    position: relative !important;
    display: block !important;
    left: 0 !important; top: 0 !important;
    transform: none !important;
    width: 1920px !important; height: 1080px !important;
    border-radius: 0 !important; box-shadow: none !important;
    background: var(--background) !important;
    page-break-after: always; break-inside: avoid;
    margin: 0 !important;
  }
  /* page-shell 取消 fit-scale 缩放 */
  .slide-shell > .page-shell {
    transform: none !important;
    width: 1920px !important; height: 1080px !important;
  }
}
```

## Script

```javascript
(function () {
  // ===== 状态 =====
  const slides      = Array.from(document.querySelectorAll('.slide-shell'));
  const total       = slides.length;
  const canvas      = document.getElementById('canvas');
  const thumbRail   = document.getElementById('thumbRail');
  const pageCountEl = document.getElementById('pageCount');
  let current = 0;

  // ===== Preview 模式（?preview=N，演讲者 iframe 用） =====
  const params       = new URLSearchParams(location.search);
  const previewParam = params.get('preview');
  const isPreview    = previewParam !== null;
  if (isPreview) document.body.classList.add('is-preview');

  // ===== BroadcastChannel：主窗口 ↔ presenter 双向同步 =====
  let channel = null;
  try { channel = new BroadcastChannel('getslide-presenter'); } catch (e) {}

  // ===== 缩略图（preview 模式跳过） =====
  if (!isPreview) {
    if (pageCountEl) pageCountEl.textContent = String(total).padStart(2, '0');
    slides.forEach((slide, i) => {
      const btn = document.createElement('button');
      btn.className = 'thumb' + (i === 0 ? ' active' : '');
      btn.dataset.idx = i;
      btn.addEventListener('click', () => goto(i));
      const num = document.createElement('span');
      num.className = 'thumb-num';
      num.textContent = String(i + 1).padStart(2, '0');
      const frame = document.createElement('div');
      frame.className = 'thumb-frame';
      const inner = document.createElement('div');
      inner.className = 'thumb-inner';
      inner.appendChild(slide.querySelector('.page-shell').cloneNode(true));
      frame.appendChild(inner);
      btn.appendChild(num);
      btn.appendChild(frame);
      thumbRail.appendChild(btn);
    });
  }

  // ===== 翻页（统一入口） =====
  function goto(i, opts) {
    opts = opts || {};
    i = Math.max(0, Math.min(total - 1, i));
    slides.forEach((s, idx) => s.classList.toggle('active', idx === i));
    document.querySelectorAll('.thumb')
      .forEach((t, idx) => t.classList.toggle('active', idx === i));
    document.querySelectorAll('.overview-cell')
      .forEach((c, idx) => c.classList.toggle('active', idx === i));
    current = i;
    if (!isPreview && !opts.silent) {
      history.replaceState(null, '', '#/' + (i + 1));
      if (channel) channel.postMessage({ type: 'goto', idx: i });
    }
  }

  // ===== 启动定位：?preview 优先 → URL hash → 0 =====
  let bootIdx = 0;
  if (isPreview) {
    bootIdx = parseInt(previewParam, 10) || 0;
  } else {
    const m = location.hash.match(/^#\/(\d+)/);
    if (m) bootIdx = parseInt(m[1], 10) - 1;
  }
  goto(bootIdx, { silent: true });

  // hash 变化（外部跳转 / 浏览器前进后退）
  window.addEventListener('hashchange', () => {
    if (isPreview) return;
    const m = location.hash.match(/^#\/(\d+)/);
    if (m) goto(parseInt(m[1], 10) - 1, { silent: true });
  });

  // postMessage：presenter → 自家 iframe（preview-goto，不刷新切页）
  window.addEventListener('message', (e) => {
    if (e.data && e.data.type === 'preview-goto' && typeof e.data.idx === 'number') {
      goto(e.data.idx, { silent: true });
    }
  });

  // BroadcastChannel：presenter ↔ 主窗口
  channel?.addEventListener('message', (e) => {
    if (isPreview) return;
    if (e.data && e.data.type === 'goto' && typeof e.data.idx === 'number') {
      goto(e.data.idx, { silent: true });
    }
  });

  // ===== 键盘 =====
  window.addEventListener('keydown', (e) => {
    if (e.target.matches('input, textarea, [contenteditable]')) return;
    // 字母快捷键 F/T/O/S 必须无修饰键，避免劫持 Cmd+S / Cmd+O 触发系统警告音
    const plain = !e.metaKey && !e.ctrlKey && !e.altKey;
    if (['ArrowRight', 'ArrowDown', 'PageDown', ' '].includes(e.key)) {
      e.preventDefault(); goto(current + 1);
    } else if (['ArrowLeft', 'ArrowUp', 'PageUp'].includes(e.key)) {
      e.preventDefault(); goto(current - 1);
    } else if (e.key === 'Home') { e.preventDefault(); goto(0); }
    else if (e.key === 'End')    { e.preventDefault(); goto(total - 1); }
    else if (plain && !isPreview && (e.key === 'f' || e.key === 'F')) {
      e.preventDefault();
      document.documentElement.requestFullscreen?.().catch(() => {});
    } else if (plain && !isPreview && (e.key === 't' || e.key === 'T')) {
      cycleTheme();
    } else if (plain && !isPreview && (e.key === 'o' || e.key === 'O')) {
      e.preventDefault(); toggleOverview();
    } else if (plain && !isPreview && (e.key === 's' || e.key === 'S')) {
      e.preventDefault(); openPresenter();
    } else if (e.key === 'Escape') {
      document.body.classList.remove('show-overview');
    } else if (plain && /^[0-9]$/.test(e.key)) {
      // 数字键跳页：1-9 直跳第 N 页；0 跳到最后一页
      e.preventDefault();
      const n = parseInt(e.key, 10);
      goto(n === 0 ? total - 1 : n - 1);
    }
  });

  // ===== 演示模式（fullscreen） =====
  if (!isPreview) {
    document.addEventListener('fullscreenchange', () => {
      document.body.classList.toggle('present', !!document.fullscreenElement);
    });
    document.getElementById('presentBtn')?.addEventListener('click', () => {
      document.documentElement.requestFullscreen?.().catch(() => {});
    });
    document.getElementById('downloadBtn')?.addEventListener('click', () => window.print());
    window.addEventListener('click', (e) => {
      if (!document.body.classList.contains('present')) return;
      if (e.target.closest('button, a, input, textarea, [contenteditable]')) return;
      if (e.button !== 0) return;
      goto(current + 1);
    });
    window.addEventListener('contextmenu', (e) => {
      if (!document.body.classList.contains('present')) return;
      e.preventDefault();
      goto(current - 1);
    });
    document.getElementById('toggleSidebar')?.addEventListener('click', () => {
      document.body.classList.toggle('no-sidebar');
    });
  }

  // ===== 滚轮翻页（preview 模式不接） =====
  if (!isPreview) {
    let wheelLock = false;
    canvas.addEventListener('wheel', (e) => {
      if (Math.abs(e.deltaY) < 30) return;
      e.preventDefault();
      if (wheelLock) return;
      wheelLock = true;
      setTimeout(() => { wheelLock = false; }, 400);
      goto(current + (e.deltaY > 0 ? 1 : -1));
    }, { passive: false });
  }

  // ===== scale-to-fit =====
  const PADDING = isPreview ? 0 : 32;
  const ro = new ResizeObserver(([entry]) => {
    const { width, height } = entry.contentRect;
    const scale = Math.min(
      Math.max(1, width  - PADDING * 2) / 1920,
      Math.max(1, height - PADDING * 2) / 1080
    );
    document.documentElement.style.setProperty('--fit-scale', Math.max(0.05, scale));
  });
  ro.observe(canvas);

  // ===== T 键：主题循环（外置 CSS 文件场景；自包含 deck 不接） =====
  const themesAttr = document.body.dataset.themes;
  const themeBase  = document.body.dataset.themeBase || '';
  const themes     = themesAttr
    ? themesAttr.split(',').map(s => s.trim()).filter(Boolean) : [];
  const themeLink  = document.getElementById('theme-link');
  let themeIdx = 0;
  function cycleTheme() {
    if (!themes.length || !themeLink) return;
    themeIdx = (themeIdx + 1) % themes.length;
    const t = themes[themeIdx];
    themeLink.href = themeBase + t + (t.endsWith('.css') ? '' : '.css');
  }

  // ===== O 键：Overview 网格 =====
  let overviewBuilt = false;
  function buildOverview() {
    if (overviewBuilt) return;
    overviewBuilt = true;
    const overlay = document.createElement('div');
    overlay.className = 'overview-overlay';
    slides.forEach((slide, i) => {
      const cell = document.createElement('button');
      cell.className = 'overview-cell' + (i === current ? ' active' : '');
      cell.dataset.idx = i;
      cell.addEventListener('click', () => {
        goto(i);
        document.body.classList.remove('show-overview');
      });
      const num = document.createElement('span');
      num.className = 'overview-num';
      num.textContent = String(i + 1).padStart(2, '0');
      const titleEl = document.createElement('span');
      titleEl.className = 'overview-title';
      const ps = slide.querySelector('.page-shell');
      titleEl.textContent = (ps && ps.dataset && ps.dataset.title) || '';
      const frame = document.createElement('div');
      frame.className = 'overview-frame';
      const inner = document.createElement('div');
      inner.className = 'overview-inner';
      inner.appendChild(slide.querySelector('.page-shell').cloneNode(true));
      frame.appendChild(inner);
      cell.appendChild(num);
      if (titleEl.textContent) cell.appendChild(titleEl);
      cell.appendChild(frame);
      overlay.appendChild(cell);
    });
    overlay.addEventListener('click', (e) => {
      if (e.target === overlay) document.body.classList.remove('show-overview');
    });
    document.body.appendChild(overlay);
  }
  function toggleOverview() {
    buildOverview();
    document.body.classList.toggle('show-overview');
  }

  // ===== S 键：Presenter Mode（独立窗口 + 4 张磁吸卡） =====
  let presenterWin = null;
  function openPresenter() {
    if (presenterWin && !presenterWin.closed) { presenterWin.focus(); return; }
    const w = window.open('about:blank', 'getslide-presenter',
      'width=1280,height=820,menubar=no,toolbar=no,location=no');
    if (!w) return;
    presenterWin = w;
    const notes = slides.map(s => {
      const el = s.querySelector('.notes');
      return el ? (el.innerHTML || '').trim() : '';
    });
    const titles = slides.map((s, i) => {
      const ps = s.querySelector('.page-shell');
      return (ps && ps.dataset && ps.dataset.title) || ('Slide ' + (i + 1));
    });
    w.document.open();
    w.document.write(renderPresenterDoc({
      deckUrl: location.pathname, total, current, notes, titles
    }));
    w.document.close();
  }

  // 演讲者窗口的 HTML（独立窗口、独立 script，模板字符串拼装一次性写入）
  function renderPresenterDoc(ctx) {
    const { deckUrl, total, current, notes, titles } = ctx;
    return [
      '<!doctype html><html lang="zh-CN"><head>',
      '<meta charset="utf-8"><title>Presenter · getslide</title>',
      '<style>',
      '* { box-sizing: border-box; margin: 0; padding: 0; }',
      'body { height: 100vh; overflow: hidden; background: #0d0e10; color: #e8e9ec;',
      '  font: 13px/1.4 -apple-system, "SF Pro Text", system-ui, sans-serif; position: relative; }',
      '.card { position: absolute; background: #16181d; border: 1px solid #2a2d35; border-radius: 10px;',
      '  display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 12px 40px rgba(0,0,0,0.4); }',
      '.card-head { height: 32px; flex-shrink: 0; background: #1c1f26; border-bottom: 1px solid #2a2d35;',
      '  display: flex; align-items: center; padding: 0 12px; font-size: 10px; font-weight: 600;',
      '  letter-spacing: 0.16em; text-transform: uppercase; color: #8a8f99; cursor: grab; user-select: none; }',
      '.card-head.dragging { cursor: grabbing; }',
      '.card-head .dot { width: 8px; height: 8px; border-radius: 50%; margin-right: 8px; }',
      '.card-current .dot { background: #4f8af5; }',
      '.card-next    .dot { background: #a479e0; }',
      '.card-script  .dot { background: #f0a040; }',
      '.card-timer   .dot { background: #62d29a; }',
      '.card-body { flex: 1; min-height: 0; position: relative; overflow: hidden; }',
      '.card iframe { width: 1920px; height: 1080px; transform-origin: top left; border: 0;',
      '  background: #fff; position: absolute; left: 0; top: 0; }',
      '.script-body { padding: 20px 24px; overflow-y: auto; font-size: 22px; line-height: 1.55;',
      '  color: #f0f1f4; font-family: "SF Pro Text", -apple-system, system-ui, sans-serif; height: 100%; }',
      '.script-body strong { color: #ffd866; font-weight: 600; }',
      '.script-empty { color: #62666d; font-style: italic; font-size: 14px; }',
      '.timer-body { padding: 16px 20px; display: flex; flex-direction: column; gap: 12px; height: 100%; }',
      '.timer-row { display: flex; align-items: center; justify-content: space-between; gap: 10px; }',
      '.timer-elapsed { font-size: 36px; font-weight: 600; font-family: ui-monospace, "SF Mono", Menlo, monospace; }',
      '.timer-page { font-size: 13px; color: #8a8f99; font-family: ui-monospace, "SF Mono", Menlo, monospace; }',
      '.timer-btns { display: flex; gap: 6px; flex-wrap: wrap; }',
      '.timer-btns button { height: 28px; padding: 0 10px; background: #1c1f26; border: 1px solid #2a2d35;',
      '  border-radius: 6px; color: #d0d6e0; cursor: pointer; font: inherit; font-size: 12px; }',
      '.timer-btns button:hover { background: #23262e; }',
      '.resize-handle { position: absolute; right: 0; bottom: 0; width: 14px; height: 14px;',
      '  cursor: nwse-resize;',
      '  background: linear-gradient(135deg, transparent 50%, #3a3d45 50%, #3a3d45 60%, transparent 60%,',
      '    transparent 70%, #3a3d45 70%, #3a3d45 80%, transparent 80%); }',
      '.toolbar { position: absolute; top: 12px; right: 12px; display: flex; gap: 8px; z-index: 50; }',
      '.toolbar button { height: 28px; padding: 0 12px; background: #1c1f26; border: 1px solid #2a2d35;',
      '  border-radius: 6px; color: #d0d6e0; cursor: pointer; font: inherit; font-size: 12px; }',
      '.toolbar button:hover { background: #23262e; }',
      '</style></head><body>',
      '<div class="toolbar"><button id="resetLayout">Reset layout</button></div>',
      '<div class="card card-current" data-card="current" data-default="left:24;top:24;width:768;height:432">',
      '  <div class="card-head"><span class="dot"></span>CURRENT · <span data-bind="title-cur"></span></div>',
      '  <div class="card-body"><iframe data-iframe="cur"></iframe></div>',
      '  <div class="resize-handle"></div></div>',
      '<div class="card card-next" data-card="next" data-default="left:816;top:24;width:432;height:243">',
      '  <div class="card-head"><span class="dot"></span>NEXT · <span data-bind="title-next"></span></div>',
      '  <div class="card-body"><iframe data-iframe="next"></iframe></div>',
      '  <div class="resize-handle"></div></div>',
      '<div class="card card-script" data-card="script" data-default="left:24;top:476;width:768;height:316">',
      '  <div class="card-head"><span class="dot"></span>SPEAKER SCRIPT</div>',
      '  <div class="card-body"><div class="script-body" data-bind="script">—</div></div>',
      '  <div class="resize-handle"></div></div>',
      '<div class="card card-timer" data-card="timer" data-default="left:816;top:291;width:432;height:200">',
      '  <div class="card-head"><span class="dot"></span>TIMER</div>',
      '  <div class="card-body"><div class="timer-body">',
      '    <div class="timer-row"><div class="timer-elapsed" id="elapsed">00:00</div>',
      '      <div class="timer-page" id="pageInfo"></div></div>',
      '    <div class="timer-btns"><button id="prevBtn">← Prev</button>',
      '      <button id="nextBtn">Next →</button>',
      '      <button id="resetBtn">Reset (R)</button></div>',
      '  </div></div><div class="resize-handle"></div></div>',
      '<script>',
      '(function(){',
      '  var TOTAL=' + total + ',',
      '      DECK_URL=' + JSON.stringify(deckUrl) + ',',
      '      NOTES=' + JSON.stringify(notes) + ',',
      '      TITLES=' + JSON.stringify(titles) + ';',
      '  var STORAGE_KEY="getslide-presenter-layout";',
      '  var cur=' + current + ', startedAt=Date.now();',
      '  var cards=document.querySelectorAll(".card");',
      '  var layout=JSON.parse(localStorage.getItem(STORAGE_KEY)||"{}");',
      '  cards.forEach(function(c){ var def=parseLayout(c.dataset.default);',
      '    var saved=layout[c.dataset.card]||def; applyLayout(c,saved);',
      '    makeDraggable(c); makeResizable(c); });',
      '  function parseLayout(s){ var o={}; s.split(";").forEach(function(p){',
      '    var kv=p.split(":"); o[kv[0]]=parseInt(kv[1],10); }); return o; }',
      '  function applyLayout(c,l){ c.style.left=l.left+"px"; c.style.top=l.top+"px";',
      '    c.style.width=l.width+"px"; c.style.height=l.height+"px"; fitIframe(c); }',
      '  function persist(){ var out={}; cards.forEach(function(c){ out[c.dataset.card]={',
      '    left:parseInt(c.style.left,10), top:parseInt(c.style.top,10),',
      '    width:parseInt(c.style.width,10), height:parseInt(c.style.height,10) }; });',
      '    localStorage.setItem(STORAGE_KEY,JSON.stringify(out)); }',
      '  function makeDraggable(c){ var head=c.querySelector(".card-head");',
      '    var sx,sy,sl,st,dragging=false;',
      '    head.addEventListener("mousedown",function(e){ if(e.target.closest("button"))return;',
      '      dragging=true; head.classList.add("dragging");',
      '      sx=e.clientX; sy=e.clientY;',
      '      sl=parseInt(c.style.left,10); st=parseInt(c.style.top,10); e.preventDefault(); });',
      '    window.addEventListener("mousemove",function(e){ if(!dragging)return;',
      '      c.style.left=(sl+e.clientX-sx)+"px"; c.style.top=(st+e.clientY-sy)+"px"; });',
      '    window.addEventListener("mouseup",function(){ if(!dragging)return;',
      '      dragging=false; head.classList.remove("dragging"); persist(); }); }',
      '  function makeResizable(c){ var h=c.querySelector(".resize-handle");',
      '    var sx,sy,sw,sh,resizing=false;',
      '    h.addEventListener("mousedown",function(e){ resizing=true;',
      '      sx=e.clientX; sy=e.clientY;',
      '      sw=parseInt(c.style.width,10); sh=parseInt(c.style.height,10);',
      '      e.preventDefault(); e.stopPropagation(); });',
      '    window.addEventListener("mousemove",function(e){ if(!resizing)return;',
      '      c.style.width=Math.max(220,sw+e.clientX-sx)+"px";',
      '      c.style.height=Math.max(120,sh+e.clientY-sy)+"px"; fitIframe(c); });',
      '    window.addEventListener("mouseup",function(){ if(!resizing)return;',
      '      resizing=false; persist(); }); }',
      '  function fitIframe(c){ var iframe=c.querySelector("iframe"); if(!iframe)return;',
      '    var body=c.querySelector(".card-body");',
      '    var w=body.clientWidth, h=body.clientHeight;',
      '    var scale=Math.min(w/1920,h/1080);',
      '    iframe.style.transform="scale("+scale+")";',
      '    iframe.style.left=((w-1920*scale)/2)+"px";',
      '    iframe.style.top=((h-1080*scale)/2)+"px"; }',
      '  document.getElementById("resetLayout").addEventListener("click",function(){',
      '    cards.forEach(function(c){ applyLayout(c,parseLayout(c.dataset.default)); });',
      '    persist(); });',
      '  var iframeCur=document.querySelector("[data-iframe=cur]");',
      '  var iframeNext=document.querySelector("[data-iframe=next]");',
      '  iframeCur.src=DECK_URL+"?preview="+cur;',
      '  iframeNext.src=DECK_URL+"?preview="+Math.min(cur+1,TOTAL-1);',
      '  cards.forEach(fitIframe);',
      '  window.addEventListener("resize",function(){ cards.forEach(fitIframe); });',
      '  var channel=null; try{ channel=new BroadcastChannel("getslide-presenter"); }catch(e){}',
      '  channel&&channel.addEventListener("message",function(e){',
      '    if(e.data&&e.data.type==="goto"&&typeof e.data.idx==="number"){ updateState(e.data.idx,false); }',
      '  });',
      '  function updateState(idx,echo){ cur=Math.max(0,Math.min(TOTAL-1,idx));',
      '    document.querySelector("[data-bind=title-cur]").textContent=TITLES[cur]||"";',
      '    document.querySelector("[data-bind=title-next]").textContent=TITLES[Math.min(cur+1,TOTAL-1)]||"";',
      '    var sc=document.querySelector("[data-bind=script]"); var t=NOTES[cur];',
      '    sc.innerHTML=(t&&t.length)?t:"<span class=script-empty>（这一页没有 notes）</span>";',
      '    document.getElementById("pageInfo").textContent=',
      '      String(cur+1).padStart(2,"0")+" / "+String(TOTAL).padStart(2,"0");',
      '    iframeCur.contentWindow&&iframeCur.contentWindow.postMessage({type:"preview-goto",idx:cur},"*");',
      '    iframeNext.contentWindow&&iframeNext.contentWindow.postMessage(',
      '      {type:"preview-goto",idx:Math.min(cur+1,TOTAL-1)},"*");',
      '    if(echo&&channel) channel.postMessage({type:"goto",idx:cur}); }',
      '  updateState(cur,false);',
      '  window.addEventListener("keydown",function(e){',
      '    if(["ArrowRight","ArrowDown","PageDown"," "].indexOf(e.key)>=0){',
      '      e.preventDefault(); updateState(cur+1,true); }',
      '    else if(["ArrowLeft","ArrowUp","PageUp"].indexOf(e.key)>=0){',
      '      e.preventDefault(); updateState(cur-1,true); }',
      '    else if(e.key==="Home"){ updateState(0,true); }',
      '    else if(e.key==="End"){ updateState(TOTAL-1,true); }',
      '    else if(e.key==="r"||e.key==="R"){ startedAt=Date.now(); tick(); }',
      '    else if(e.key==="Escape"){ window.close(); }',
      '  });',
      '  document.getElementById("prevBtn").addEventListener("click",function(){updateState(cur-1,true);});',
      '  document.getElementById("nextBtn").addEventListener("click",function(){updateState(cur+1,true);});',
      '  document.getElementById("resetBtn").addEventListener("click",function(){startedAt=Date.now();tick();});',
      '  function pad(n){return String(n).padStart(2,"0");}',
      '  function tick(){ var sec=Math.floor((Date.now()-startedAt)/1000);',
      '    var m=Math.floor(sec/60), s=sec%60, h=Math.floor(m/60);',
      '    document.getElementById("elapsed").textContent=h>0?',
      '      h+":"+pad(m%60)+":"+pad(s):pad(m)+":"+pad(s); }',
      '  setInterval(tick,500); tick();',
      '})();',
      '</' + 'script>',
      '</body></html>'
    ].join('\n');
  }
})();
```

## Constraints

### 结构

- 每个 slide 必须是 `.slide-shell > .page-shell` 结构（`.page-shell` 由当前 spec 提供）
- **page-shell 的 8 锚点元素类型 + class 名 由 chrome 约束**（见 Template 段）：`.chrome-top-left/-right/-bottom-left/-bottom-right` 是 `<div>`，`.chrome-rule` 是 `<hr>`。spec 仅填锚点内的文字 / SVG，不改元素类型与 class 名
- **每个 page-shell 必须有 `data-title="..."`**——O 键 overview 网格 + S 键 presenter NEXT 卡片都用这个字段；缺失则 fallback 到 "Slide N"

### 翻页 + 同步

- **page 切换由 JS toggle `.slide-shell.active` 实现**——所有 slide-shell 默认 `display: none`，仅 `.active` 的 `display: block`。新增页面只要标 class，不需手动管显隐
- 翻页支持 5 种入口：键盘（Arrow / PageUp/Down / Space / Home / End / **数字键 1-9 直跳；0 跳末页**）、**滚轮（canvas 区域内，throttled 400ms 防触摸板连发）**、缩略图点击、present 模式下鼠标左/右键、URL hash `#/N`。sidebar 内滚轮仍滚 thumb 列表，不触发翻页
- 字母快捷键（F / T / O / S）必须裸按、不带任何修饰键（`!metaKey && !ctrlKey && !altKey`），避免劫持 `Cmd+S` / `Cmd+O` 触发系统 NSBeep 警告音
- **URL hash `#/N`**：每次翻页 `history.replaceState` 同步；外部改 hash 也会触发翻页（`hashchange` 监听）。N 是 1-indexed
- **`?preview=N` 单页模式**：URL 参数触发 `body.is-preview` → 隐藏 topbar/sidebar/边框，scale-to-fit 用 0 padding。专为 presenter iframe 设计，**不要**手工触发；preview 模式下 T/O/S/F 全部禁用
- present 模式（fullscreen）靠 `body.present` class；浏览器拒绝时无副作用
- 缩略图是 `.page-shell` 的 DOM 克隆 + 缩放；克隆内 `--fit-scale: 1` 打破继承避免双重缩放

### Notes（演讲者逐字稿）—— 铁律

- 每个 page-shell 应内嵌一个 `<div class="notes">…</div>`；CSS 默认 `display: none`，**永远不会**出现在主视图、缩略图、overview 网格、preview iframe 里
- **绝不允许把演讲者面向的描述文字（"这一页讲…"、"speaker: 提到 X 时…"、灰色解释 caption）放在 notes 之外**。叙述性文字一律进 notes；slide 正文只放观众面向的标题/数据/图
- notes 写作三规：① 不是讲稿，是**提示信号**（核心词加粗 + 过渡句独立段）② 每页 150–300 字（2–3 分钟节奏）③ 用口语，不写书面语
- presenter 窗口（S 键）从每页的 `.notes innerHTML` 读取并大字号渲染；空的会显示"（这一页没有 notes）"

### Presenter Mode（S 键）

- 弹出**新窗口** 1280×820，4 张磁吸卡（CURRENT iframe / NEXT iframe / SCRIPT / TIMER）
- 卡片 head 可拖、右下角 14×14 三角可拉伸；位置 + 尺寸持久化到 `localStorage["getslide-presenter-layout"]`，"Reset layout" 按钮恢复默认
- iframe 用 `?preview=N` 加载同一份 deck HTML，所以**像素级与主视图一致**（同 CSS / fonts / fit-scale）
- 主窗 ↔ presenter 窗口靠 `BroadcastChannel("getslide-presenter")` 双向同步；presenter → 自家 iframe 用 `postMessage({type:'preview-goto', idx:N})` **不刷新只切 `.is-active`**
- presenter 窗口键盘：← → / Space / PageUp/Down / Home/End 翻页（同步主窗）· R 复位计时 · Esc 关闭

### T 键主题循环

- 仅当 `<body data-themes="a,b,c">` + `<link id="theme-link">` 都存在时才工作；否则 no-op
- `data-theme-base` 是 CSS 文件相对路径前缀（如 `"../themes/"`）
- 每按一次 T，把 `theme-link.href` 切到下一个；自包含 deck（CSS 全 inline）不接此机制

### Overview Grid（O 键）

- 第一次按 O 时构建 overlay；之后只 toggle `body.show-overview`
- 卡片宽 360px 固定（`grid-template-columns: repeat(auto-fill, 360px)`），`.overview-inner` `transform: scale(0.1875)`
- 点击 cell → `goto(idx)` + 关闭 overlay；点 overlay 空白处 / Esc 也关闭

## Why

- **chrome CSS 直接抄 v1**——v1 已经验证过的视觉，重写没意义
- **chrome 颜色 namespace 化**（`--chrome-*`）——chrome 是产品 UI，跟 slide 设计内容解耦。spec 通常给 `--chrome-fg: var(--foreground)` 这样的默认指向，但主题想 chrome 跟 slide 走不同视觉时（编辑器风浅色 chrome + dark slide），单独改 chrome token 即可
- **几何 hardcoded 不破规则**——chrome 是 deck 运行时层，几何参数（48px topbar / 264px sidebar / 184px thumb-frame）跨主题一致，硬编码反而清楚
- **`thumb-frame` / `slide-shell` 用 `var(--background)`**——它们显示的是 *slide 自己*的底色（缩略图就是 slide 缩小版），不是 chrome 视觉，所以仍然引 spec 的 background
- **preview 模式用 URL 参数而非 class 输入**——iframe 里 JS 启动一次性读 `?preview=N` 进入只读单页态。CSS 通过 `body.is-preview` 触发，跟运行时切换的 `.present` 解耦
- **presenter 用独立窗口而非 modal**——投屏时观众屏（主窗 fullscreen）和演讲者屏（presenter 窗口）通常在不同显示器；独立窗口可拖到第二屏。`localStorage` 存布局让用户的两屏布局跨次复用
- **iframe 像素级一致**靠 `?preview=N` + 同一份 HTML/CSS/fonts，无需双份模板。切页用 `postMessage` 不 reload —— 跨页切换无白闪 / 字体加载抖动
- **BroadcastChannel 而非 opener.postMessage**——双向 + 同源 + 不依赖 window reference 仍存活；presenter 窗口被关也不会 leak 到主窗
