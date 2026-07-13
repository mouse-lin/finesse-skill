# Chart Crafting — Hand-Built SVG + GSAP (the implementation layer)

The **implementation** counterpart to `dataviz.md`. Where `dataviz.md` decides *which* chart, its a11y grade, and *which library* — this file is **how to draw it by hand** when you're shipping a single self-contained file (`design-dna.md` §8 boilerplate) and don't want a chart-library dependency: raw SVG for the mark, GSAP for the motion, and a reduced-motion terminal state paired with every animation.

> **When to load:** a dashboard / product page that must ship as one HTML file with no `Chart.js`/`ECharts` CDN, or where you want full control over the mark and the draw-in. If you're already inside a React app or need 25 chart types, stay in `dataviz.md` §3 (library picks). This file is the *no-library* path — and it still inherits `dataviz.md`'s selection matrix (§1) and a11y fallbacks (§2). Hand-drawing a chart does not exempt it from the value-label / table-fallback rules.

> **The one rule that makes hand-built charts not look cheap:** the mark is driven by **real geometry from the data**, never faked. A `<div>` with a hardcoded `width: 73%` is a fake chart (banned — `anti-cheap.md`). A `<rect>` whose height is computed from a value through the normalization function below is a real one. The difference is whether the numbers flow through the coordinate math.

---

## 1. The Coordinate Normalization Primitive (the foundation of everything below)

Every hand-built chart starts here. Two pure functions map data-space → SVG-space. Get these right and every chart type is a variation on them.

```js
const W = 680, H = 240, pad = 16;              // viewBox dims + inner padding
const xs = i => pad + i * (W - 2*pad) / (pts.length - 1);   // index  → x
const ys = v => (H - pad) - (v - min) / (max - min) * (H - 2*pad); // value → y (inverted: SVG y grows down)
```

- **`min`/`max`** come from the data (`Math.min(...data)`), often padded ±5–10% so the line doesn't kiss the frame.
- **Draw into a fixed `viewBox`**, size with CSS — the chart scales without recomputation. Add `preserveAspectRatio="none"` only when you *want* the mark to stretch to fill a card (sparklines); keep it default when aspect matters (donuts).
- **Thin strokes that must not thicken when the viewBox scales:** `vector-effect: non-scaling-stroke`.

This primitive is the single most reusable thing in this file. Line, area, scatter, and bar all sit on top of it.

---

## 2. Line / Area + Draw-In

**Path generation** (straight segments):

```js
const line = 'M' + pts.map((p,i) => `${xs(i)},${ys(p)}`).join(' L');
const area = line + ` L${xs(pts.length-1)},${H-pad} L${xs(0)},${H-pad} Z`; // close to baseline
```

**Smooth curve** (Catmull-Rom → cubic Bézier) when the trend should read organic, not jagged — compute control points from neighbors and emit `C c1x,c1y c2x,c2y x,y` per segment. Use straight segments for precise/financial data, smoothing for "growth" storytelling.

**Fill** the area path with a vertical gradient (accent → transparent), defined once in `<defs>`:

```html
<linearGradient id="g" x1="0" x2="0" y1="0" y2="1">
  <stop offset="0" stop-color="var(--accent)" stop-opacity=".28"/>
  <stop offset="1" stop-color="var(--accent)" stop-opacity="0"/>
</linearGradient>
```

**Draw-in animation** (the signature motion — the line writes itself on):

```js
const len = path.getTotalLength();
path.style.strokeDasharray = len;
path.style.strokeDashoffset = len;              // hidden
gsap.to(path, { strokeDashoffset: 0, duration: 1.6, ease: 'power2.out' });
```

Fade the area fill (`opacity 0→1`) on the same timeline so it reveals *with* the line, not before it.

---

## 3. Donut / Ring Gauge + Grow

One `<circle>` for the track, one for the progress arc, same center. The whole trick is **circumference math**:

```js
const R = 54, C = 2 * Math.PI * R;
seg.style.strokeDasharray  = `${C} ${C}`;
seg.style.strokeDashoffset = C;                 // fully hidden
gsap.to(seg, { strokeDashoffset: C * (1 - pct), duration: 1.2, ease: 'power2.inOut' }); // pct ∈ [0,1]
```

**Easy-to-get-wrong details:**
- `transform: rotate(-90deg)` (or `-90` in `transform` attr around the center) so the arc **starts at 12 o'clock**, not 3.
- `stroke-linecap: round` for a soft arc end — but drop it for multi-segment donuts (round caps overlap the next segment).
- **Multi-segment donut:** each segment's `rotate` start angle = sum of all preceding segments' angles. Track a running `startAngle` accumulator.
- **Half-ring gauge (270°):** scale the dash math by `C * 270/360`; a needle is a `<line>` rotated to `-135 + pct*270` degrees.

---

## 4. Bar / Column — the value drives the height, or it's a barcode

> **The single most-botched hand-built chart.** The *barcode* failure — every bar the same height, all maxed to the container's top edge, thin, no axis labels — has one root cause: **the bar's height was never computed from the value.** A `gsap.from(scaleY: 0→1)` off a full-height base, or a `<rect>`/`<div>` sized to fill its slot, animates beautifully into a wall of identical bars. Height *is* the number. Compute it first, animate second.

**The reliable recipe (what the showcase dashboards actually ship).** A flex/grid row of `<div>` columns, each height set from `value/max`, bottom-aligned. No SVG, no coordinate math — the hardest thing to get wrong:

```html
<div class="bars">   <!-- height:230px; display:flex; align-items:end; gap:14px -->
  <div class="col"><div class="bar" style="height:64%"></div><span class="lab">Mon</span></div>
  …
</div>
```
```js
const max = 1500;    // the domain max — a round number ≥ the biggest value, NOT data-min
data.forEach((v,i) => col.innerHTML =
  `<div class="bar" style="height:${v/max*100}%"></div><span class="lab">${labels[i]}</span>`);
```

- **Anchor at zero.** `height = value/max`, never `(value − min)/(max − min)` — a bar's *area* encodes the value, so its baseline is 0. The §1 `ys` (which subtracts `min`) is for lines/scatter, **not bars**. One bar reaches ~100%, the rest sit proportionally below. If every bar is ~the same height, either the data is genuinely flat (use a line + one big delta number instead) or the mapping is broken.
- **Wide bars, few of them.** `max-width: 48–64px` per bar (or grid slots of `1fr`), **≤ ~12 columns.** More than that, or bars thinner than the gaps between them, → switch to a line/area (`dataviz.md` §1). Barcode-thin columns are the tell.
- **X-axis label on every column** (`Mon`, `Jul`, category name). A bar chart without them is unreadable — which period? which category? Dense axis → label every Nth; never rotate vertical.
- **Animate the fill, not the size.** The outer `.bar` keeps its computed `height` as a static inline style; only the inner segment (stacked) or a `scaleY` grow animates — and even a `scaleY: 0→1` targets that already-sized box, so it can never overshoot to full height.

**Stacked bar** — the *column* height is `total/max`; inner segments split *that* height by share, and only the segments animate:

```js
const total = d.reduce((a,b) => a+b, 0);
col.style.height = total/max*100 + '%';               // column sized by the total, first
d.forEach(v => seg.style.height = v/total*100 + '%'); // segments split the column, not the plane
// grow: from a scaleY:0 CSS start → gsap.to(segs, { scaleY:1, transformOrigin:'bottom', stagger:.04 })
```

**SVG path** (only when you need precise ticks or a shared coordinate space with a line overlay) — accumulate each segment's y from a running total, height still `v/total*barH`, anchored at `baseY`:

```js
let acc = 0;
segs.forEach(v => { const h = v/total*barH; rect(x, baseY - acc - h, w, h); acc += h; }); // bottom up
```

> **Broken-bar tripwire — any one is a ship-blocker:** every bar the same height · bars maxed to the container's top edge · bars thinner than the gaps (barcode) · no x-axis labels · a grouped / 2-series chart with no legend · **bars used for a trend over time** (that's a line/area — `dataviz.md` §1, `product-ui.md` §3). The first five trace to one root — the value never reached the geometry. Re-check that each height is `value/max` (or `total/max`), computed *before* any animation.

**Tooltip:** `mouseenter` reads `data-*` on the bar → position a floating `<div>` above it. Compute the offset relative to the chart's own container, not the viewport: `const r = bar.getBoundingClientRect(), cr = chart.getBoundingClientRect(); tooltip.style.left = (r.left - cr.left) + 'px'` — using viewport coordinates directly misplaces the tooltip the moment the chart isn't at the page origin. Keep **value labels visible by default** where space allows (`dataviz.md` §2: bars are Robust *only if* values aren't hover-only).

**Concentric ring KPI cluster** (cost/usage breakdown by category, alternative to a donut when nesting itself communicates "this rolls up into that"): nested plain `<div>`s (not stroke arcs), radius decreasing from an outer low-opacity ring (biggest number, e.g. total) inward to a small solid filled center (smallest/most-specific number), each labeled in place. Grow with a staggered scale-in: `gsap.to(ring, { scale: 1, duration: .8, ease: 'back.out(1.6)', delay: i * .12 })` per ring, outer-to-inner. Reach for this over a donut when the categories are hierarchical/cumulative (networking + storage + compute = total) rather than parts of one whole competing for the same 100%.

---

## 5. Small Marks (sparkline · dot-grid · in-row bar · waveforms)

- **Sparkline** — axis-less micro line: a bare `<polyline points="…">` with `vector-effect: non-scaling-stroke` + `preserveAspectRatio="none"` to fill a KPI card. No labels, no grid.
- **Dot-grid heatmap** (contribution / uptime streak) — CSS `grid` of cells, each `background: rgba(var(--accent-rgb), L)` with `L ∈ {.1, .35, .6, .85}` mapped from the value bucket. Hover → tooltip.
- **In-row bar** (data-ink density in a table) — a `<td>` with an absolutely-positioned `::after` whose `width = value/max %`. A whole distribution table becomes a bar chart for one line of CSS.
- **ECG / heart-rate wave** — a hand-written path of PQRST segments (relative `l` commands stacking the spike), translated horizontally on a `@keyframes { to { transform: translateX(-50%) } }` seamless loop; optional highlight dot rides the path via `offset-path` or GSAP MotionPath. Freeze under reduced-motion.
- **Voice waveform** — a row of `<rect>`/`<span>` bars with heights from an array; playing state animates `scaleY` with `stagger: { each: .05, from: 'random' }, repeat: -1, yoyo: true`.

---

## 6. The Three Animations × Reduced-Motion (always pair them)

> **First: make GSAP available.** Every `gsap.*` call below assumes GSAP is loaded — a `gsap.*` call with no GSAP on the page just silently does nothing (and your chart never reveals). Pick one, before your script runs:
> - **Self-host (preferred — offline-safe, zero remote dependency, version frozen):** put `gsap.min.js` (+ `ScrollTrigger.min.js` only if you use scroll) in `./lib/`, then `<script src="lib/gsap.min.js"></script>`. This is what the bundled `examples/*.html` do.
> - **CDN (only if the project allows remote deps):** `<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.13.0/gsap.min.js"></script>`.
> - **React / bundler:** `npm i gsap`, then `import { gsap } from 'gsap'`.
> - **No GSAP wanted? Degrade to CSS:** a single grow / fade / draw-in works as a CSS `transition` or `@keyframes` toggled by a class on load — keep the *same* reduced-motion terminal state. Reach for GSAP when you need **staggers, timelines, number counters, or MotionPath** (the things CSS can't do cleanly).
>
> ⚠️ The examples' `src="lib/gsap.min.js"` is **relative to the `examples/` folder** — copying that path into a new project resolves to nothing unless you actually place GSAP there. Load GSAP for real; don't ship a dead `<script src>` or an undefined `gsap`.

> **GSAP in one screen (the whole API a dashboard needs — don't rely on remembering it).**
> - `gsap.to(el, {opacity:1, y:0, duration:.7, ease:'power3.out', delay:.1})` — animate **to** these values (the default).
> - `gsap.from(el, {scaleY:0, transformOrigin:'bottom'})` — animate **from** these into the element's current state (bars/rings grow in).
> - `gsap.fromTo(el, {a}, {b})` — explicit start + end. `gsap.set(el, {attr:{d}})` — set **instantly**, no tween (per-frame slider updates, §7).
> - **stagger:** `gsap.to('.card', {y:0, opacity:1, stagger:.06})` — one call drives many, each `.06s` after the last; `stagger:{each:.05, from:'start'|'random'}` for control.
> - **SVG marks:** animate `strokeDashoffset` (line/gauge draw-in) · `scaleY`/`scaleX` + `transformOrigin` (bar/segment grow) · `attr:{...}` for raw attributes.
> - **counters:** tween a throwaway object and format in `onUpdate` (recipe below).
> - **easing:** `power2.out` / `power3.out` for reveals · `back.out(1.6)` for a pop · **never** `power1`/`linear` (reads cheap).
> - **scroll-triggered** (needs `ScrollTrigger` loaded): `gsap.to(el, {opacity:1, y:0, scrollTrigger:{trigger:el, start:'top 85%'}})` — but a dashboard's first screen is already visible, so fire on load instead (the above-the-fold variant below), not behind a ScrollTrigger.
> Every one of these pairs with its reduced-motion terminal state — the table is next.

Three motions cover ~90% of dashboard charts. **Every one ships with its reduced-motion terminal state in the same block** — this is the non-negotiable pattern, not an afterthought.

```js
const RM = matchMedia('(prefers-reduced-motion: reduce)').matches;
```

| Motion | Animated | Reduced-motion terminal state |
|--------|----------|-------------------------------|
| **Draw-in** (line/area) | `strokeDashoffset: len → 0` | `path.style.strokeDashoffset = 0` |
| **Grow** (bar/ring) | `scaleY: 0→1` / `offset: C → C*(1-pct)` | set the final `scaleY`/`offset` directly |
| **Counter** (KPI number) | tween a value, format in `onUpdate` | `el.textContent = format(target)` |

Counter recipe:

```js
if (RM) { el.textContent = target.toLocaleString(); }
else gsap.to({ v: 0 }, { v: target, duration: 1.4, ease: 'power2.out',
  onUpdate() { el.textContent = Math.round(this.targets()[0].v).toLocaleString(); } });
// prefix/suffix ($, %, +, k) get concatenated inside onUpdate
```

Batch reveal (cards/rows entering on scroll):

```js
gsap.utils.toArray('.card').forEach((el, i) =>
  gsap.from(el, { y: 24, opacity: 0, duration: .7, delay: i*.05,
    scrollTrigger: { trigger: el, start: 'top 85%' } }));
```

**Above-the-fold variant (no ScrollTrigger):** a dashboard's primary KPIs/charts are visible on load, not scrolled to — don't gate them behind `scrollTrigger`. Give every entering element a shared `.reveal` class (`opacity:0; transform:translateY(16px)`, with a `prefers-reduced-motion` override to show it plainly), then fire one `gsap.to('.reveal', { opacity:1, y:0, stagger:.06-.08, onStart: () => { /* kick off every draw-in, counter, and bar-grow here */ } })` on load. The `onStart` hook is what makes charts start drawing in lockstep with the cards revealing, instead of popping in before their container has faded up.

Plus the CSS backstop on every animated page:

```css
@media (prefers-reduced-motion: reduce){
  * { animation-duration: .01ms !important; transition-duration: .01ms !important; }
}
```

> A hand-built chart with a draw-in but **no** reduced-motion terminal state is shipping broken (it may never reveal, or reveal mid-animation, for those users). The pair is mandatory — same rule as `SKILL.md` §7.

---

## 7. Interactive Charts — Slider-Driven Live Update

Simulators (scenario sliders, retention-window controls). The rule: **draw-in uses `gsap.to` (animated); live user-driven updates use `gsap.set` (instant)** — no one wants a 1.6s tween on every pixel of drag.

```js
slider.addEventListener('input', e => {
  const t = e.target.value / 100;
  gsap.set(path, { attr: { d: recompute(t) } });   // instant, per-frame
  output.textContent = fmt(base * t);
});
```

Recompute the path `d` (or the counter target) from the new input, set it immediately. Keep the entrance draw-in for first paint only.

**Two slider tiers.** A native `<input type=range">` with just the thumb restyled (`::-webkit-slider-thumb`) is enough for a quick scenario knob. For a premium feel (retention windows, budget caps), hide the native thumb entirely (`-webkit-appearance:none; width:0; height:0` on the thumb pseudo-element) and build the fill/knob/tag yourself: an absolutely-positioned `.fill` div whose `width` and a `.knob` whose `left` are both set from the same `(value - min) / (max - min)` fraction on `input`, plus a floating value tag positioned the same way. Recompute on `resize` too, since the fraction is stored as a percentage of the track's *current* rendered width, not a fixed px value.

---

## 8. When to Hand-Build vs. Reach for a Library

Hand-built SVG is right when: single self-contained file, ≤4 chart types, full control over the mark/motion wanted, data volumes small (line <1k pts, bars <20, donut ≤6 slices — the SVG thresholds in `dataviz.md` §1). Past those volumes, or for treemap/sankey/network/geographic/candlestick, **go back to `dataviz.md` §3** — hand-rolling those is more surface area to get wrong for no gain. The finesse philosophy holds: use the tool that gets you there with the least to get wrong. For the common declarative charts in a one-file dashboard, that tool is raw SVG + GSAP — this file.
