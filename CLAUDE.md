# CLAUDE.md — prompt-night

Project-scoped rules for Claude Code sessions. Read before touching files.

## Project
Reveal.js deck for Prompt Night n24 talk. Two decks:
- `deck/index-v2.html` — public deck (no speaker notes)
- `deck/speaker.html` — speaker rig (notes enabled, dynamic font-fit)

Shared theme: `deck/theme-v2.css`. Vendored Reveal at `deck/vendor/reveal.js-5.1.0/`. Static-served via `python3 -m http.server 8000` from `deck/`.

## ⚠ Layout invariants — DO NOT REVERT

The deck uses **viewport-sized Reveal + dynamic per-slide font-fit**. Reverting any of the following regresses the full-bleed layout that took multiple sessions to land:

### `deck/speaker.html` (and `index-v2.html`)
Reveal config MUST stay:
```js
width: window.innerWidth,
height: window.innerHeight,
margin: 0,
minScale: 1,
maxScale: 1,
```
Do NOT change to fixed `1920×1080` / `2400×1350` with margin. That re-introduces letterbox bands.

`speaker.html` includes a `fitSection(s)` binary-search font scaler bound to `Reveal.on('slidechanged')` + `document.fonts.ready`. It temporarily sets `justify-content: flex-start` during measurement to detect top-edge clipping that flex-center hides from `scrollHeight`. Do NOT remove the flex-start swap.

### `deck/theme-v2.css`
The `.reveal .slides > section` rule MUST use:
- `height: 100%` and `min-height: 100%` (NOT `100vh` — must match Reveal's slide container, not the window)
- `padding: 2vh 4vw` (vh-based vertical padding survives short viewports)

The `.reveal` base font MUST use a min(vw, vh) clamp so content scales on both axes:
```css
font-size: clamp(14px, min(1.5vw, 2.65vh), 38px);
```

Both files carry inline `⚠ DO NOT REVERT` comments above the load-bearing blocks. Leave them.

## Verification

Before reporting "fixed":
1. Run `python3 -m http.server 8000` from `deck/` if not already running.
2. Open `localhost:8000/speaker.html` in Chrome (or use `/chrome` via MCP tools).
3. Check at least slides 0, 3, 5, 6, 9, 25 — these stressed the fit algo previously.
4. No content clipped at top/bottom edges. Eyebrows visible. No letterbox bars on ultrawide.

Programmatic check (paste in console):
```js
(async () => { const out=[]; const t=Reveal.getTotalSlides(); for(let i=0;i<t;i++){ Reveal.slide(i); await new Promise(r=>setTimeout(r,120)); const s=document.querySelector('.reveal .slides > section.present'); if(!s)continue; fitSection(s); const off=Array.from(s.children).filter(c=>c.tagName!=='ASIDE'&&!c.classList.contains('grain')); var minT=Infinity,maxB=-Infinity; off.forEach(c=>{const r=c.getBoundingClientRect();if(r.top<minT)minT=r.top;if(r.bottom>maxB)maxB=r.bottom;}); out.push({i,minT:Math.round(minT),maxB:Math.round(maxB),vh:innerHeight}); } return JSON.stringify(out.filter(o=>o.minT<0||o.maxB>o.vh)); })()
```
Empty array = pass.

## Editing conventions

- Cache-bust URL with `?v=N` (incrementing) when validating CSS/JS changes in browser — static server doesn't send no-cache headers.
- Prefer editing existing slide markup over restructuring sections — fit algo handles content size, but radical DOM changes may need re-validation across all slides.
- Both decks share `theme-v2.css`. CSS changes affect both. If a change is speaker-only, scope with `body.speaker` or similar — don't fork the CSS file.
- Speaker notes live inside `<aside class="notes" data-markdown>` — `aside.notes` is hidden from slide render, only shown in speaker view popup (`?present`).

## Tooling

- Use the `/chrome` MCP tools (`mcp__claude-in-chrome__*`) to validate slides visually + measure overflow.
- Use `Reveal.slide(n)` to navigate; `fitSection(s)` to manually re-fit.
- Reveal speaker popup is independent — opens via `S` key in main deck.

## Language / commits

- English in code, comments, commits.
- Communicate in English unless user writes Portuguese (then Brazilian Portuguese).

## What NOT to do

- Do NOT add a build step (no webpack/vite/etc). Pure static files only.
- Do NOT delete `aside.notes` blocks — they're the speaker script.
- Do NOT rewrite the fit algorithm without preserving the flex-start measurement swap and the `min(vw, vh)` font-size constraint.
- Do NOT introduce CSS frameworks. Theme uses CSS variables + hand-rolled rules.
