# Product UI — Dashboards, Admin, Data Apps

The **product** register: design SERVES the product. Dashboards, admin panels, analytics, data tables, app shells, settings. Optimize for **clarity · density · usability** — fast task completion, no errors. Spectacle is not the goal here; **craft still is.**

> This path still inherits the **substrate** (`design-dna.md`: tinted neutrals, no pure #fff/#000, translucent borders, OKLCH, dark-mode parity) and the **cheapness blacklist** (`anti-cheap.md`). A dashboard can be dense and functional without looking cheap. SPECTACLE dial = 1–4; DENSITY = 6–9; motion is feedback only.

---

## 1. Layout Shell & Information Architecture

**Canonical shell:** sidebar (240px desktop / 60px collapsed-icons / drawer on mobile) + topbar (56–64px: breadcrumb, page actions, user menu) + content (max-width 1400px, `repeat(auto-fit, minmax(280px, 1fr))` grid).

- **Nav IA:** ≤8 top-level items; group as main / config / account(pinned bottom); 24px between groups. Icon **+** label always (never icon-only). Selected state must be obvious (left bar or bg fill).
- **Depth ≤3 levels.** Breadcrumb when depth ≥3; every crumb clickable, current crumb dimmed.
- **Mobile bottom-nav ≤5 items**, labels always visible.
- **First screen earns its place:** the key metrics/state should read without scrolling or interaction.

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

## 4. Forms

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

## 7. Cognitive Load & Usability

- **Cut extraneous load:** unclear nav, vague labels ("Options" → "Display settings"), visual clutter, inconsistent patterns, redundant steps, duplicated info.
- **Structure intrinsic load:** step indicators for multi-step; **progressive disclosure** (advanced filters collapsed, rare actions folded, optional fields grouped); sensible defaults / examples.
- **Working memory ≤ 4–5 choices per screen**; 10–20 rows per table screen; nav depth ≤3.
- **Nielsen heuristics** as a review rubric (score 0–4 each): status visibility · match real world · user control & undo · consistency · error prevention · recognition over recall · flexibility/shortcuts · minimal design · error recovery · help. <20/40 → rework.

## 8. Product Tokens & Density

Three-layer tokens: **primitive** (`--blue-600:#2563EB`, `--space-4:1rem`) → **semantic** (`--color-primary`, `--spacing-section`) → **component** (`--button-padding`, `--card-padding`).

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
- **Color as the only signal** in charts/status. → icon/text/pattern too.

## 10. Product Pre-Flight (in addition to the shared §8)

- [ ] IA ≤3 levels, sidebar ≤8 grouped items, key info visible on first screen.
- [ ] Tables: alignment rules, locked row density, sort/filter/pagination, empty + skeleton states.
- [ ] Charts: type fits the question, legend + tooltip, no 3D, accessible palette.
- [ ] Forms: label-above, required marks, blur validation, errors-below with fix.
- [ ] All five interaction states shipped (loading/empty/error/success/disabled).
- [ ] Component vocabulary consistent; modal used sparingly; tokens not hardcoded.
- [ ] Dark mode contrast tested independently; touch targets ≥44px.
- [ ] Still passes the substrate + cheapness blacklist (it's a dashboard, not an excuse to look cheap).
