# Product UI — Dashboards, Admin, Data Apps

The **product** register: design SERVES the product. Dashboards, admin panels, analytics, data tables, app shells, settings. Optimize for **clarity · density · usability** — fast task completion, no errors. Spectacle is not the goal here; **craft still is.**

> A dashboard is a **different design language from a brand page**, not a brand page with charts. It inherits only the **universal craft floor** from `design-dna.md` (tinted neutrals, no pure `#fff`/`#000`, translucent/hairline borders, tinted shadows, contrast floors, OKLCH, dark-mode parity) — and it does **NOT** inherit the brand-page moves: **no grain, no vignette, no `clamp()` hero type, no dark-by-default, no hero engine.** The premium here comes from §0's product substrate, not from brand fireworks. SPECTACLE dial = 1–4; DENSITY = 6–9; motion is feedback only. Still bound by the **cheapness blacklist** (`anti-cheap.md`).

---

## 0. The Product Substrate — what actually makes a dashboard look premium

Distilled from 10 shipped showcase dashboards. This is the layer that separates them from the default **flat white card + hard grey border + generic blue/green** admin look. Lay this *before* filling in tables/charts/forms.

> **Read the shipped code first — non-optional; skipping it is how dashboards come out generic.** These examples ARE our own showcase-gallery dashboards (`showcase/gallery.html`) bundled into `../examples/`. Open the closest one, study how the substrate / cards / KPI tiles / charts are actually built, then **lift patterns, not whole files** (each brief still gets its own accent + soul):
> - `acru-financial-dashboard.html` — **light**, canonical sidebar; stacked-bar + hover tooltip, half-ring gauge, credit-card widget.
> - `buildly-growth-dashboard.html` / `stakent-monitoring-dashboard.html` — **dark**, canonical sidebar; line/area draw-in, glowing sparklines, five KPI roll-up counters, slider tiers.
> - `pawcare-adaptive-health.html` — **floating rounded panel**; hotspot pins on a real photo, accent-picker + dark-mode toggle, hand-built ECG / ring marks.
>
> Full index + a per-file "what to study" table: `../examples/EXAMPLES.md`. **Match the example to the morphology you're building** (sidebar / floating-panel / bento / triptych — §1) and to light-vs-dark.
>
> **The corpus is all dashboards — there is no form-workflow page in it.** If you're building a console / wizard / settings page (the §1 workflow shell), this "read the closest example" step **does not apply**: no dashboard is close, and force-fitting one onto a form is worse than reading nothing. Build from `workflow-ui.md` instead. The rule is "study shipped code *of the thing you're building*", not "always open a file."

### 0.1 Surfaces & cards (the #1 premium signal in product UI)

- **The page is never pure `#fff`/`#000`** — it's an off-tint neutral, tinted toward the brand hue. Light dashboards sit on a tinted paper (`#eef0ec` sage · `#f3f2ef` ivory · `#eef1fb` lilac · `#e9e9ec` cool-grey · `#d8f1f6` aqua); dark ones on a tinted near-black (`#08080a` · `#0a0a10`). A flat `#ffffff` page background is the first tell of a cheap dashboard.
- **Card recipe:** `background:#fff` (light) or a tinted-dark panel (`#141417`/`#13131c`); `border-radius:16–22px` (mode ≈ 20); `padding:18–22px`; and **either** a hairline border `1px solid` at 3–8% alpha (`rgba(255,255,255,.07)` dark · `#e2e2e7`-class 4–6%-black light) **or no border at all**, letting a shadow do the separation. **Never a hard `1px solid #ccc/#ddd/#333` box** — that single line is what reads cheap.
- **Whisper shadow, not a hard edge.** The signature is a near-invisible ambient: `box-shadow: 0 1px 3px rgba(0,0,0,.03–.06)`, **tinted toward the ink** (`rgba(30,20,70,…)` on a violet page, `rgba(19,61,74,…)` on a teal page), never pure black. Big shadows are **reserved for floating wrappers and modals**, not sprinkled on every card.
- **Radius tiers (lock them):** card `--r: 16–22px` · small controls `--r-sm: 11–15px` · outer floating shell `--r-lg: 32px`.
- **The floating-panel move** (consoles/ops tools that want a product-y frame): the whole dashboard sits on one raised panel over a **darker** outer page — a two-level neutral pair (`--page` darker `#e9e6dd`/`#dee2d4`/`#e6e9df`, `--bg` lighter panel). `max-width:~1440–1480px; margin:26px auto; border-radius:32px; padding:26px 30px 38px; box-shadow:0 40px 90px -50px rgba(20,24,20,.35)` — the **negative spread** makes it tight and cinematic, not a puffy drop shadow. Add a faint brand-tinted corner glow (`radial-gradient(280px 200px at 90% -10%, rgba(accent,.18), transparent 60%)`).
- **Accent-glow shadows only where earned** — `0 8px 24px rgba(accent,.4)` belongs on the **primary CTA / key card**, never on every surface. **Focus rings** are soft tinted (`0 0 0 3px var(--accent-soft)`), not the browser default outline.

### 0.2 Color — a distinct branded palette, never the generic dashboard triad

> **→ `product-palettes.md` is the color layer for this whole register. Open it before writing any CSS.** It has what this section only *asserts*: 5 **tinted neutral ramps** (the ramp is ~80% of a dashboard's pixels — the single highest-leverage decision here), 16 accents with separate light/dark values and text-on-accent contrast worked out, 12 paste-ready complete sets, the two-tone light-content/dark-rail shell most admin back-offices actually use, and the known SaaS palettes (Linear · Stripe · Supabase · Grafana · Notion…) for when someone asks for "something like Linear". Banning a default without shipping alternatives just means you reach for the default anyway.

- **Pick ONE branded accent** (or a duotone pair) and lock it, exactly like the brand register — just restrained (SPECTACLE low). The reflex **`#3B82F6` blue + `#22C55E` green + `#F97316` orange** corporate triad is the dashboard equivalent of AI-purple: predictable from the word "dashboard" alone → banned as an identity (`anti-cheap.md` §Charts, §0 Two-Altitude Check). Every one of the 10 showcase dashboards uses a *distinct* pair, none the generic triad: **warm-orange · violet · lime-green · ink+amber · indigo+coral · teal+amber · sky-aqua · royal-purple+amber · mint+lavender · teal+coral.**
- **Tint the neutral ramp to the accent's hue** — sage greys under a green accent, warm stone under an orange one, lilac under a violet one. The *page itself* carries the brand, not just one button. A pure-grey `#f5f5f5 / #e5e5e5 / #737373` ramp is the flat, cheap default (§9).
- **Reinforce the accent beyond a button:** soft-tint chips (`--accent-soft` at 8–16% alpha) on icons/badges, and the corner glow on a floating panel.
- **Semantic up/down green-red are functional, kept separate** from the brand accent — don't let status colors become the page's identity. Watch the collision: a **green** accent fights `--up`, a **red/brick/coral** accent fights `--down`. Semantics are load-bearing — **move the accent, not the semantic** (`product-palettes.md` §7).
- `design-dna.md` §6 lists a few product rows (Trust SaaS, Analytics, Financial dark, Developer-tool, Clinical calm, Authority navy) — those are *brand-style* two-value entries (bg + accent) and four of the six are blue. Use `product-palettes.md` instead; it fills the full token contract.

### 0.3 KPI / stat tile — the anatomy (so a tile reads as designed, not a number in a box)

Top → bottom, consistent across all 10:
1. **Top row:** a tinted **icon chip** (`30–36px`, `border-radius:9–10px`, bg = `panel-2`/`accent-soft`) **+** a **delta/status chip** (`font-size:11–12px; font-weight:600–700`, green/red or a badge).
2. **Label:** `font-size:11.5–12.5px; color:var(--ink-3)`, optionally uppercase `letter-spacing:.03–.05em`.
3. **Big number:** `font-size:22–30px; font-weight:700–800`, **always** `font-variant-numeric:tabular-nums; letter-spacing:-.02em`, counting up from 0 on load.
4. **Optional:** unit in `<small>`, a "prev" subtext, or a **30–70px inline sparkline** (SVG polyline, stroke-dashoffset draw-on).

Don't ship a uniform `repeat(6,1fr)` row of identical tiles — vary the grid (see §1 bento) and mix tile types.

### 0.4 Motion — feedback, not spectacle (SPECTACLE 1–4)

Three moves cover a dashboard, all **above-the-fold on load** (no ScrollTrigger gating the first screen) and each **paired with a reduced-motion terminal state** (`chart-crafting.md` §6):
- **Reveal stagger** — `.reveal{opacity:0; transform:translateY(16px)}` → one `gsap.to('.reveal',{opacity:1,y:0,stagger:.06–.08})`, whose `onStart` kicks off the counters and chart draw-ins so they move *with* the cards.
- **Number roll-up** — counters tween 0 → target, formatted in `onUpdate`.
- **Chart draw-on** — line stroke-dashoffset, bar/ring grow.

No bounce/elastic, no decorative motion that conveys no state. These use **GSAP** — see `chart-crafting.md` §6 for how to actually load it (self-host `./lib/` · CDN · npm) and the CSS-only fallback; a `gsap.*` call with no GSAP on the page silently does nothing.

---

## 1. Layout Shell & Information Architecture

**Canonical shell:** sidebar (`230–250px` desktop / 60px collapsed-icons / drawer on mobile; sticky, full-height, `padding:22–24px 16–18px`) + topbar (56–64px: breadcrumb, page actions, user menu) + content (`padding:20–22px 26–28px 40px`; the column absorbs width, so a hard `max-width` is optional). Content sub-grids: a KPI row `repeat(4–5,1fr)`, a primary split `1fr 300–340px` (chart + right rail), **gap `14–18px`**.

- **Nav IA:** ≤8 top-level items; group as main / config / account(pinned bottom); 24px between groups. Icon **+** label always (never icon-only). Selected state must be obvious (left bar or bg fill).
- **Depth ≤3 levels.** Breadcrumb when depth ≥3; every crumb clickable, current crumb dimmed.
- **Mobile bottom-nav ≤5 items**, labels always visible.
- **First screen earns its place:** the key metrics/state should read without scrolling or interaction.

**Alternative shells** (still inherit the substrate — these are container variants, not an excuse to skip §1's IA rules):
- **Floating rounded panel:** the whole dashboard sits as one card on a tinted page background — `max-width:~1440px; margin:2rem auto; border-radius:28–32px` with a soft ambient shadow (`0 40px 90px -50px`, not a hard drop shadow). Reads as a self-contained "app" rather than a bare page; works well for consoles/ops tools where the brand wants a product-y frame around the UI.
- **Bento console:** asymmetric `fr` columns in one grid (e.g. `2.15fr 1.7fr 1fr 1fr 1fr`) mixing a hero card (API key, primary metric) with square stat tiles — suits a console with many small heterogeneous stats where a uniform KPI row would waste space on the short ones.
- **Top pill-nav (no sidebar):** a rounded navbar with segmented pill links replaces the sidebar when IA is shallow (≤5 top sections) and mobile parity matters more than deep nesting — collapses to icon-only or hides below the tablet breakpoint rather than drawer-izing.
- **Centered bento / triptych (top-nav, no sidebar):** `max-width:1440px; margin:0 auto; padding:22–26px 30px 44px`; content is an fr-weighted bento or a fixed-flex-fixed triptych `grid-template-columns:296px 1fr 320px; gap:16–20px; align-items:start` (the `start` gives independent, masonry-like column heights).
- **Workflow shell (rail + form column + aside):** for pages you **operate** rather than read — publish/create wizards, merchant & admin consoles, config, settings. Sidebar + topbar (with the step rail) + a **width-capped form column** (`720–820px`) + a sticky aside (live preview / summary / help) + a sticky commit bar. Full recipe — numbered section cards, radio-card choices, the pre-submit check, derived budget panels, draft/commit — in **`workflow-ui.md`**. Don't build one of these out of dashboard parts; the failure modes are different (a dashboard fails by being unreadable, a console fails by being *unfinishable*).

> **None of these fit?** Then build the shell the product needs. These six are the proven starting points, not a closed set — the same escape `style-personas.md` gets on the brand side (`inspiration-catalog.md`, "the brief doesn't fit any of the 10 personas cleanly"). What you may **not** skip: the §0 substrate, this section's IA rules (≤8 grouped nav items, depth ≤3, first screen earns its place), and the pre-flight in §10. Invent the container, keep the floor. Say out loud which shell you rejected and why (SKILL.md §0.D), so a new shape is a decision and not a drift.

**Bento recipe — how tile-size variation is actually coded** (the fix for "a grid of 6 identical cards"):
1. **Uneven fractional tracks** (primary technique): `grid-template-columns: 2.15fr 1.7fr 1fr 1fr 1fr` — width encodes importance; no spans needed at desktop.
2. **`grid-template-areas` + row span** for true bento: e.g. `"left center right" / "left products products"` — one column runs full height while another spans two.
3. **`grid-column: 1/-1` at breakpoints** — hero/key tiles go full-width when tracks collapse.
4. **Vary by content role, not just size:** in one row alternate a dark gradient **key/hero card**, a **multi-stat stack**, small **square utility tiles**, and a **sparkline card** — equal-size tiles still read as varied because their silhouettes differ.
5. **One non-card centerpiece per page** breaks tile monotony: an image hero, a browser-preview mockup, a Gantt timeline, or a dark gradient key-card. A page fielding 6–10 *different* widget types never looks like a card gallery.

## 2. Data Tables

- **Alignment:** numbers/percentages/currency **right** (tabular-nums); text/dates **left**; status badges left or center. This is non-negotiable — it's how the eye scans magnitude.
- **Column width:** fixed for id/time/status (80–120px), `1fr` for name/description, narrow for numbers. No horizontal scroll on desktop.
- **Row density (pick one, lock it):** compact 32–36px (100+ rows) · normal 40–48px · comfortable 56–64px. Mobile min 44px.
- **Sort:** sortable headers show arrow on hover; active = bold header + ▲/▼ + `aria-sort`.
- **Filter:** above the table; reflect "N results"; one-click clear-all.
- **Pagination:** 10/25/50/100 per page; show "N total, X–Y shown"; current page highlighted.
- **Empty state:** icon + "no data" + why/next-step + CTA — never a blank grid.
- **Loading:** skeleton rows matching real row height (reserves space, kills CLS), not a centered spinner.

## 3. Charts — Selection & Rules

Pick by the question the data answers, not by what looks nice:

| Question | Chart | Avoid |
|----------|-------|-------|
| trend over time | line / area (single), multi-line (≤4 series) | stacked bars for trend |
| compare categories | bar; horizontal bar if labels long | pie when >5 categories |
| part-of-whole | pie/donut (≤5 slices), 100% stacked for proportion-over-time | 3D pie (always) |
| distribution | histogram, box plot | pie |
| correlation | scatter; bubble (3rd var = size) | line |
| ranking | horizontal bar; slope for rank change | vertical bar (label clipping) |
| hierarchy / flow | treemap, sankey | pie |
| geographic | choropleth / bubble map | table alone |
| activity over days | calendar heatmap | line |

**Rules:** ≤5–6 colors from an accessible palette (no pure red/green pairing — add texture/pattern too); grid lines low-contrast; data ≥3:1 contrast, labels ≥4.5:1; legend visible + interactive; tooltip on hover/tap; **no 3D, no rotated axis labels**; empty chart → "no data" message, never blank.

**Bars & any hand-built chart** (the #1 dashboard failure — the barcode chart): a bar's `height = value/max` from a **zero baseline** (never `value − min`), ≤~12 **wide** columns (`max-width:48–64px`, not barcode-thin), an **x-axis label under every column**, only the fill animates. Every bar the same height, or maxed to the ceiling, means the value never reached the geometry — that's broken, not styled. Use a **line/area for trends over time**, bars only to compare categories. **Building any chart by hand (single self-contained file, no chart lib) → you MUST load `references/chart-crafting.md`** (§4 bars + the broken-bar tripwire) — this table selects the chart; that file is how you draw it without shipping a barcode.

> This is the starter table for the common cases. For a dashboard with 5+ chart types, an unfamiliar data shape (funnel, gauge, network graph, forecast band, candlestick…), or an a11y-sensitive product — load `references/dataviz.md` for the full 25-type matrix, per-type accessibility grade + mandatory fallback, and library recommendations by rendering approach.

## 4. Forms

> These are the **field-level** rules — enough for a login, a filter bar, a profile edit. The moment the form's primary action is a **consequential commit** (发布 / 上线 / 提交 / 保存配置) — a publish wizard, a merchant console, a config page — you need the **page-level** layer too: **`workflow-ui.md`** (workflow shell, numbered section cards, upload/chip/stepper/**radio-card** kit, live-preview aside, the pre-submit check card, derived budget panels, draft + commit). A long consequential form built from this section alone will be technically valid and still unfinishable.

- **Label above input** (default, mobile-friendly); left-aligned only for dense settings screens. Never placeholder-as-label.
- **Required mark** (`*` or "(required)"); label 12–14px, weight 500; `htmlFor` matches `id`.
- **Validate on blur** (timely, non-nagging); on-submit lists all errors; on-change only for live checks (username availability).
- **Errors below the field**, red text + `aria-invalid` + `aria-describedby`, stating cause **and** fix ("Password needs 8+ chars" not "Invalid").
- **Input states (all six):** default / hover / focus (2px accent ring) / disabled / error / success.
- **Grouping:** `fieldset` + `legend`, 24–32px between groups. Multi-step: step indicator + progress, validate per step.

## 5. Interaction States (ship all of them)

- **Loading:** skeleton (matches final shape) for views/tables; small spinner only for button submits. Show feedback past ~300ms.
- **Empty:** icon/illustration + "what's missing + why + next step" + CTA. Composed, not a void.
- **Error:** field-level (below input) · form-level (top, lists issues + locations) · global/network (toast or centered, with retry).
- **Success:** `aria-live="polite"` toast 3–5s, or page-level confirm with the key facts (order #, timestamp) + next action.
- **Disabled:** `opacity .5; cursor: not-allowed; pointer-events: none`. Used for unmet conditions, not as a mystery.

## 6. Component System

Reach for standard components; never reinvent affordances. Typical inventory: **Button · Input · Label · Form · Select · Checkbox · Radio · Textarea · Switch · Calendar · Slider** (input) · **Card · Tabs · Accordion · Table · Breadcrumb** (layout) · **Dialog · AlertDialog · Drawer · Popover · Command(⌘K)** (overlay) · **Toast · Alert · Progress · Skeleton** (feedback) · **Avatar · Badge** (display).

- **Modal discipline:** prefer inline or Drawer for editing; Dialog for confirmations / small forms; AlertDialog for destructive actions. Never nest modals; never a <300px dialog.
- **Vocabulary consistency (audit before ship):** one button variant per intent (default/outline/destructive) used everywhere; one card padding; one icon size; one radius scale; semantic color tokens (`text-foreground`, `bg-muted`) never hardcoded hex; spacing only on a 4px grid.
- **Production/scheduling timeline (Gantt-lite):** when the product is inherently calendar-driven (creative pipelines, ops schedules, launch plans), a day-ruler header + per-row bars positioned by `left`/`width` **percentage** (not px) with a live "today" marker line reads as a first-class view, not a table substitute. Connect a marker to a floating context card (a call invite, a comment thread) via a dashed line whose endpoints are computed from `getBoundingClientRect()` deltas against a shared container, recomputed on resize. This absolute-positioning approach doesn't survive a single-column mobile reflow — degrade to a static stacked card list below the shell's tablet breakpoint rather than trying to keep percentages meaningful in one column.
- **Annotated mockup / hotspot pins:** a CSS "browser chrome + block" mockup of the product's own live preview, or a real photo the product is about (a patient/pet, a job site), gets small pill labels pinned at `top/left` **percentage** coordinates with a staggered fade-in (each ~120–150ms after the last). This is legitimate under `anti-cheap.md`'s mockup boundary only when the mock **is** the product's own feature (a CMS site-preview pane, a health-status annotation surface) — not a stand-in for a screenshot the page couldn't produce.

## 7. Cognitive Load & Usability

- **Cut extraneous load:** unclear nav, vague labels ("Options" → "Display settings"), visual clutter, inconsistent patterns, redundant steps, duplicated info.
- **Structure intrinsic load:** step indicators for multi-step; **progressive disclosure** (advanced filters collapsed, rare actions folded, optional fields grouped); sensible defaults / examples.
- **Working memory ≤ 4–5 choices per screen**; 10–20 rows per table screen; nav depth ≤3.
- **Nielsen heuristics** as a review rubric (score 0–4 each): status visibility · match real world · user control & undo · consistency · error prevention · recognition over recall · flexibility/shortcuts · minimal design · error recovery · help. <20/40 → rework.

## 8. Product Tokens & Density

Three-layer tokens: **primitive** (`--blue-600:#2563EB`, `--space-4:1rem`) → **semantic** (`--color-primary`, `--spacing-section`) → **component** (`--button-padding`, `--card-padding`).

**Palette → `product-palettes.md`.** Pick the **neutral ramp** first (5 tinted families — it's ~80% of the page), then one **accent** from 16, or take one of the **12 paste-ready sets**. That file's §1 token contract is the full role list — `--page` `--bg` `--panel-2` `--border` · `--ink-1/2/3` · `--accent` `--accent-2` `--accent-soft` `--on-accent` `--glow` · `--up` `--down` `--warn` · `--shadow` `--shadow-lg` — and it's what the **semantic** layer above resolves to. Run its §7 collision check before shipping.

- **Spacing:** 4px base (4/8/12/16/24/32/48). Denser than marketing. Always tokens, never hardcoded.
- **Type scale (fixed, NOT `clamp()`):** 12 / 14 / 16 / 18 / 20 / 24px. Product users are on fixed-size screens; respond by changing columns, not font size (tables must stay aligned).
- **Radius/elevation:** lock one scale. Most components md radius + md shadow; cards lg+lg; inputs sm, shadow on focus only; modal lg + xl shadow.
- **UI font:** high-readability sans (`-apple-system, 'Segoe UI', Roboto, Inter, sans-serif`); mono for data/code. No serif/script in UI labels. Dark mode tested independently, not inverted.

## 8.A Typography Precision (add to the product substrate)

Details that separate a polished product from a rough one:

- **Orphan prevention:** `text-wrap: balance` on headings; `text-wrap: pretty` on multi-line body copy. Prevents single-word last lines without manual `&nbsp;`.
- **Card CTA alignment:** in a card grid, CTA buttons must be bottom-locked — use `display: flex; flex-direction: column` on the card, `margin-top: auto` on the CTA. Buttons floating at different heights based on copy length is a layout bug, not a style choice.
- **Pricing table baseline:** when comparing plans side-by-side, feature lists must start at the same Y coordinate across all columns. Use matching `min-height` or `align-items: start` + fixed padding on the description block, not variable-height text pushing features down.
- **Optical compensation:** mathematical centering ≠ visual centering. Apply 1–2px `translateY` or asymmetric padding to: play buttons inside circles, icons in icon-buttons, and avatar initials in circular containers. Never skip this on any centered symbol with visual weight asymmetry.

---

## 9. Product-Specific Anti-Patterns (add to the blacklist)

- **Decorative motion** that conveys no state (bounce/elastic on hover). → `ease-out` shadow/opacity feedback only.
- **Display/serif fonts in UI labels, buttons, data.** → system sans.
- **Reinvented standard controls** (custom checkbox div). → standard accessible component.
- **Modal-first** for everything. → inline / drawer first.
- **Inconsistent component vocabulary** (same action, different look). → lock variants.
- **Spinner-only loading** with layout shift. → skeletons.
- **Color as the only signal** in charts/status. → icon/text/pattern too (full anti-pattern list in `references/dataviz.md` §5).
- **Flat-white-card dashboard** — a pure `#fff`/`#000` page, cards outlined with a hard `1px solid #ccc/#ddd/#333`, no tinted shadow, and a generic blue+green+orange chart palette. The single most common cheap-dashboard tell. → §0 product substrate: off-tint page, hairline-or-none border, whisper **tinted** shadow, one **branded** accent, KPI-tile anatomy.
- **Brand-page moves smuggled into a dashboard** — grain overlay, vignette, `clamp()` giant hero type, forced dark-default, or a WebGL/Three.js hero engine on an admin panel. → those are brand-register grammar (`design-dna.md`); a dashboard uses §0 instead.

## 10. Product Pre-Flight (in addition to the shared §8)

- [ ] **Product substrate (§0):** off-tint page (no pure `#fff`/`#000`); cards use a hairline-or-none border + a whisper **tinted** shadow (not a hard grey box); one **branded** accent (not the generic blue+green+orange triad), substrate tinted to it; KPI tiles follow the anatomy; **no brand grain / vignette / `clamp()` hero type / hero engine** on the dashboard.
- [ ] **Palette (`product-palettes.md` §7):** tinted neutral ramp (not pure grey); accent doesn't collide with `--up`/`--down`; `--ink-3` clears 4.5:1 on `--bg`; text-on-accent clears 4.5:1 (lime/amber fills need near-black); `--glow` used at most once per screen; **no raw hex outside `:root`** — grep for `#3B82F6` and friends.
- [ ] **If it's a workflow page** (console / wizard / config — the §1 workflow shell): run `workflow-ui.md` §9 as well. Chiefly — a live **pre-submit check card** with jump-to-fix links, derived totals showing their **breakdown**, draft/autosave, and a commit confirm that states the **consequence**. A disabled submit button with no stated reason is a hard fail.
- [ ] IA ≤3 levels, sidebar ≤8 grouped items, key info visible on first screen.
- [ ] Tables: alignment rules, locked row density, sort/filter/pagination, empty + skeleton states.
- [ ] Charts: type fits the question, legend + tooltip, no 3D, accessible palette; a11y grade checked + mandatory fallback shipped if Fragile/Never-primary (`references/dataviz.md` §2, §6).
- [ ] Forms: label-above, required marks, blur validation, errors-below with fix.
- [ ] All five interaction states shipped (loading/empty/error/success/disabled).
- [ ] Component vocabulary consistent; modal used sparingly; tokens not hardcoded.
- [ ] Dark mode contrast tested independently; touch targets ≥44px.
- [ ] Still passes the substrate + cheapness blacklist (it's a dashboard, not an excuse to look cheap).
