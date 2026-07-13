[中文](README.md) | [English](README.en.md)

![finesse-skill cover](assets/cover.jpg)

# finesse-skill

> **Never-cheap, high-craft web interfaces — brand spectacle + product precision**
> A design skill for AI coding assistants that builds interfaces that never look templated.

`finesse` is a design skill for AI coding assistants (Claude Code, Codex, Cursor, Copilot, and more). It routes by **register**:

- **brand** — design IS the product: landing pages, brand sites, launches, portfolios, hero pages. Optimize for **spectacle + soul + first impression** with a real visual engine (Three.js / GLSL / Canvas / WebGL / GSAP).
- **product** — design SERVES the product: dashboards, admin panels, analytics, data tables, app shells, settings. Optimize for **clarity + density + usability** with a component system and data visualization.

Both paths share the same foundation: **premium physical substrate + anti-slop audit**. Dashboards are not allowed to look cheap either.

## How it works

1. **Read the brief** (register: brand vs product) → output one-line Design Read to commit direction.
2. **Set three dials**: SOUL · SPECTACLE (the signature dial) · DENSITY. Product register pins SPECTACLE low, DENSITY high.
3. **Both paths**: lay the premium substrate (grain · vignette · type tension · translucent borders · OKLCH color lock).
4. **Fork**:
   - **brand** → pick a soul (industry → style persona) + build one hero engine (five types, done at 100%).
   - **product** → component system + data viz (information architecture · tables · charts · forms · interaction states).
5. **Anti-slop blacklist + pre-flight check**, then ship.

> First time here? See **[USAGE.en.md](USAGE.en.md)** — a checklist for getting good results with zero design experience.

## What's inside

- **Premium physical substrate** — SVG grain / vignette / type weight tension / color family (distilled from 53 pages of industry showcase analysis)
- **Five hero engine types** — Three.js / Canvas / WebGL-FBO / GSAP / CSS-only, each with a `prefers-reduced-motion` fallback skeleton
- **3D effects spectrum** — CSS pseudo-3D (pointer-tilt cards / flip cards / coverflow / depth-parallax) + Three.js real 3D (model viewer / image-displacement plane); the `depth` command drops in one 3D moment
- **Industry soul decision matrix** — industry → style / palette / typeface / engine selection table; cures "every page looks the same"
- **Product UI route** — dashboard information architecture · 25 chart types · data tables · forms · interaction state inventory · component system · cognitive load
- **Register-driven routing** — brand vs product split into two completely different rule sets
- **Anti-slop blacklist** — production-verified AI tells + absolute bans + reflex-reject font/palette/aesthetic list
- **Design-model token lock** — multi-page consistency + generate → self-audit → iterate loop
- **OKLCH color strategy ladder** — four commitment levels: restrained / committed / full / drenched

## Examples

Pages built from the same design DNA — brand path (visual engines · soul-driven) across multiple industries:

| | |
|:---:|:---:|
| ![Nexus — Three.js particle orbital rings, distributed intelligence](assets/examples/nexus.png) | ![Drift — Canvas 2D flow field particles, brand landing page](assets/examples/drift.png) |
| ![Forge — Three.js particle fire, game studio](assets/examples/forge.png) | ![Volt — all-electric car, bold typographic hero](assets/examples/volt.png) |
| ![Morning Ritual — specialty coffee, editorial light theme](assets/examples/coffee.png) | ![Eclipse — data gravity platform, bold typographic](assets/examples/eclipse.png) |

Product register (component system · data viz · information architecture):

| | |
|:---:|:---:|
| ![Buildly — AI growth intelligence, canonical sidebar shell + area-chart draw-in + donut](assets/examples/buildly.jpg) | ![Pulsegrid — infrastructure monitoring, glowing sparklines + premium custom slider](assets/examples/stakent.jpg) |
| ![ACRU — team productivity, canonical sidebar shell + hover-tooltip bar chart](assets/examples/acru.jpg) | ![PawCare+ — pet health companion, real-photo hotspot annotations + theme switcher](assets/examples/pawcare.jpg) |
| ![Nodeflux — API console, floating panel + true bento (2.15fr 1.7fr 1fr…) + concentric usage rings](assets/examples/nodeflux.jpg) | ![Inkline — publishing CMS, top-nav triptych + browser-preview hotspot annotations](assets/examples/inkline.jpg) |
| ![Huddle — team workspace, top-nav bento + colored task cards + multi-arc donut + voice waveform](assets/examples/huddle.jpg) | ![Threadline — support operations, floating panel + asymmetric bento + circular gauge + allocation table](assets/examples/ledgerio.jpg) |

## Installation & Tool Support

### npx skills (recommended · one command for Claude Code / Codex / Cursor, etc.)

[`npx skills`](https://github.com/vercel-labs/skills) is Vercel Labs' open-source cross-tool skill installer; it asks which agent to install into at runtime.

```bash
# Install all skills
npx skills add https://github.com/mouse-lin/finesse-skill

# Install a single skill
npx skills add https://github.com/mouse-lin/finesse-skill --skill "finesse-ui"
```

After installation, say "use finesse to build a …" or `/finesse` in the conversation to trigger the skill.

Alternatively, copy `skills/finesse-ui/` manually into your `.claude/skills/` (or `.agents/skills/`, `~/.claude/skills/`).

### Cursor

Copy `.cursor/rules/finesse-ui.mdc` into your project's `.cursor/rules/` directory:

```bash
cp .cursor/rules/finesse-ui.mdc your-project/.cursor/rules/
```

Cursor automatically loads this rule when you are editing HTML / CSS / JS / TS / Vue / Svelte files — no manual trigger needed.

### OpenAI Codex

Copy `AGENTS.md` into your project root. Codex reads it automatically and follows the finesse workflow:

```bash
cp AGENTS.md your-project/AGENTS.md
```

Or paste the contents of `skills/finesse-ui/SKILL.md` directly into a Codex conversation.

### GitHub Copilot

Copy `.github/copilot-instructions.md` into your project's `.github/` directory:

```bash
cp .github/copilot-instructions.md your-project/.github/
```

Copilot reads it automatically and generates code to finesse standards.

### Trae

Copy the full `.trae/skills/finesse-ui/` directory (SKILL.md + references + examples + scripts) into your project:

```bash
cp -r .trae/skills/finesse-ui your-project/.trae/skills/
# Trae CN uses .trae-cn, same full directory:
cp -r .trae-cn/skills/finesse-ui your-project/.trae-cn/skills/
```

This is a full mirror, not a condensed rule — Trae's Skills folder convention (`.trae/skills/<name>/SKILL.md`) matches Claude Code's natively and can load every reference file on demand, so copying the whole directory gets you the same depth as Claude Code. **Maintenance note:** any change to `skills/finesse-ui/` must be copied into `.trae/skills/finesse-ui/` and `.trae-cn/skills/finesse-ui/` by hand — these are independent static copies, not symlinks, and won't update themselves.

### CodeBuddy

Copy the full `.codebuddy/skills/finesse-ui/` directory (SKILL.md + references + examples + scripts) into your project:

```bash
cp -r .codebuddy/skills/finesse-ui your-project/.codebuddy/skills/
```

Also a full mirror — CodeBuddy's Skills folder convention (`.codebuddy/skills/<name>/SKILL.md`) matches Claude Code's natively. **Maintenance note:** any change to `skills/finesse-ui/` must be copied into `.codebuddy/skills/finesse-ui/` by hand — this is an independent static copy, not a symlink, and won't update itself.

### Any other tool (ChatGPT / direct API / etc.)

Paste the contents of `skills/finesse-ui/SKILL.md` into the system prompt or the first user message. No installation required.

---

## Repository structure

```
finesse-skill/
├── skills/
│   └── finesse-ui/
│       ├── SKILL.md                      # Main entry: methodology + workflow
│       ├── references/
│       │   ├── design-dna.md             # Premium substrate (palette/type/grain/vignette)
│       │   ├── hero-engines.md           # Five engine skeletons + reduced-motion fallbacks + inline-image technique
│       │   ├── 3d-effects.md             # 3D spectrum: CSS pseudo-3D (tilt/flip/coverflow/parallax) + Three.js real 3D
│       │   ├── style-personas.md         # Industry → soul decision matrix
│       │   ├── anti-cheap.md             # Anti-slop blacklist (AI tells + bans + reflex-reject)
│       │   ├── product-ui.md             # Product route: dashboards/tables/charts/forms/states
│       │   ├── dataviz.md                # Chart decision matrix: 25 types × a11y grade × library picks
│       │   ├── commerce-ui.md            # Commerce route: PDP/PLP/cart/checkout + dark-pattern blacklist
│       │   ├── asset-sourcing.md         # No assets on hand: generate vs. real stock vs. placeholder + authorization gate
│       │   ├── preflight.md              # Pre-flight checklist (includes strategic omissions)
│       │   ├── redesign-mode.md          # Redesign protocol: audit-first, six-step upgrade
│       │   ├── design-model.md           # Multi-page token consistency template
│       │   └── inspiration-catalog.md    # Technique index for 48 more pages from the 53-page corpus (grouped by persona, no source files)
│       └── examples/
│           ├── EXAMPLES.md               # 5 showcase pages with persona/engine annotations
│           └── *.html                    # Real showcase pages (read-only reference)
├── .claude-plugin/plugin.json            # Claude Code native plugin
├── .cursor/rules/finesse-ui.mdc          # Cursor rules (condensed single file, auto-loaded)
├── AGENTS.md                             # OpenAI Codex instructions
├── .github/copilot-instructions.md       # GitHub Copilot instructions (condensed single file)
├── .trae/skills/finesse-ui/              # Trae (full mirror, same depth as skills/finesse-ui, synced by hand)
├── .trae-cn/skills/finesse-ui/           # Trae CN (same, full mirror)
├── README.md                             # Chinese README
├── README.en.md                          # This file
└── LICENSE
```

## Out of scope

finesse covers both brand and product UI, so its range is wide. The only cases that don't fit: **pure backend / API / data tasks with no interface**, or a brief that explicitly requires a **generic, zero-craft page** (finesse always brings craft — if you genuinely want bland, that's a different tool). Everything from a spectacle landing page to a dense admin dashboard is in scope: set the register in §0 and route accordingly.

## License

MIT
