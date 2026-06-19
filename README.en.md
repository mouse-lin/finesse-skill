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

## What's inside

- **Premium physical substrate** — SVG grain / vignette / type weight tension / color family (distilled from 37 pages of industry showcase analysis)
- **Five hero engine types** — Three.js / Canvas / WebGL-FBO / GSAP / CSS-only, each with a `prefers-reduced-motion` fallback skeleton
- **Industry soul decision matrix** — industry → style / palette / typeface / engine selection table; cures "every page looks the same"
- **Product UI route** — dashboard information architecture · 25 chart types · data tables · forms · interaction state inventory · component system · cognitive load
- **Register-driven routing** — brand vs product split into two completely different rule sets
- **Anti-slop blacklist** — production-verified AI tells + absolute bans + reflex-reject font/palette/aesthetic list
- **Design-model token lock** — multi-page consistency + generate → self-audit → iterate loop
- **OKLCH color strategy ladder** — four commitment levels: restrained / committed / full / drenched

## Installation & Tool Support

### Claude Code (native)

```bash
npx skills add https://github.com/<your-username>/finesse-skill
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
│       │   ├── hero-engines.md           # Five engine skeletons + reduced-motion fallbacks
│       │   ├── style-personas.md         # Industry → soul decision matrix
│       │   ├── anti-cheap.md             # Anti-slop blacklist (AI tells + bans + reflex-reject)
│       │   ├── product-ui.md             # Product route: dashboards/tables/charts/forms/states
│       │   ├── preflight.md              # Pre-flight checklist
│       │   └── design-model.md           # Multi-page token consistency template
│       └── examples/
│           ├── EXAMPLES.md               # 5 showcase pages with persona/engine annotations
│           └── *.html                    # Real showcase pages (read-only reference)
├── .claude-plugin/plugin.json            # Claude Code native plugin
├── .cursor/rules/finesse-ui.mdc          # Cursor rules (auto-loaded on UI files)
├── AGENTS.md                             # OpenAI Codex instructions
├── .github/copilot-instructions.md       # GitHub Copilot instructions
├── README.md                             # Chinese README
├── README.en.md                          # This file
└── LICENSE
```

## Out of scope

finesse covers both brand and product UI, so its range is wide. The only cases that don't fit: **pure backend / API / data tasks with no interface**, or a brief that explicitly requires a **generic, zero-craft page** (finesse always brings craft — if you genuinely want bland, that's a different tool). Everything from a spectacle landing page to a dense admin dashboard is in scope: set the register in §0 and route accordingly.

## License

MIT
