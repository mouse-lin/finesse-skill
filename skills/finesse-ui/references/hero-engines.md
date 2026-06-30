# Hero Engines — The One Spectacular Moment

> **When loaded by a command** (you're iterating, not building from scratch):
> - **`bolder`** — current SPECTACLE is too low. Raise it +2 and step the engine UP one tier (CSS-only → GSAP → Canvas → Three.js/FBO). Add the new engine; keep everything else.
> - **`quieter`** — current SPECTACLE is too high / janky. Lower it −2 and step the engine DOWN (Three.js → GSAP → CSS-only). Simplify or remove; don't rebuild the page.
> - **`animate`** — SPECTACLE is fine; the motion isn't. Add or swap the engine in isolation, motion only. Don't touch palette / layout / copy.
> In all three: change **one** thing, keep the §3 substrate and the soul intact, then re-run the §1.B "spectacle shown" check and reduced-motion fallback. Full build (`craft`) reads the whole file instead.

A finesse page earns its name with **one** technically-ambitious visual engine, done at 100%, usually behind or inside the hero. Pick by soul + SPECTACLE dial. Every skeleton below is vanilla-JS-first (single-file showcase pages), with notes for framework stacks.

**Universal rules for every engine:**
- Mount on a `position: fixed; inset: 0; z-index: 0` canvas; content sits at `z-index: 5+`.
- Progressive enhancement: the page is complete and readable with the engine deleted.
- Animate only `transform`/`opacity` for DOM; for canvas, cap work to hold 60fps.
- `prefers-reduced-motion`: stop the loop on a still frame (or hide + show static hero).
- DPR-adapt every canvas: `canvas.width = innerWidth * devicePixelRatio` (else retina = blurry).
- **Loading libs:** small single-file libs (GSAP core + ScrollTrigger, ~113KB) → **self-host** in `./lib/` and commit; heavy libs with an import tree (Three.js) → **versioned CDN import map**, lazy-loaded. Never reference a `./lib/x.js` with no file behind it. Full recipe in `3d-effects.md` §2.0.

---

## Engine A — Three.js + GLSL (3D depth, particles, bloom)

Use for: astronomy, AI/neural, crypto networks, DNA/science, metaball fluids. Gate on `SPECTACLE ≥ 7`. Lazy-load `three` (~150KB gz).

```js
import * as THREE from 'three';

const canvas = document.getElementById('c');
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(60, innerWidth / innerHeight, 0.1, 2000);
camera.position.z = 600;

// 22k-particle galaxy via BufferGeometry + ShaderMaterial
const COUNT = 22000;
const pos = new Float32Array(COUNT * 3);
for (let i = 0; i < COUNT; i++) {
  const r = Math.pow(Math.random(), 0.6) * 700, a = Math.random() * Math.PI * 2;
  pos[i*3] = Math.cos(a) * r; pos[i*3+1] = (Math.random()-0.5) * 80; pos[i*3+2] = Math.sin(a) * r;
}
const geo = new THREE.BufferGeometry();
geo.setAttribute('position', new THREE.BufferAttribute(pos, 3));
const mat = new THREE.ShaderMaterial({
  uniforms: { uTime: { value: 0 }, uColor: { value: new THREE.Color('#5fd4ff') } },
  transparent: true, depthWrite: false, blending: THREE.AdditiveBlending,
  vertexShader: `uniform float uTime; void main(){ vec3 p=position;
    float a=uTime*0.05; mat2 R=mat2(cos(a),-sin(a),sin(a),cos(a)); p.xz=R*p.xz;
    vec4 mv=modelViewMatrix*vec4(p,1.0); gl_PointSize=2.0*(300.0/-mv.z); gl_Position=projectionMatrix*mv; }`,
  fragmentShader: `uniform vec3 uColor; void main(){ float d=length(gl_PointCoord-0.5);
    if(d>0.5) discard; gl_FragColor=vec4(uColor, 1.0-d*2.0); }`,
});
scene.add(new THREE.Points(geo, mat));

const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
function loop(t) { mat.uniforms.uTime.value = t * 0.001; renderer.render(scene, camera); if (!reduce) requestAnimationFrame(loop); }
requestAnimationFrame(loop);
addEventListener('resize', () => { camera.aspect = innerWidth/innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });
```
Scroll-couple it (optional): drive `camera.position.z` or `scene.rotation.y` from a GSAP ScrollTrigger `scrub`.

> For a **rendered 3D object** (GLTF product viewer, image-displacement plane) rather than a particle field, see `3d-effects.md` §2 — same Three.js setup, depth-specific recipes.

---

## Engine B — Canvas 2D (particles, fields, real-time data)

Use for: music waveforms, quant K-lines, fire/embers, aurora, flow fields, matrix rain. Light, no deps.

```js
const canvas = document.getElementById('c'), ctx = canvas.getContext('2d');
let W, H, DPR = Math.min(devicePixelRatio, 2);
function resize(){ W=innerWidth; H=innerHeight; canvas.width=W*DPR; canvas.height=H*DPR; ctx.setTransform(DPR,0,0,DPR,0,0); }
resize(); addEventListener('resize', resize);

const N = 180, P = Array.from({length:N}, () => ({ x:Math.random()*W, y:H+Math.random()*H, vy:0.4+Math.random(), r:Math.random()*2+0.5, life:Math.random() }));
const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
function frame(){
  ctx.fillStyle = 'rgba(7,4,2,0.18)'; ctx.fillRect(0,0,W,H);     // trail fade, not full clear
  for (const p of P){
    p.y -= p.vy; p.x += Math.sin(p.y*0.02)*0.4; p.life -= 0.004;
    if (p.y < -10 || p.life <= 0){ p.y=H+10; p.x=Math.random()*W; p.life=1; }
    const hue = 15 + p.life*30;                                   // ember red→amber over life
    ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fillStyle = `hsla(${hue},90%,${50+p.life*20}%,${p.life})`;
    ctx.shadowBlur = 12; ctx.shadowColor = `hsl(${hue},90%,55%)`; ctx.fill();
  }
  if (!reduce) requestAnimationFrame(frame);
}
frame();
```
Real-time data variant (K-lines, waveforms): keep a ring buffer, scroll left each frame, recompute the visible min/max for an auto-fitting price scale.

---

## Engine C — WebGL FBO shader (fluid, reaction-diffusion, ray-march)

Use for: fluid dynamics (Navier-Stokes), Gray-Scott Turing patterns, ray-marched SDF scenes, thin-film iridescence. Heavy, GPU-bound, one fullscreen quad. Gate on `SPECTACLE ≥ 8` and only when the soul is genuinely about emergence/physics.

Structure (vanilla WebGL or a thin lib like `regl`/`OGL`):
1. Two float framebuffers (ping-pong): `read` / `write`.
2. **Simulation pass** — fragment shader reads `read`, advects/diffuses/solves, draws into `write`. Swap.
3. **Display pass** — map the final state to color, draw to screen.
4. Mouse injects a "source" (dye/velocity) into the sim each frame.

```glsl
// fragment — advection step (Navier-Stokes dye transport)
uniform sampler2D uState; uniform vec2 uTexel; uniform float uDt, uDissipation;
varying vec2 vUv;
void main(){
  vec2 vel = texture2D(uState, vUv).xy;
  vec2 src = vUv - vel * uDt * uTexel;       // trace back
  gl_FragColor = texture2D(uState, src) * uDissipation;
}
```
Discipline: this is the easiest engine to ship as jank. Profile it; if it dips below 50fps on a mid device, halve the FBO resolution. Always provide a static gradient fallback poster for reduced-motion.

---

## Engine D — GSAP ScrollTrigger (scroll-pinned story, the workhorse)

Use for: scroll-driven narrative, horizontal pan galleries, parallax, reveal stagger. The single most reusable engine; works for nearly every soul at `SPECTACLE 4–7`.

**Reveal stagger** (lightest — prefer this before reaching for pins):
```js
gsap.registerPlugin(ScrollTrigger);
gsap.utils.toArray('.reveal').forEach((el, i) => {
  gsap.from(el, { opacity: 0, y: 34, duration: 0.7, ease: 'power3.out',
    scrollTrigger: { trigger: el, start: 'top 80%', once: true } });
});
```

**Horizontal pan** (pin the wrapper, scrub the track):
```js
const track = document.querySelector('.otrack');
const distance = track.scrollWidth - innerWidth;
gsap.to(track, { x: -distance, ease: 'none',
  scrollTrigger: { trigger: '.pan-wrap', start: 'top top', end: () => `+=${distance}`,
    pin: true, scrub: 1, invalidateOnRefresh: true } });
```

**Sticky-stack** (cards pin and shrink as the next arrives): pin every card except the last at `start: 'top top'`, drive each card's `scale/opacity` from the NEXT card's scrub trigger.

Critical failures to avoid: `start: 'top center'` instead of `'top top'` (fires mid-scroll, looks broken); forgetting `invalidateOnRefresh` (breaks on resize); no `ScrollTrigger.getAll().forEach(t=>t.kill())` cleanup in SPAs.

> Framework note (React/Next): wrap in `useGSAP()` / `gsap.context()` and `ctx.revert()` on unmount. For simple "enter on scroll," prefer Motion's `whileInView` over GSAP — lighter, no plugin. Gate all of it on `useReducedMotion()`.

---

## Engine E — CSS-only (mask, 3D transform, variable font, scroll-timeline)

Use for: dual-layer mouse mask (reveal a second image), CSS 3D cube, variable-font weight/width morph, native scroll-driven animation. Free, best perf, no JS libs. Often the *most* memorable because it's rare.

> Reusable, **component-level** CSS 3D — pointer-tilt cards, flip cards, coverflow, depth-parallax layers — lives in `3d-effects.md` §1 (the `depth` command). This section is the full-bleed hero variant; that one is the in-page, drop-into-a-grid variant.

**Dual-layer mouse mask** (the showcase STUDIO move):
```css
.layer-night { position:absolute; inset:0; background:url(reveal.jpg) center/cover;
  -webkit-mask: radial-gradient(circle 200px at -400px -400px, #000 40%, transparent 72%);
          mask: radial-gradient(circle 200px at -400px -400px, #000 40%, transparent 72%);
  will-change: mask; }
```
```js
addEventListener('pointermove', e => {
  const m = `radial-gradient(circle 220px at ${e.clientX}px ${e.clientY}px, #000 40%, transparent 72%)`;
  night.style.mask = m; night.style.webkitMask = m;
});
```

**Native scroll-driven** (no JS, Chrome/Safari; degrade gracefully elsewhere):
```css
@keyframes rise { from { opacity:0; transform:translateY(40px) } to { opacity:1; transform:none } }
.reveal { animation: rise linear both; animation-timeline: view(); animation-range: entry 0% cover 35%; }
```

**Variable-font morph:** animate `font-variation-settings: 'wght' …, 'wdth' …` on scroll or hover for kinetic typography (needs a variable font like Roboto Flex / Barlow).

---

## Brand Technique: Inline Typography Images

A high-soul brand move: embed a small image or short video clip directly inside a headline as a visual punctuation mark. Removes the need for a separate image row; the image becomes part of the typographic rhythm.

```html
<h1 class="inline-headline">
  Where
  <span class="inline-img" aria-hidden="true">
    <img src="your-image.jpg" alt="" />
  </span>
  craft meets code
</h1>
```

```css
.inline-img {
  display: inline-block;
  vertical-align: middle;
  width: clamp(60px, 8vw, 140px);
  height: .85em;               /* match the cap height, not the line-height */
  border-radius: 999px;
  overflow: hidden;
  position: relative;
  top: -.05em;                 /* optical lift to align with cap line */
}
.inline-img img { width: 100%; height: 100%; object-fit: cover; }
```

**Rules:**
- Gate on `SOUL ≥ 8` — it's a strong personality statement; wrong at lower soul values.
- Image must be a narrow portrait crop or square — wide landscape crops break the text rhythm.
- Font must be at display size (`clamp(56px, 9vw, 140px)+`); the technique collapses at body sizes.
- Best for: agency heroes, portfolio headlines, editorial product announcements, fashion/culture pages.
- Provide `@media (prefers-reduced-motion: reduce)` — freeze any video variant to a still frame.

---

## Framework Integration Checklist

When embedding a hero engine inside a component framework, follow these rules to prevent memory leaks, stale animations, and broken reduced-motion gates.

### React / Next.js

```js
// GSAP — wrap in useGSAP() so cleanup is automatic on unmount
import { useGSAP } from '@gsap/react';
useGSAP(() => {
  gsap.from('.hero-text', { opacity: 0, y: 40, duration: 1, ease: 'power3.out' });
  ScrollTrigger.create({ trigger: '.section', start: 'top 80%', onEnter: () => {} });
  // ctx.revert() is called automatically on unmount — all ScrollTriggers killed
}, { scope: containerRef });

// Three.js — lazy-import to keep initial bundle light
useEffect(() => {
  let renderer, rafId;
  import('three').then(THREE => {
    renderer = new THREE.WebGLRenderer({ canvas: canvasRef.current, alpha: true });
    // ... setup scene, camera, loop
    const loop = () => { rafId = requestAnimationFrame(loop); renderer.render(scene, camera); };
    loop();
  });
  return () => { cancelAnimationFrame(rafId); renderer?.dispose(); };
}, []);

// Reduced motion gate (run before starting any loop)
const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (prefersReduced) { /* freeze to still frame, skip requestAnimationFrame */ }
```

### Vue 3

```js
import { onMounted, onUnmounted } from 'vue';
let renderer, rafId;

onMounted(() => {
  // setup Three.js / Canvas here
  rafId = requestAnimationFrame(loop);
});

onUnmounted(() => {
  cancelAnimationFrame(rafId);
  renderer?.dispose();
  ScrollTrigger.getAll().forEach(t => t.kill()); // if using GSAP
});
```

### Svelte

```js
import { onMount, onDestroy } from 'svelte';
let rafId;

onMount(() => {
  // setup engine
  rafId = requestAnimationFrame(loop);
});

onDestroy(() => {
  cancelAnimationFrame(rafId);
  // dispose renderer if Three.js was used
});
```

### Common pitfalls (all frameworks)

- Missing `ScrollTrigger.getAll().forEach(t => t.kill())` on unmount causes phantom scroll triggers that fire on other pages in SPAs.
- Missing `renderer.dispose()` on Three.js WebGLRenderer leaks GPU memory — one of the most common performance bugs.
- `window.addEventListener('scroll', ...)` driving animation on every scroll event is a frame-rate killer — use `ScrollTrigger`, `IntersectionObserver`, or CSS `animation-timeline: view()` instead.
- Never start a canvas loop without gating on `prefers-reduced-motion` first. Freeze, don't skip.

---

## Choosing fast

| Soul / SPECTACLE | Engine |
|---|---|
| 3D depth, particles, "cosmic" · 7–10 | A (Three.js) |
| live data, fields, fire, neon · 5–8 | B (Canvas 2D) |
| physics/emergence/fluid · 8–10 | C (WebGL FBO) |
| narrative scroll, gallery, editorial · 4–7 | D (GSAP) |
| interactive reveal, kinetic type, max-perf · 3–6 | E (CSS-only) |

When unsure, **D or E**. They cover most briefs at the lowest risk and never jank.
