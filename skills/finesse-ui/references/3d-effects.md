# 3D Effects — Depth Without the Gimmick

> **When loaded by the `depth` command** (you're adding/iterating a 3D moment, not rebuilding):
> change **one** thing, keep the §3 substrate and the soul intact, then re-run the §1.B "spectacle shown" check and the reduced-motion fallback. A full build (`craft`) reads this only if the brief calls for depth.

3D on the web splits into two registers — and reaching for the wrong one is the #1 mistake.

| | **CSS 3D (pseudo / 2.5D)** | **Three.js (real 3D)** |
|---|---|---|
| What it is | DOM elements rotated in perspective space | actual meshes, cameras, lights, shaders rendered to WebGL |
| Cost | free, no deps, ships in any page | ~150KB gz, lazy-load, GPU-bound |
| Right for | tilt cards, flip cards, coverflow, depth-parallax, kinetic layers | product/model viewers, image-displacement planes, particle depth, scroll-driven scenes |
| SPECTACLE | 3–6 | 7–10 |

**Default to CSS 3D.** It covers ~80% of "make it feel 3D" briefs at zero risk and zero jank. Reach for Three.js only when the brief genuinely needs a rendered object (a rotatable product, a distorted image plane, real depth-of-field) — never for a tilt or a flip.

---

## 0. Does 3D earn its place? (anti-slop gate)

Run this before writing a line. 3D is the easiest effect to ship as a tell.

- **Motivated only.** Depth must serve hierarchy, product comprehension, or playful identity — never "it looked cool." A spinning cube on a B2B page is slop.
- **One 3D moment per page**, like the hero engine. A tilt-card grid *and* a coverflow *and* a Three.js scene = noise. Pick the one that carries the soul.
- **Subtle beats showy.** Tilt ranges of ±20°+ read as a cheap CodePen demo. The premium version is ±8–14° with a slow ease-out return.
- **It must survive no-perspective.** Page is fully readable and usable with every 3D transform stripped. 3D is enhancement, never structure.

---

## 1. CSS 3D — the reusable, component-level layer

### 1.A Perspective fundamentals

3D lives in a two-element contract: a **parent** that owns the vanishing point, a **child** that rotates in it.

```css
.scene  { perspective: 900px; }              /* smaller = stronger distortion; 700–1200 typical */
.card   { transform-style: preserve-3d; transition: transform .5s cubic-bezier(.2,.8,.2,1); }
.card > .lift { transform: translateZ(40px); }  /* nested layers float at real depth */
```
- `perspective` on the **parent**, `transform-style: preserve-3d` on the **moving** element (and every ancestor between the two, or depth collapses).
- Put `perspective` on a wrapper, not the body, so each component owns its own vanishing point.

### 1.B Pointer-tilt card (the signature move)

The single highest-value 3D effect: a card that leans toward the cursor with a light glare.

```js
const r = card.getBoundingClientRect.bind(card);
card.addEventListener('pointermove', e => {
  const b = card.getBoundingClientRect();
  const px = (e.clientX - b.left) / b.width  - 0.5;   // -0.5 … 0.5
  const py = (e.clientY - b.top)  / b.height - 0.5;
  card.style.transform = `rotateY(${px * 12}deg) rotateX(${-py * 12}deg)`;
  card.style.setProperty('--gx', `${(px + 0.5) * 100}%`);
  card.style.setProperty('--gy', `${(py + 0.5) * 100}%`);
});
card.addEventListener('pointerleave', () => { card.style.transform = ''; });
```
```css
.card { position: relative; transform-style: preserve-3d; transition: transform .4s ease-out; }
.card::after {                                   /* cursor-tracked glare */
  content:''; position:absolute; inset:0; border-radius:inherit; pointer-events:none;
  background: radial-gradient(circle at var(--gx,50%) var(--gy,50%),
              rgba(255,255,255,.18), transparent 45%); opacity:0; transition:opacity .3s;
}
.card:hover::after { opacity:1; }
```
- `pointerleave` resets via empty string so the **CSS transition** eases it home — don't animate the return in JS.
- ±12° max. Push glare, not rotation, for intensity.
- **No hover on touch** → either skip (graceful) or drive from `deviceorientation` (gate behind a permission tap on iOS).

### 1.C Flip card (front ⇄ back)

```css
.flip { perspective: 1000px; }
.flip-inner { transform-style: preserve-3d; transition: transform .7s cubic-bezier(.2,.8,.2,1); }
.flip:hover .flip-inner,
.flip:focus-within .flip-inner { transform: rotateY(180deg); }
.flip-face { position:absolute; inset:0; backface-visibility:hidden; }
.flip-back { transform: rotateY(180deg); }
```
Gate the trigger on `:focus-within` too, so it's keyboard-reachable. One flip per card; a wall of flipping cards is slop.

### 1.D Coverflow / 3D carousel

Items fan back in Z; the centered one faces forward. Drive by an `active` index.

```js
function layout(items, active) {
  items.forEach((el, i) => {
    const off = i - active;                                    // signed distance from center
    const sign = Math.sign(off), mag = Math.min(Math.abs(off), 3);
    el.style.transform =
      `translateX(${off * 56}%) translateZ(${-mag * 140}px) rotateY(${-sign * mag * 34}deg)`;
    el.style.zIndex = String(10 - mag);
    el.style.opacity = String(Math.max(0, 1 - mag * 0.28));
  });
}
```
Parent needs `perspective: 1400px`. Advance `active` on click / arrow keys / drag. Cap visible depth (`mag` clamp) so far cards don't render to nothing.

### 1.E Depth-parallax layers (mouse-reactive)

A stacked scene where layers drift at different rates → real diorama depth, not a flat parallax fake.

```js
const layers = [...scene.querySelectorAll('[data-depth]')];   // data-depth="0.2" … "1"
scene.addEventListener('pointermove', e => {
  const b = scene.getBoundingClientRect();
  const x = (e.clientX - b.left) / b.width  - 0.5;
  const y = (e.clientY - b.top)  / b.height - 0.5;
  for (const l of layers) {
    const d = +l.dataset.depth;
    l.style.transform = `translate3d(${x * d * -40}px, ${y * d * -40}px, ${d * 60}px)`;
  }
});
```
`translateZ` (the `d * 60px`) is what separates this from a 2D parallax — give the scene `perspective` and the layers gain genuine spatial separation.

### 1.F Scroll-driven 3D (no JS)

Native scroll-timeline rotates an element as it enters — best perf, degrades to "no rotation" where unsupported.

```css
@keyframes deck { from { transform: rotateX(28deg) translateY(60px); opacity:0; }
                  to   { transform: none; opacity:1; } }
.card-3d { transform-origin: 50% 100%;
  animation: deck linear both; animation-timeline: view(); animation-range: entry 0% cover 40%; }
```

---

## 2. Three.js — real 3D, only when the brief needs a rendered object

Setup, DPR, reduced-motion, and `renderer.dispose()` cleanup follow `hero-engines.md` Engine A. These are the depth-specific recipes beyond the particle galaxy.

### 2.0 Loading the library (self-host vs CDN — decide by weight)

A library is never *in* this skill — the skill emits the page, and the page loads the library. Which way depends on size, not preference:

| Library | Weight | How to load | Why |
|---|---|---|---|
| **GSAP** core + ScrollTrigger | ~70KB + ~43KB, single files, no dependency tree | **self-host** → `./lib/gsap.min.js` + `./lib/ScrollTrigger.min.js`, plain `<script src>` | small, offline-safe, zero remote dependency; version frozen with the page |
| **Three.js** core + addons | ~1.2MB core + an addon **dependency tree** (`three/addons/*` import each other and `three`) | **versioned CDN** via import map, **lazy-loaded** | too heavy to vendor; hand-vendoring the addon tree is fragile |

**Rule of thumb:** small single-file lib → drop it in `./lib/` and commit it. Heavy lib with an internal import tree → pin a version on a CDN. Never leave a `./lib/x.js` reference with no file behind it (the original sin these examples shipped with).

```html
<!-- Three.js — versioned import map; core + addons MUST share the exact version -->
<script type="importmap">
{ "imports": {
  "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js",
  "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"
}}
</script>
```

- **Pin the version** (`three@0.160.0`), never a bare `three` or `@latest` — a silent major bump breaks the addon API.
- **Lazy-load** the heavy import so it never blocks first paint: `import()` the engine inside an `IntersectionObserver` when the canvas scrolls into view (see §2.B).
- Going fully offline (kiosk, archival)? Then vendor `three.module.js` *and* every addon you import into `./lib/` and point the import map at local paths — accept the ~1.5MB+ repo cost.

### 2.A Image-displacement plane (the "WebGL image" hover)

A textured plane that ripples/dissolves on hover or scroll — the premium gallery move.

```glsl
// vertex — push verts along normal by a noise field × progress
uniform float uProgress; varying vec2 vUv;
void main(){ vUv = uv;
  float w = sin(uv.x * 8.0 + uProgress * 6.28) * cos(uv.y * 8.0) * uProgress * 0.18;
  vec3 p = position + normal * w;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(p, 1.0); }
```
```glsl
// fragment — sample the texture with a progress-driven UV skew
uniform sampler2D uTex; uniform float uProgress; varying vec2 vUv;
void main(){ vec2 uv = vUv + vec2(0.0, (vUv.x - 0.5) * uProgress * 0.1);
  gl_FragColor = texture2D(uTex, uv); }
```
Animate `uProgress` 0→1 with GSAP on `pointerenter` / back on `pointerleave` (or scrub from ScrollTrigger). One plane = one `PlaneGeometry(w, h, 32, 32)` — enough segments for smooth displacement, not so many it janks.

### 2.B Product / model viewer (GLTF, orbit, auto-rotate)

```js
const { GLTFLoader } = await import('three/addons/loaders/GLTFLoader.js');
new GLTFLoader().load(url, gltf => {
  scene.add(gltf.scene);
  scene.add(new THREE.HemisphereLight(0xffffff, 0x222233, 1.1));
});
let drag = 0, vel = 0.003;                       // idle auto-spin
canvas.addEventListener('pointerdown', e => { drag = e.clientX; vel = 0; });
addEventListener('pointermove', e => { if (drag) { scene.rotation.y += (e.clientX - drag) * 0.01; drag = e.clientX; } });
addEventListener('pointerup', () => { drag = 0; vel = 0.003; });
// in loop: if (!drag) scene.rotation.y += vel;
```
- Lazy-load `three` + the loader only when the viewer scrolls into view (`IntersectionObserver`) — never on initial paint.
- Soft studio light (hemisphere + one key) reads premium; a single flat light reads cheap.
- Provide a **static product photo poster** for reduced-motion and load failure — never a blank canvas.

### 2.C Scroll-coupled 3D scene

Drive `camera.position.z`, `scene.rotation.y`, or a model's morph from a GSAP ScrollTrigger `scrub` (see `hero-engines.md` Engine D). The camera flies through / around as the user scrolls — strong for a single product-story page, overkill for a marketing grid.

---

## 3. Discipline (mandatory — same gates as every engine)

- **`prefers-reduced-motion`:** freeze tilt/parallax/auto-rotate to the rest state; show a still poster for Three.js scenes. Never ship 3D motion with no fallback.
- **60fps:** animate only `transform` / `opacity` for CSS; cap geometry segments and pixel ratio (`min(devicePixelRatio, 2)`) for WebGL. Test on a mid-range device — perspective math is cheap, but shadow-casting lights and high-segment planes are not.
- **Flatten on mobile:** pointer-tilt and mouse-parallax have no input on touch. Either disable (the card just sits flat — fine) or wire `deviceorientation` deliberately. Don't ship a desktop-only interaction with no mobile state.
- **Substrate intact:** 3D layers obey the §3 z-index order (engine 0 · grain 1 · content 5) and the color/accent lock. A tilt card still gets the grain, the translucent border, the tinted neutral.
- **Backface + overflow:** `backface-visibility: hidden` on flip faces; watch `overflow: hidden` on a perspective parent — it clips children popping toward the viewer in `translateZ`.

---

## 4. Choosing fast

| Brief | Effect | Tier |
|---|---|---|
| cards that react to the cursor | §1.B pointer-tilt | CSS |
| reveal a back side / spec sheet | §1.C flip card | CSS |
| browse a set, one-at-a-time focus | §1.D coverflow | CSS |
| layered hero with diorama depth | §1.E depth-parallax | CSS |
| sections that assemble on scroll | §1.F scroll-driven | CSS |
| gallery images that ripple/dissolve | §2.A displacement plane | Three.js |
| rotatable product / 3D object | §2.B GLTF viewer | Three.js |
| fly-through product story | §2.C scroll-coupled scene | Three.js |

When unsure, **stay in CSS (§1).** It's rarer to see done well, never janks, and reads as more premium than a heavy Three.js scene shipped at 40fps.
