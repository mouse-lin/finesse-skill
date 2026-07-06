---
name: finesse-ui
description: Build never-cheap, high-craft web interfaces — both brand surfaces (landing pages, brand sites, launches, portfolios, hero pages with real WebGL/Three.js/Canvas/GSAP engines) and product UI (dashboards, admin panels, analytics, data tables, app shells, settings). Routes by register: brand → soul + spectacle engine; product → component system + information density + data viz. Always reads the brief first and audits against an anti-slop cheapness blacklist. Supports verb commands (audit · bolder · quieter · soul · animate · densify · redesign) for targeted iteration on existing pages. Triggers on "make this look premium", "landing page", "dashboard", "admin panel", "analytics UI", "data table", "app UI", "give it a soul / a vibe", "anti-slop", "hero animation", "/finesse".
version: 0.5.0
user-invocable: true
argument-hint: "[craft · audit · bolder|quieter|soul · animate|depth|densify · redesign · init|document] [target]"
license: MIT
---

# finesse — Technically Spectacular · Soul-Distinct · Never Cheap

> **finesse builds two kinds of interface and routes by register (§0):**
> - **brand** — design IS the product: landing pages, brand sites, launches, portfolios, hero pages. Optimize for **spectacle + soul + first impression** — a real visual engine, an opinionated personality.
> - **product** — design SERVES the product: dashboards, admin panels, analytics, data tables, app shells, settings. Optimize for **clarity + density + usability** — and still never cheap.
>
> The through-line is identical: **high craft, zero AI-slop.** The premium substrate (§3), the cheapness blacklist (§6), and the pre-flight (§8) apply to **both**. What forks is the middle: brand reaches for a hero engine (§4); product reaches for a component system + data viz (`references/product-ui.md`).
>
> Every rule below is **contextual**. Nothing fires automatically. Read the brief, set the register, then pull only what fits. A skill that produces the same page for every brief has failed.

---

## How to use this skill

1. Run **§0 Brand Read** — infer **register** (brand vs product) + soul before touching code. Output a one-line Design Read.
2. Set the **§1 Three Dials** (SOUL · SPECTACLE · DENSITY). Product register pins SPECTACLE low, DENSITY high.
3. **Both paths:** lay the **§3 Premium Substrate** (`references/design-dna.md`) — the physical layer that reads as expensive (it keeps dashboards from looking cheap too).
4. **Then the paths fork:**
   - **brand** → pick a **§2 Soul** (`references/style-personas.md`) and build **one §4 Hero Engine** (`references/hero-engines.md`).
   - **product** → build a **component system + data viz** per `references/product-ui.md` (density, tables, charts, forms, interaction states).
5. Assemble the **§5 page skeleton**, motion-motivated only.
6. Run the **§6 Cheapness Blacklist** (`references/anti-cheap.md`) and **§8 Pre-Flight** (`references/preflight.md`) before shipping.

The `references/*.md` files are the deep material. Load the one you need for the current phase — do not inline all of them.

| Reference | When to load |
|-----------|-------------|
| `design-dna.md` | Laying the premium substrate (grain, vignette, type, color tokens) |
| `hero-engines.md` | Building the hero engine (brand register) |
| `3d-effects.md` | Adding a 3D moment — CSS tilt/flip/coverflow/depth-parallax or Three.js model/displacement |
| `style-personas.md` | Picking a soul (brand register) |
| `inspiration-catalog.md` | Persona picked but you want a wider menu of proven techniques for that soul, or the brief doesn't fit any of the 10 personas cleanly |
| `anti-cheap.md` | Before any delivery — cheapness scan |
| `product-ui.md` | Dashboard / admin / data app (product register) |
| `dataviz.md` | Chart-heavy product UI beyond the starter table — full 25-type selection matrix, a11y grade + mandatory fallback, library picks |
| `preflight.md` | Final checklist before saying "done" |
| `design-model.md` | Multi-page projects — token consistency |
| `redesign-mode.md` | Upgrading an existing page — audit-first protocol |
| `audit.md` | Read-only diagnostic — cheapness + spectacle + preflight scan |
| `init.md` | New project — write `PRODUCT.md` (the persistent brief) |
| `document.md` | Existing codebase — extract `design-model.yaml` from real code |

---

## Commands

finesse runs as a full build by default, but supports **verb commands** for targeted iteration on an existing page — so you don't re-run the whole Brand Read for a single complaint. Each command loads one reference and does one job.

| Command | Category | Does | Reference |
|---------|----------|------|-----------|
| `craft [brief]` | Build | The full flow: Brand Read → Dials → substrate → engine → assemble (the default) | all |
| `init` | Setup | New project: write `PRODUCT.md` (register, soul, locked dials, anti-references) — the brief every later task reads | `init.md` |
| `document` | Setup | Existing codebase: extract the built design system into `design-model.yaml`; report drift | `document.md` |
| `audit [target]` | Evaluate | **Read-only** diagnostic: run the cheapness blacklist + spectacle-shown + pre-flight, output a findings list. **Changes nothing.** | `audit.md` |
| `bolder [target]` | Refine | Raise SPECTACLE +2, upgrade the engine (e.g. Canvas → Three.js) | `hero-engines.md` |
| `quieter [target]` | Refine | Lower SPECTACLE −2, step down to GSAP / CSS-only; calm an overloaded page | `hero-engines.md` |
| `soul [target]` | Refine | Re-pick the persona / soul when a page "feels generic" or wrong-vibe | `style-personas.md` |
| `animate [target]` | Enhance | Add or swap the hero engine in isolation; motion only | `hero-engines.md` |
| `depth [target]` | Enhance | Add **one** 3D moment — CSS pseudo-3D (tilt · flip · coverflow · depth-parallax) or Three.js (model viewer · image displacement) | `3d-effects.md` |
| `densify [target]` | Enhance | Adjust DENSITY ± — add/remove content, tune information-per-viewport | `product-ui.md` |
| `redesign [target]` | Iterate | Upgrade an existing page, audit-first; never full-rebuild for one complaint | `redesign-mode.md` |

### Routing rules

1. **First word matches a command** → load that command's reference and follow it. Everything after the command name is the target. Lay the **§3 substrate** and run the relevant **§6/§8 checks**, but skip the parts of §0–§5 that don't apply to that single action (e.g. `quieter` doesn't re-pick a soul).
2. **First word doesn't match, but intent clearly maps to one command** ("too plain / boring" → `bolder`; "too flashy" → `quieter`; "feels generic" → `soul`; "make it pop" → `animate`; "add depth / make it 3D / tilt / parallax" → `depth`; "too sparse / too dense" → `densify`; "improve / fix this page" → `redesign`) → route to that command and proceed as if invoked. If two fit, ask once which.
3. **No argument at all** (bare `/finesse`) → the user is asking *"what should I do here?"* Don't dump the static menu. Read a few cheap signals and **lead with the 2-3 highest-value commands**, each with a one-line reason, then offer the full table as fallback. Never auto-run — recommend, the user confirms. Signal → pick:
   - **no `PRODUCT.md`** and there's real code/pages → lead with `document` (capture what's built) and/or `init` (write the brief). Brand-new empty project → `init` then `craft`.
   - **`PRODUCT.md` exists, has built pages, never audited** → lead with `audit <surface>` (read-only health check).
   - **git working tree points at one page/file** → scope `audit` or `redesign` to those files, naming them.
   - **a recent `audit` found P0/P1** → lead with `redesign` (fix the backlog) or the specific refine verb the findings point to (gradient-text/eyebrows → `quieter`/`soul`; flat motion → `animate`).
   - **nothing built yet, clear brief** → `craft`.
   Keep it to 2-3 pointed picks with the exact command to type. The menu is the fallback, not the lede.
4. **A target but no command, building something new** → run the full `craft` flow (§0 → §8). The default for "build me a landing page / dashboard".
5. **`audit` is read-only.** It only reports findings; it never edits code. Every other command is allowed to modify the target.

> Auto-trigger is unchanged: finesse still activates from natural language via its `description`. Commands are an **added** precision entry-point (`/finesse quieter page.html`), not a replacement — both routes lead to the same references.

After any command that modified the page, run the relevant **§8 Pre-Flight** gates before declaring done.

---

## 0. BRAND READ (Before Anything Else)

Most AI design output is bad because the model jumps to a default aesthetic instead of reading the brief. Don't.

### 0.A Determine the Register (this forks every later decision)

- **brand** — design IS the product: landing page, brand site, launch, portfolio, campaign, hero page. Be bold, opinionated, spectacular. Goes the soul + hero-engine route (§2, §4).
- **product** — design SERVES the product: dashboard, admin, analytics, data table, app shell, settings, tool. Optimize for clarity, density, usability. Goes the component-system route (`references/product-ui.md`). Still never cheap — it inherits the substrate (§3) and the cheapness blacklist (§6).

**Read project memory first.** If a `PRODUCT.md` exists at the project root, read it (register, users, brand personality, locked dials, anti-references) — it **overrides your guesses**. If a `design-model.yaml` exists, read it too for the locked palette/type/substrate so this page matches existing ones. These are written by `init` / `document` (see Commands).

- **No `PRODUCT.md`, multi-page or repeat project, thin brief** → offer to run `init` first (one `PRODUCT.md` keeps every later page consistent). Don't force it on a one-off page.
- **Existing codebase, no `design-model.yaml`** → offer `document` to capture what's there before adding to it.
- If memory exists but the new request contradicts it, surface the conflict — don't silently override the lock.

### 0.B Output a one-line "Design Read" before generating

Format: `Design Read: {industry} · {soul in 2-3 words} · register={brand|product} · SPECTACLE={n} · hero-engine={type}`

Example: `Design Read: deep-space astronomy · cinematic + reverent · register=brand · SPECTACLE=8 · hero-engine=Three.js particle galaxy`

**STOP after the Design Read. Do not generate any code yet.** Wait for the user to confirm the direction or redirect. Only proceed to §1 once the user says "go ahead", "looks good", "yes", or provides additional guidance.

### 0.C If the brief is ambiguous, ask ONE question — do not guess blind

One sharp question beats five rounds of wrong defaults. Ask the thing that most changes the output: *"Is this meant to feel restrained-editorial or maximal-spectacle?"* / *"What should a visitor remember 10 seconds after leaving?"* Then commit. **Wait for the answer before proceeding.**

### 0.D Anti-Default Discipline

Name the lazy default for this brief, then beat it. "Coffee brand → the default is warm-beige + brass serif. I'm rejecting that for {x}." The single most-tested AI tell is reaching for the obvious aesthetic of the category. (Reflex-reject lists live in `references/anti-cheap.md`.)

### 0.E Quick-Start Dial Mapping

If the brief contains these cues, use these presets as a starting point before refining in §1:

| User says | SOUL | SPECTACLE | DENSITY |
|-----------|------|-----------|---------|
| "premium", "luxury", "high-end" | 8 | 5 | 3 |
| "minimal", "clean", "understated" | 6 | 3 | 3 |
| "bold", "striking", "impactful" | 7 | 7 | 4 |
| "editorial", "magazine", "publication" | 8 | 4 | 6 |
| "tech", "AI", "SaaS" marketing | 6 | 7 | 5 |
| "corporate", "B2B", "enterprise" | 4 | 3 | 6 |
| "playful", "vibrant", "creative" | 7 | 6 | 5 |
| "data-heavy", "dashboard", "analytics" | 4 | 2 | 9 |
| "landing page" (no other cues) | 7 | 6 | 4 |
| "portfolio" | 8 | 6 | 3 |

Override these immediately if the brief provides stronger or contradicting signals.

---

## 1. THE THREE DIALS

Set these explicitly from the Design Read. They drive everything downstream.

| Dial | 1–3 | 4–6 | 7–10 |
|------|-----|-----|------|
| **SOUL** — how opinionated / branded the personality is | neutral, safe, system-default | a clear vibe | unmistakable, one-of-a-kind identity |
| **SPECTACLE** — how technically-ambitious the visual engine is *(finesse's signature dial)* | static + CSS only | GSAP scroll, Canvas 2D accents | Three.js / GLSL / WebGL-FBO hero, generative, scroll-pinned cinema |
| **DENSITY** — information per viewport | airy, one idea per screen | balanced | editorial, data-rich |

### 1.A Dial inference (Design Read → values)

- Astronomy / music / game / crypto / fashion-tech → **SPECTACLE 7–10** (the genre rewards a real engine).
- Law / finance / healthcare / B2B SaaS marketing → **SPECTACLE 3–5** (craft over fireworks; one restrained motion moment).
- Heritage / luxury / editorial / publication → **SOUL 8–10, SPECTACLE 4–6** (the type and substrate carry it, not WebGL).
- **Any product register** (dashboard / admin / analytics / app) → **SPECTACLE 1–4, DENSITY 6–9** — clarity beats fireworks. Skip §2/§4 and go to `references/product-ui.md`.

### 1.B "Spectacle claimed, spectacle shown" (mandatory)

If `SPECTACLE ≥ 7`, the page MUST actually contain a working visual engine (a real Three.js/Canvas/GLSL/scroll-pinned moment), degrade gracefully, and hold 60fps on a mid-range device. A page that claims SPECTACLE 8 but ships a gradient blob is **broken**. If you cannot ship working spectacle in scope, drop the dial to 4 and ship an impeccably-crafted static page instead. Never half-build an engine that janks or cuts off.

---

## 2. PICK A SOUL (Industry → Persona) · brand register

> **Product register:** soul still matters (brand accent, one type system, the substrate), but skip the spectacle personas below — go to `references/product-ui.md`. The rest of §2 and §4 are for **brand**.

finesse's job is **soul diversity**: the same method must yield visually unrelated pages for different briefs. Reach into `references/style-personas.md` for the industry→persona map (palette family, type pairing, hero-engine fit, signature effect). Examples of the *range* you must be able to hit:

- **Cinematic tech** (cyan/magenta, Inter + JetBrains Mono, Three.js particles) — astronomy, AI, crypto.
- **Phosphor terminal** (single neon-green, mono-forward, Canvas data viz) — quant/fintech, security.
- **Editorial publication** (cream/ink, Playfair + Spectral, GSAP scroll-reveal, grayscale photography) — magazines, film, journals.
- **Warm heritage** (amber/copper/ember, Fraunces/EB Garamond, Canvas fire/particles) — whisky, coffee, craft.
- **Brutal typographic** (bone/black + one hot accent, Anton/Bebas, mix-blend-mode) — fashion week, music, culture.
- **Quiet luxury minimal** (off-white/forest, Raleway 100–900, CSS-only mask/parallax) — architecture, hotels, fragrance.

Rules:
- **One soul per page.** Don't fluctuate warm and cool greys, or swap accent colors mid-scroll. Lock it (see §3, color lock).
- **Rotate, don't repeat.** If the last brief used editorial-serif, this one must not. Saturated aesthetic lanes (editorial-typographic, beige-brass craft, AI-purple-glow) are banned as *defaults* — earn them or avoid them (`references/anti-cheap.md`).

> Once a persona is picked, `references/inspiration-catalog.md` has a wider bench of real pages per persona (48 beyond the 5 in `examples/`) — technique notes, not files, for when you want a second reference point beyond the persona table's single description.

---

## 3. THE PREMIUM SUBSTRATE (Why It Reads as Expensive)

The difference between a cheap page and an expensive one is mostly a thin physical layer, applied consistently. Full recipes and exact values in `references/design-dna.md`. The non-negotiables:

- **Grain** — a fixed SVG `feTurbulence` noise layer at `opacity .025–.05`. Static, but kills the flat "vector slop" look. (Light pages too, lower opacity.)
- **Vignette** — a radial-gradient darken on dark heroes to create an optical focal point.
- **Type tension** — display headings at `clamp()` with **negative tracking** (`-.02 to -.045em`) and `line-height .86–.95`; extreme weight contrast against a light body (e.g. 900 against 300). Tight, large, confident.
- **Layered z-index** — engine(0) · grain(1) · vignette · content(5). Depth, not flatness.
- **Translucent borders** — `rgba(255,255,255,.07–.22)` on dark, `rgba(0,0,0,.06–.08)` on light. Never a hard `#333` line.
- **No pure `#fff` / `#000`.** Tint every neutral a few points toward the brand hue.
- **Color lock (mandatory):** once an accent is chosen, it owns the whole page. No surprise teal badge on a rose page. Audit every component before shipping.

> Internally reason in **OKLCH** for palettes (perceptual consistency, easy light/dark pairing), even if you emit hex. Design light and dark together; test contrast in each — never just invert.

---

## 4. THE HERO ENGINE (finesse's Differentiator) · brand register

> **Product register:** no hero engine — reach for a data-viz + component system instead (`references/product-ui.md`). This section is for **brand**.

A finesse page earns its name with **one** technically-spectacular moment — usually the hero. Not five. One, done at 100%. Pick the engine that fits the soul; full mount/render/scroll skeletons + reduced-motion fallbacks live in `references/hero-engines.md`.

| Engine | Use when | Cost |
|--------|----------|------|
| **Three.js + GLSL** | 3D depth, particle systems (galaxies, networks, DNA), metaballs, bloom | heavy; lazy-load `three`, gate on SPECTACLE ≥ 7 |
| **Canvas 2D** | particles, fields, real-time data (K-lines, waveforms, fire), flow | light; DPR-adapt for retina |
| **WebGL FBO shader** | fluid (Navier-Stokes), reaction-diffusion, ray-marching, iridescence | heavy; one fullscreen quad, multi-pass |
| **GSAP ScrollTrigger** | scroll-pinned story, horizontal pan, parallax, reveal stagger | medium; the single most reusable engine |
| **CSS-only** | dual-layer mask, 3D transforms, variable-font morph, scroll-driven `animation-timeline` | free; no JS, best perf |

> **Component-level 3D ≠ hero engine.** The table above is for the one full-bleed hero moment. For *reusable, in-page* 3D — pointer-tilt cards, flip cards, coverflow, depth-parallax layers, or a Three.js product/model viewer — reach for `references/3d-effects.md` (the `depth` command). Default to its CSS tier; it ships in any page at zero cost and rarely janks. One 3D moment per page still applies: don't stack a hero engine *and* a tilt grid *and* a coverflow.

**Engine discipline (mandatory):**
- **Progressive enhancement.** The page must be readable and complete with the engine removed. The engine is a fixed background or a hero accent, never load-bearing for content.
- **60fps or simplify.** Animate only `transform` / `opacity`. Test on a mid-range device, not your machine. Below ~50fps, cut particle count or resolution.
- **`prefers-reduced-motion` is mandatory** — freeze the engine to a still frame (or hide it and show a composed static hero). Never ship motion with no fallback.
- **Motivated motion only.** Every ScrollTrigger / marquee / pinned section needs a one-sentence reason (hierarchy, storytelling, feedback, state). "It looked cool" is not a reason. Max **one** marquee per page.

---

## 5. PAGE SKELETON

Canonical section sequence (adapt to the soul, never ship all of it by rote):

```
HERO (100vh, the engine moment)  →  MARQUEE/TICKER (≤1 per page)  →
STATEMENT / MANIFESTO (word-reveal)  →  CORE CONTENT (specs grid · horizontal-pan · collection)  →
INDUSTRY SECTION (process · parallax imagery · pull-quote)  →  CTA / FINALE (oversized type)  →  FOOTER (mono, hairline top border)
```

- **Nav:** single line, ≤80px tall, `mix-blend-mode: difference` works beautifully over imagery. Backdrop-blur on scroll.
- **Layout diversification:** once a layout family is used (3-col cards, full-width quote, split image+text), it appears **at most once more**. Max 2 consecutive image+text zigzags. A page with 8 sections uses ≥4 layout families.
- **Eyebrow restraint:** the tiny-uppercase-tracked label above every headline is the #1 AI tell. Max **1 eyebrow per 3 sections**. Usually the headline alone is enough.
- **Theme lock:** one theme for the whole page. No warm-paper section dropped into a dark page (unless a deliberate one-time scroll theme-switch).

---

## 6. THE CHEAPNESS BLACKLIST

Before declaring done, scan against `references/anti-cheap.md` — the merged anti-slop list (AI tells + absolute bans + reflex-reject fonts/palettes/aesthetics). The headline offenders:

- **em-dashes** in copy as a flourish — banned outright (the single most-violated tell).
- **gradient text**, default **glassmorphism**, side-stripe card borders, **AI-purple glow**.
- **eyebrow on every section**, numbered `01 · 02 · 03` markers as default architecture.
- **identical card grids** (icon + title + text × 6), div-based fake screenshots / fake dashboards.
- **fake-precise numbers** (`92%`, `4.1×`) with no real source.
- **default-category palette** (beige+brass for craft, purple-glow for AI/SaaS) — name it, reject it.
- **`Inter`/`Fraunces`/`Instrument Serif` as unexamined defaults** — fine if chosen with a reason, a tell if reached for blindly.
- **zero imagery** on an image-implied brief (food, hotel, fashion, travel) — that's a bug, not minimalism.

---

## 7. PERFORMANCE & ACCESSIBILITY GUARDRAILS

- Animate `transform`/`opacity` only; never `top/left/width/height`. `will-change` sparingly.
- `prefers-reduced-motion`: stop canvas loops, freeze grain, swap to static. Mandatory on every animated page.
- Color contrast WCAG AA: body ≥ 4.5:1, large text ≥ 3:1. Includes buttons over photos (add scrim/stroke), placeholders, focus rings.
- Visible focus states on every interactive element. Nav and CTAs reachable by keyboard.
- Core Web Vitals: lazy-load heavy engines, `min-h-dvh` over `100vh` on mobile, responsive images (WebP/AVIF), CLS < 0.1.
- Mobile collapse declared explicitly per multi-column section. Touch targets ≥ 44px.

---

## 8. PRE-FLIGHT CHECK

Run the full checklist in `references/preflight.md` before saying "done." It merges the substrate check, the cheapness scan, the spectacle-claimed verification, and the a11y gates. If any hard rule fails, it is shipping broken work — fix before delivery.

---

## 8.A POST-DELIVERY ITERATION GUIDE

After the user receives the initial output, map their feedback to the correct targeted fix. **Never rebuild from scratch for a single complaint** — identify the dial or module responsible and adjust only that. The **Command** column is the verb to route through (see `## Commands`); if the user typed the command, you're already there.

| User says | Command | Action |
|-----------|---------|--------|
| "too plain / boring" | `bolder` | Raise SPECTACLE +2; consider upgrading the engine type (e.g. Canvas → Three.js) |
| "too flashy / overwhelming" | `quieter` | Lower SPECTACLE −2; simplify or swap to Engine D (GSAP) or E (CSS-only) |
| "wrong vibe / feels off" | `soul` | Re-run §2 with a different persona from `references/style-personas.md` |
| "too much whitespace" | `densify` | Raise DENSITY +2; add one content section |
| "too cluttered" | `densify` | Lower DENSITY −2; cut a section, increase section padding |
| "more personality / bolder" | `bolder` | Raise SOUL +2; push color commitment level up one step in `references/design-dna.md` |
| "feels generic / like every other AI site" | `soul` | Trigger §0.D anti-default: name and reject the current soul, pick a non-obvious persona |
| "change the colors" | `soul` | Re-run color strategy in `references/design-dna.md`; maintain the accent lock rule |
| "different animation" | `animate` | Swap engine type in §4; re-run `references/hero-engines.md` for that engine's skeleton |
| "add depth / make it 3D / tilt / parallax" | `depth` | Add **one** 3D moment from `references/3d-effects.md` — default to the CSS tier (tilt/flip/coverflow/depth-parallax); Three.js only for a real rendered object |
| "remove a section" | `redesign` | Remove it, then re-audit §5 layout families (ensure ≥4 families remain) |
| "feels slow / heavy" | `quieter` | Lower SPECTACLE; switch to Engine E (CSS-only) or reduce particle count/FBO resolution |
| "needs to work on mobile" | `redesign` | Declare mobile layout per multi-column section; `min-h-dvh`, touch targets ≥44px |
| "is this any good? / review it" | `audit` | Read-only: run the blacklist + spectacle-shown + pre-flight, report findings |

---

## 9. OUT OF SCOPE

finesse covers **both** brand and product UI, so its scope is wide. Hand off only when the work is a **pure backend / API / data task with no interface**, or a brief that explicitly wants a **generic, conventional, zero-craft page** (finesse always brings craft — if the user truly wants bland, that's a different tool). Everything from a spectacle landing page to a dense admin dashboard is in scope: set the register in §0 and route accordingly.
