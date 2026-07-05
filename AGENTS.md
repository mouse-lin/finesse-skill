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

## Commands (targeted iteration)

A full build runs the whole workflow below. For iterating on an existing page, route the request through one verb instead of rebuilding — Codex has no slash commands, so map the user's intent to the verb and load only that reference.

| Verb | Intent it matches | Load |
|------|-------------------|------|
| `craft` | "build me a landing page / dashboard" (default) | full workflow |
| `init` | "set up finesse / new project" → write `PRODUCT.md` brief | `references/init.md` |
| `document` | "capture / document the design system" → extract `design-model.yaml` | `references/document.md` |
| `audit` | "is this any good?", "review it" — **read-only**, never edits | `references/audit.md` (runs `scripts/detect.mjs`) |
| `bolder` | "too plain / boring" → SPECTACLE +2, engine up a tier | `references/hero-engines.md` |
| `quieter` | "too flashy / janky" → SPECTACLE −2, engine down a tier | `references/hero-engines.md` |
| `soul` | "feels generic / wrong vibe" → re-pick persona, re-lock color | `references/style-personas.md` |
| `animate` | "make it move / different animation" → add/swap engine only | `references/hero-engines.md` |
| `depth` | "add depth / make it 3D / tilt / parallax" → add one 3D moment (CSS pseudo-3D, or Three.js for a real object) | `references/3d-effects.md` |
| `densify` | "too sparse / too dense" → DENSITY ± | `references/product-ui.md` |
| `redesign` | "improve / fix this page" → audit-first, never full-rebuild | `references/redesign-mode.md` |

For a single complaint, change **one** thing, keep the substrate and soul, then re-run the relevant pre-flight gates.

---

## Mandatory workflow (execute in order)

### 1. Design Read — before any code
If a `PRODUCT.md` exists at the project root, read it first — it's the locked brief (register, soul, dials, anti-references) and overrides your guesses; read `design-model.yaml` too if present (locked palette/type/substrate). Missing on a multi-page/repeat project? Offer `init` (and `document` for existing code) before generating.

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
Run the local detector first (no network, pure file scan), then add your own eye:

```
node skills/finesse-ui/scripts/detect.mjs --json <target ...>
```

It reports findings P0/P1/P2 with `file:line`, a `p0` count, and a `notCovered[]` list of tells it can't see. It **always exits 0** (findings are data in the JSON, not a tool failure — read `p0`; `--strict` makes a P0 block with a non-zero exit for CI). A clean run means "no regex-detectable slop", not "good page" — always continue into the by-hand pass. If the script is absent, scan by hand; don't treat its absence as a pass. Hard bans:
- Em-dashes in copy as a flourish (most-violated AI tell — zero exceptions)
- Gradient text, default glassmorphism, AI-purple glow, side-stripe card borders
- Eyebrow label on every section; numbered `01 · 02 · 03` section markers
- Identical 6-card grids (icon + title + text × 6)
- Fake-precise numbers (`92%`, `4.1×`) with no source
- Default-category palettes reached for without reason
- Zero imagery on image-implied briefs

### 6. Pre-flight before declaring done
See `skills/finesse-ui/references/preflight.md`. Run `node skills/finesse-ui/scripts/detect.mjs --json <target>` and confirm `p0` is 0 (the script always exits 0; the count lives in the JSON), and when a browser is available, open the page and screenshot the hero to confirm the engine renders real pixels (not a white/flat screen). Mandatory gates:
- [ ] Design Read output + dials documented
- [ ] Substrate applied (grain, vignette, type tension, color lock)
- [ ] SPECTACLE claimed = SPECTACLE shipped (detector + browser-verified, not just asserted)
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
| `skills/finesse-ui/references/3d-effects.md` | Adding a 3D moment: CSS tilt/flip/coverflow/parallax or Three.js model/displacement |
| `skills/finesse-ui/references/style-personas.md` | Choosing a brand soul/persona |
| `skills/finesse-ui/references/inspiration-catalog.md` | Wider bench of real pages per persona, or a brief that doesn't fit any of the 10 personas |
| `skills/finesse-ui/references/anti-cheap.md` | Running the cheapness audit |
| `skills/finesse-ui/references/audit.md` | Read-only diagnostic (cheapness + spectacle + pre-flight) |
| `skills/finesse-ui/references/product-ui.md` | Product register: dashboards, tables, charts |
| `skills/finesse-ui/references/preflight.md` | Pre-flight checklist |
| `skills/finesse-ui/references/redesign-mode.md` | Upgrading an existing page, audit-first |
| `skills/finesse-ui/references/design-model.md` | Multi-page token consistency |
| `skills/finesse-ui/references/init.md` | New project: write `PRODUCT.md` brief |
| `skills/finesse-ui/references/document.md` | Existing code: extract `design-model.yaml` |
| `skills/finesse-ui/scripts/detect.mjs` | Local slop + spectacle detector (run in audit / pre-flight) |

---

## Out of scope

Pure backend / API / data tasks with no UI. Briefs that explicitly require generic, zero-craft output (finesse always brings craft — if the user wants bland, that is a different tool).
