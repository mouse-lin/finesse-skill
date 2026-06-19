# Design Model — Lock It So Multiple Pages Agree

When a brief needs **more than one page** (or a multi-section site that must stay coherent), freeze the decisions into a small machine-readable model *before* writing markup. Two sessions reading the same model should produce visually consistent output.

> Single one-off page? Skip this — go straight from the Design Read to code. This file earns its keep on multi-page / multi-session work and when iterating.

---

## When to write a model

- The project is a **multi-page** site, or a brand that will get repeat work.
- You want **reproducibility** — coming back next week and matching exactly.
- You're **remixing** an existing finesse page and want a diffable record of the soul.

Write it to `design-model.yaml` next to the page(s). Emit hex for direct use, but choose values reasoning in OKLCH.

---

## Template

```yaml
# design-model.yaml — the locked soul of this project
meta:
  name: aether
  industry: spatial-intelligence / deep-tech
  register: brand                 # brand = design IS the product
  persona: Cinematic Tech         # from style-personas.md
  design_read: "deep-tech · cinematic + reverent · SPECTACLE=8 · Three.js particles"

dials:
  soul: 8
  spectacle: 8
  density: 4

# ── PRIMITIVES ── raw values, reasoned in OKLCH
color:
  bg:      "#04060D"
  surface: "#0F0F16"
  ink:     "#ECECF2"
  muted:   "#9A9AAB"
  dim:     "#6C6C7C"
  accent:  "#22E3FF"              # THE color — owns the whole site
  accent2: "#FF2D8E"              # only if duotone persona
  border:  "rgba(255,255,255,0.07)"
  strategy: committed             # restrained | committed | full | drenched

type:
  display: "Inter"                # justify if it's a reflex-default
  display_weight: 800
  body: "Inter"
  body_weight: 300
  mono: "JetBrains Mono"
  scale_ratio: 1.28               # >= 1.25
  display_tracking: "-0.045em"
  display_leading: 0.9

substrate:
  grain_opacity: 0.032
  grain_base_freq: 0.75
  vignette: true
  radius_scale: 14px              # one radius, locked
  motion_easing: "cubic-bezier(.16,1,.3,1)"

hero_engine:
  type: three-particles           # A=three | B=canvas | C=fbo | D=gsap | E=css
  detail: "22k-point galaxy, additive blend, scroll-coupled camera z"
  reduced_motion: "freeze to still frame at 55% opacity"

structure:
  sections: [hero, marquee, manifesto, capabilities, research, cta, footer]
  layout_families: [pinned-hero, horizontal-pan, split, full-quote, grid]
  nav: "mix-blend-difference, single line, backdrop-blur on scroll"
```

---

## The generate → audit → iterate loop

1. **Build the model** (above) — commit the soul on paper first.
2. **Generate** the page(s) from the model. No improvising new colors/fonts mid-build; if you need one, update the model first.
3. **Self-audit** against `preflight.md` and `anti-cheap.md`. Record fails.
4. **Iterate** — fix fails, bump a dial if the soul is too timid or too loud, regenerate the affected sections. Re-audit.
5. **Lock** — the final `design-model.yaml` is the source of truth. Any later page for this brand reads it and matches.

This is what keeps a 5-page site from drifting into 5 different souls — and what lets you (or another session) reproduce the look exactly later.

---

## Multi-page: MASTER + PAGE OVERRIDES pattern

For projects with 3+ pages, use a two-level hierarchy instead of a single flat file:

```
design-model/
├── MASTER.yaml          ← global truth: brand soul, palette, type, substrate
└── pages/
    ├── landing.yaml     ← hero-specific overrides
    ├── product.yaml     ← product-register overrides (SPECTACLE lower, DENSITY higher)
    ├── blog.yaml        ← editorial overrides (column width, reading line-length)
    └── about.yaml       ← minimal overrides (usually just section sequence)
```

**MASTER.yaml** locks what must never drift across pages:
- `color.accent` — the one brand accent, unchanged everywhere
- `color.bg`, `color.ink`, `color.border` — the neutral family
- `type.display`, `type.body`, `type.mono` — the font trio
- `substrate` — grain opacity, radius scale, motion easing
- `dials.soul` — the brand personality level

**Page override files** only specify what changes for that page:

```yaml
# pages/product.yaml — overrides for the dashboard/app section
extends: MASTER
dials:
  spectacle: 2          # override: product register, no hero engine
  density: 8            # override: data-rich layout
hero_engine: none
structure:
  shell: sidebar-topbar
  sections: [sidebar, topbar, main-content, settings-drawer]
```

**Rules:**
- Any value not in the override file inherits from MASTER — never duplicate what hasn't changed.
- Page overrides cannot change `color.accent` or `type.display` — soul consistency is non-negotiable.
- Add a new page file before starting any new page. Do not improvise mid-build.
