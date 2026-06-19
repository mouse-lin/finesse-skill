# Examples — Positive Reference Corpus

Five real pages from a 37-page showcase, each a clean instance of one persona + one hero engine. Read them to see the substrate, soul, and engine rules applied together — not as templates to copy verbatim, but as proof of what "technically spectacular · soul-distinct · never cheap" looks like in shipped code.

| File | Persona | Hero engine | What to study |
|------|---------|-------------|---------------|
| `aether-cinematic-tech.html` | Cinematic Tech | A · Three.js + GLSL | fixed 3D canvas, additive-blend particles, cyan/magenta on near-black, grain + vignette |
| `nova-brutal-typographic.html` | Brutal Typographic | D · GSAP | oversized Anton headline, `mix-blend-mode: difference` nav, bone/black + hot accent, outline+fill type |
| `offscreen-editorial.html` | Editorial Publication | D · GSAP scroll-reveal | light register, Playfair + Spectral, grayscale photography, ruled hairlines, drop-caps |
| `signal-phosphor-terminal.html` | Phosphor Terminal | B · Canvas 2D | single neon-green, real-time K-line canvas, CRT scanlines + flicker, mono-forward type |
| `studio-quiet-luxury.html` | Quiet Luxury Minimal | E · CSS-only | dual-layer mouse mask (no JS lib), Raleway 100–900 weight range, max whitespace, light theme |

**Note:** these are single-file pages. `aether` references `three.module.js`; `nova`/`offscreen` use GSAP; in the original showcase those libs live in a shared `lib/` folder. Here the files are kept as **read-only reference** for structure and craft, not as standalone-runnable demos. Lift patterns, not whole files — each new brief deserves its own soul.
