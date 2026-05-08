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
        <section class="page-shell" data-variant="lite">
          <!-- chrome ring 8 锚点（HTML 元素类型 chrome 约束，文字 spec 填） -->
          <div class="chrome-top-left">…</div>      <!-- logo / brand mark -->
          <div class="chrome-top-right">…</div>     <!-- nav / 当前页号 -->
          <hr class="chrome-rule" />                <!-- 顶部 96px 处的 hairline -->
          <div class="chrome-bottom-left">…</div>   <!-- copyright / footer -->
          <div class="chrome-bottom-right">…</div>  <!-- page-num / brand letter -->

          <!-- 页面内容容器：block / component 由 spec + content 决定填什么 -->
          <div class="page-content" data-layout="strict">…</div>
        </section>
      </div>
      <!-- 更多 slide-shell -->
    </main>
  </div>
</div>
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
(function() {
  const slides = Array.from(document.querySelectorAll('.slide-shell'));
  const total  = slides.length;
  const canvas = document.getElementById('canvas');
  const thumbRail = document.getElementById('thumbRail');
  const pageCountEl = document.getElementById('pageCount');
  let current = 0;

  // 缩略图生成
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
    const clone = slide.querySelector('.page-shell').cloneNode(true);
    inner.appendChild(clone);
    frame.appendChild(inner);

    btn.appendChild(num);
    btn.appendChild(frame);
    thumbRail.appendChild(btn);
  });

  // 翻页
  function goto(i) {
    i = Math.max(0, Math.min(total - 1, i));
    slides.forEach((s, idx) => s.classList.toggle('active', idx === i));
    document.querySelectorAll('.thumb').forEach((t, idx) => t.classList.toggle('active', idx === i));
    current = i;
  }
  goto(0);

  // 键盘
  window.addEventListener('keydown', (e) => {
    if (e.target.matches('input, textarea, [contenteditable]')) return;
    if (['ArrowRight', 'ArrowDown', 'PageDown', ' '].includes(e.key)) {
      e.preventDefault(); goto(current + 1);
    } else if (['ArrowLeft', 'ArrowUp', 'PageUp'].includes(e.key)) {
      e.preventDefault(); goto(current - 1);
    } else if (e.key === 'Home') { e.preventDefault(); goto(0); }
    else if (e.key === 'End')    { e.preventDefault(); goto(total - 1); }
    else if (e.key === 'f' || e.key === 'F') {
      e.preventDefault();
      document.documentElement.requestFullscreen?.().catch(() => {});
    }
  });

  // 演示模式
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

  // sidebar toggle
  document.getElementById('toggleSidebar')?.addEventListener('click', () => {
    document.body.classList.toggle('no-sidebar');
  });

  // 滚轮翻页（canvas 区域内；throttled 防触摸板连续触发；sidebar 内滚轮仍是 thumb 列表滚动）
  let wheelLock = false;
  canvas.addEventListener('wheel', (e) => {
    if (Math.abs(e.deltaY) < 30) return;     // 过滤微小滚动 / 横向 trackpad 抖动
    e.preventDefault();
    if (wheelLock) return;
    wheelLock = true;
    setTimeout(() => { wheelLock = false; }, 400);
    goto(current + (e.deltaY > 0 ? 1 : -1));
  }, { passive: false });

  // scale-to-fit
  const PADDING = 32;
  const ro = new ResizeObserver(([entry]) => {
    const { width, height } = entry.contentRect;
    const scale = Math.min(
      (width - PADDING * 2) / 1920,
      (height - PADDING * 2) / 1080
    );
    document.documentElement.style.setProperty('--fit-scale', Math.max(0.05, scale));
  });
  ro.observe(canvas);
})();
```

## Constraints

- 每个 slide 必须是 `.slide-shell > .page-shell` 结构（`.page-shell` 由当前 spec 提供）
- **page-shell 的 8 锚点元素类型 + class 名 由 chrome 约束**（见 Template 段）：`.chrome-top-left/-right/-bottom-left/-bottom-right` 是 `<div>`，`.chrome-rule` 是 `<hr>`。spec 仅填锚点内的文字 / SVG，不改元素类型与 class 名
- **page 切换由 JS toggle `.slide-shell.active` 实现**——所有 slide-shell 默认 `display: none`，仅 `.active` 的 `display: block`（CSS 第 266-268 行 + script goto() 已实现）。新增页面只要标 class，不需手动管显隐
- 翻页支持 4 种入口：键盘（Arrow / PageUp/Down / Space）、**滚轮（canvas 区域内，throttled 400ms 防触摸板连发）**、缩略图点击、present 模式下鼠标左/右键。sidebar 内滚轮仍滚 thumb 列表，不触发翻页
- present 模式靠 fullscreen API + `body.present` class；浏览器拒绝时无副作用
- 缩略图是 `.page-shell` 的 DOM 克隆 + 缩放；克隆内 `--fit-scale: 1` 打破继承避免双重缩放

## Why

- **chrome CSS 直接抄 v1**——v1 已经验证过的视觉，重写没意义
- **chrome 颜色 namespace 化**（`--chrome-*`）——chrome 是产品 UI，跟 slide 设计内容解耦。spec 通常给 `--chrome-fg: var(--foreground)` 这样的默认指向，但主题想 chrome 跟 slide 走不同视觉时（编辑器风浅色 chrome + dark slide），单独改 chrome token 即可
- **几何 hardcoded 不破规则**——chrome 是 deck 运行时层，几何参数（48px topbar / 264px sidebar / 184px thumb-frame）跨主题一致，硬编码反而清楚
- **`thumb-frame` / `slide-shell` 用 `var(--background)`**——它们显示的是 *slide 自己*的底色（缩略图就是 slide 缩小版），不是 chrome 视觉，所以仍然引 spec 的 background
