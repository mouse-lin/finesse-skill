# Examples — Positive Reference Corpus

Thirteen real pages from a showcase corpus (5 brand + 8 dashboards), each a clean instance of one persona + one hero engine (brand register) or one dashboard morphology (product register). Read them to see the substrate, soul, and engine — or the layout shell, chart-crafting, and reduced-motion rules — applied together in shipped code, not as templates to copy verbatim. The 8 dashboards are **self-contained and viewable**: GSAP is bundled at `lib/gsap.min.js`, avatars/photos are inline data-URIs (no remote assets), so opening any one in a browser renders and animates.

The other pages aren't bundled as files (repo-size reasons — several carry Three.js/WebGL payloads), but every brand-register one is catalogued as a technique note in `../references/inspiration-catalog.md`, grouped by persona. Check there for a second reference point when these don't cover the brief's industry or mood.

## Brand register (persona + hero engine)

| File | Persona | Hero engine | What to study |
|------|---------|-------------|---------------|
| `aether-cinematic-tech.html` | Cinematic Tech | A · Three.js + GLSL | fixed 3D canvas, additive-blend particles, cyan/magenta on near-black, grain + vignette |
| `nova-brutal-typographic.html` | Brutal Typographic | D · GSAP | oversized Anton headline, `mix-blend-mode: difference` nav, bone/black + hot accent, outline+fill type |
| `offscreen-editorial.html` | Editorial Publication | D · GSAP scroll-reveal | light register, Playfair + Spectral, grayscale photography, ruled hairlines, drop-caps |
| `signal-phosphor-terminal.html` | Phosphor Terminal | B · Canvas 2D | single neon-green, real-time K-line canvas, CRT scanlines + flicker, mono-forward type |
| `studio-quiet-luxury.html` | Quiet Luxury Minimal | E · CSS-only | dual-layer mouse mask (no JS lib), Raleway 100–900 weight range, max whitespace, light theme |

## Product register (dashboard morphology)

Pulled from a batch of 10 demo dashboards built to stress-test `../references/product-ui.md` and `../references/chart-crafting.md`. These eight cover the full morphology range — canonical sidebar (dark + light), floating rounded panel, true bento, asymmetric bento, top-nav triptych, top-nav centered bento — plus tables, gauges, stacked/grouped bars, sparklines, KPI rows, and the browser-mockup/hotspot pattern. The four **new** ones (nodeflux/inkline/huddle/ledgerio) are **re-themed to fresh neutral domains** (not verbatim showcase copies) so you study the *structure*, then bring your own soul. Match the example to the morphology and light-vs-dark you're building (`product-ui.md` §0–§1); the remaining 2 (photo-studio, reelflow) live as technique notes in `../references/inspiration-catalog.md`.

| File | Shell | What to study |
|------|-------|----------------|
| `buildly-ai/index.html` → `buildly-growth-dashboard.html` | Canonical sidebar + topbar (dark) | The default shell from `product-ui.md` §1 in its cleanest form: line+area draw-in chart, donut arc, five KPI number-roll-up counters, native-thumb slider (`chart-crafting.md` §7's simple tier) |
| `stakent-monitoring-dashboard.html` | Canonical sidebar + topbar (dark) | Glowing sparklines (`feGaussianBlur` filter layered on the stroke — a polish detail not in `chart-crafting.md`'s base recipe), the premium custom knob+fill+tag slider (`chart-crafting.md` §7's second tier) |
| `acru-financial-dashboard.html` | Canonical sidebar + topbar (light) | Stacked bar chart with a real hover tooltip positioned relative to the chart container not the viewport (`chart-crafting.md` §4), half-ring gauge (`chart-crafting.md` §3), credit-card-styled widget |
| `pawcare-adaptive-health.html` | Floating rounded panel | Hotspot-annotation pins on a real photo (`product-ui.md` §6), a combined accent-color-picker + dark-mode toggle that keeps the two independent (`theming.md`'s accent-recolor boundary), hand-built ECG waveform and ring/segment bars (`chart-crafting.md` §5) |
| `nodeflux-api-console.html` *(re-themed → email delivery)* | Floating panel + **true bento** | The `2.15fr 1.7fr 1fr 1fr 1fr` bento recipe (`product-ui.md` §1): dark gradient key-card, multi-stat stack, concentric usage rings (`chart-crafting.md` §4), latency sparkline, activity waveform — cards varied by content role, not just size |
| `inkline-publishing-cms.html` *(re-themed → knowledge base)* | Top-nav **triptych** `296px 1fr 320px` | Three-column masonry (`align-items:start`) + a CSS browser-preview mockup with staggered hotspot pins (`product-ui.md` §6); weekly line+area draw-in, stat+sparkline tiles, numbered score row |
| `huddle-team-workspace.html` *(re-themed → course learning)* | Top-nav **centered bento** (no sidebar) | Colored task cards, multi-arc donut (`chart-crafting.md` §3), grouped/paired bar chart (`chart-crafting.md` §4), leaderboard, chat bubbles + voice-waveform |
| `ledgerio-treasury-console.html` *(re-themed → subscription analytics)* | Floating panel + **asymmetric bento** `1fr 1.35fr` | Color-blocked cards (dark pulse · mint gauge · mint/lav price), stacked bar (`chart-crafting.md` §4), a cyclable circular gauge, and an allocation **table with animated mini-bars** (`chart-crafting.md` §6 — the table-bar animation) |

**Note:** these are single-file pages. The **8 dashboards are self-contained and runnable** — GSAP loads from the bundled `lib/gsap.min.js` (+ `lib/ScrollTrigger.min.js`), and every avatar/photo is an inline data-URI (no `assets/` folder, no remote hotlinks). The **brand** pages still reference external libs (`aether` → `three.module.js`) and are read-only references. Lift patterns, not whole files — each new brief deserves its own soul, and (per `chart-crafting.md` §6) load GSAP for real in your own project rather than copying the `lib/` path.
