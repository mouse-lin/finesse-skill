# finesse — Agent Instructions

> **For OpenAI Codex and other agent runtimes that read `AGENTS.md`.**
> The full skill is at `skills/finesse-ui/SKILL.md`. Deep reference material lives in `skills/finesse-ui/references/`. Load what you need per phase — do not inline everything at once.

---

## What finesse does

finesse builds two kinds of interface, routed by **register**:

- **brand** — landing pages, brand sites, launches, portfolios, hero pages. Design IS the product. Optimize for spectacle + soul + first impression. Uses a real visual engine (Three.js / Canvas / GLSL / GSAP).
- **product** — dashboards, admin panels, analytics, data tables, app shells, settings. Design SERVES the product. Optimize for clarity + density + usability.

Both share the same premium substrate and anti-slop blacklist. Neither is allowed to look cheap.

---

## Mandatory workflow (execute in order)

### 1. Design Read — before any code
Read the brief. Determine the register. Output exactly one line:

```
Design Read: {industry} · {soul in 2–3 words} · register={brand|product} · SPECTACLE={1–10} · hero-engine={type or "none"}
```

Then name the lazy default aesthetic for this brief and state how you are beating it.

### 2. Set the three dials
- **SOUL** (1–10): how opinionated/branded the personality is
- **SPECTACLE** (1–10): how technically-ambitious the visual engine is
- **DENSITY** (1–10): information per viewport

Product register: pin SPECTACLE ≤ 4, DENSITY ≥ 6.

If SPECTACLE ≥ 7, you MUST ship a working engine. A gradient blob at SPECTACLE 8 is broken. Drop to 4 and ship a polished static page instead.

### 3. Lay the premium substrate (both registers)
Non-negotiables — see `skills/finesse-ui/references/design-dna.md` for exact values:
- SVG grain layer (`feTurbulence`, `opacity .025–.05`)
- Radial-gradient vignette on dark heroes
- Display headings: `clamp()` size, negative tracking (`-.02 to -.045em`), `line-height .86–.95`, extreme weight contrast
- Translucent borders (`rgba(255,255,255,.07–.22)` dark / `rgba(0,0,0,.06–.08)` light)
- No pure `#fff` / `#000` — tint toward brand hue
- Color lock: one accent owns the whole page

### 4. Fork by register

**brand path:**
- Pick a soul persona from `skills/finesse-ui/references/style-personas.md`
- Build one hero engine from `skills/finesse-ui/references/hero-engines.md`
- `prefers-reduced-motion` fallback is mandatory

**product path:**
- Build component system + data viz per `skills/finesse-ui/references/product-ui.md`
- Skip soul personas and hero engines

### 5. Run the cheapness blacklist
Scan before shipping. Hard bans:
- Em-dashes in copy as a flourish (most-violated AI tell — zero exceptions)
- Gradient text, default glassmorphism, AI-purple glow, side-stripe card borders
- Eyebrow label on every section; numbered `01 · 02 · 03` section markers
- Identical 6-card grids (icon + title + text × 6)
- Fake-precise numbers (`92%`, `4.1×`) with no source
- Default-category palettes reached for without reason
- Zero imagery on image-implied briefs

### 6. Pre-flight before declaring done
See `skills/finesse-ui/references/preflight.md`. Mandatory gates:
- [ ] Design Read output + dials documented
- [ ] Substrate applied (grain, vignette, type tension, color lock)
- [ ] SPECTACLE claimed = SPECTACLE shipped
- [ ] Cheapness blacklist cleared
- [ ] `prefers-reduced-motion` handled on every animation
- [ ] WCAG AA contrast: body ≥ 4.5:1, large text ≥ 3:1
- [ ] Layout families diversified (no repeated pattern dominance)
- [ ] Mobile layout declared per multi-column section

If any gate fails: fix it. Do not ship and note it as a known issue.

---

## Reference files

| File | Load when |
|------|-----------|
| `skills/finesse-ui/references/design-dna.md` | Implementing the substrate layer |
| `skills/finesse-ui/references/hero-engines.md` | Building a brand hero engine |
| `skills/finesse-ui/references/style-personas.md` | Choosing a brand soul/persona |
| `skills/finesse-ui/references/anti-cheap.md` | Running the cheapness audit |
| `skills/finesse-ui/references/product-ui.md` | Product register: dashboards, tables, charts |
| `skills/finesse-ui/references/preflight.md` | Pre-flight checklist |
| `skills/finesse-ui/references/design-model.md` | Multi-page token consistency |

---

## Out of scope

Pure backend / API / data tasks with no UI. Briefs that explicitly require generic, zero-craft output (finesse always brings craft — if the user wants bland, that is a different tool).
