# Copilot Instructions: finesse Design Standard

> GitHub Copilot reads this file automatically. These rules replace generic AI design defaults with finesse's never-cheap, high-craft standards.

## Core Directive

You are generating UI for a finesse project. finesse builds two kinds of interface:
- **brand** (landing pages, hero sites, portfolios) — spectacle + soul + first impression
- **product** (dashboards, analytics, admin) — clarity + density + usability

Both are **never cheap**. The premium substrate and anti-slop rules apply to both.

## The 8 Non-Negotiables

1. **Read the brief first.** Before any code, output one line: `Design Read: {industry} · {soul} · register={brand|product} · SPECTACLE={n}`. Name the lazy default aesthetic and state how you are beating it.

2. **Premium substrate on every page.** SVG grain layer (`feTurbulence`, `opacity .025–.05`). Radial vignette on dark heroes. Display headings with negative tracking (`-.02 to -.045em`) and `line-height .86–.95`. Translucent borders, never `rgba(0,0,0,1)` lines. No pure `#fff`/`#000`.

3. **SPECTACLE claimed = SPECTACLE shipped.** If you say Three.js, ship Three.js. If the engine is out of scope, drop to 4 and ship a polished static page. Never half-build something that janks.

4. **Em-dash ban. No exceptions.** The single most-violated AI tell. Replace with a comma, period, or restructure the sentence.

5. **No eyebrow on every section.** The tiny uppercase tracked label above every headline is the #1 layout tell. Max 1 eyebrow per 3 sections. The headline usually suffices alone.

6. **Color lock.** One accent owns the whole page. No surprise teal badge on a rose page. Audit every component before shipping.

7. **`prefers-reduced-motion` is mandatory.** Freeze canvas loops, freeze grain animation, provide a static fallback. Never ship motion with no escape hatch.

8. **Complete implementation only.** No `// TODO: implement`, no placeholder text, no stubbed components. Write the full, working code every time.

## Hard Bans

- Gradient text, default glassmorphism, AI-purple glow, side-stripe card borders
- Numbered `01 · 02 · 03` section markers as default architecture
- Identical 6-card grids (icon + title + description × 6)
- Fake-precise numbers (`92%`, `4.1×`) without a real source
- Default-category palettes (beige+brass for craft, purple-glow for AI/SaaS) reached for without reason

## Reference Material

The full skill lives in `skills/finesse-ui/SKILL.md`. Deep material in `skills/finesse-ui/references/`. Load what you need per phase.
