# Pre-Flight Check

Run before saying "done." Merges the substrate check, the cheapness scan, the spectacle verification, and the accessibility gates. Failing any **hard rule** is shipping broken work — fix before delivery. Soft rules are judgment calls; if you skip one, say why.

---

## A. Direction & Soul (hard)

- [ ] **Design Read** was committed (industry · soul · register · SPECTACLE · engine).
- [ ] **Anti-default named** — the lazy aesthetic for this brief was identified and beaten.
- [ ] **One soul, one accent, locked** across every section. No drift, no second accent unless duotone-by-design.
- [ ] **Theme locked** — no warm-paper section inside a dark page (unless deliberate one-time switch).
- [ ] Page matches its **register** (brand = bold/spectacle; if it's really product-UI, finesse is the wrong tool).

## B. Premium Substrate (hard)

- [ ] **Grain** layer present (`opacity .02–.05`).
- [ ] **No pure `#fff`/`#000`**; neutrals tinted toward brand hue.
- [ ] **Translucent borders** only; no hard `#333` lines; shadows hue-tinted (not pure black on light bg).
- [ ] **Display type tension** — `clamp()` size, negative tracking, line-height .86–.95, weight contrast against light body.
- [ ] **Layered z-index** depth (engine · grain · vignette · content).

## C. Spectacle (hard if SPECTACLE ≥ 7)

- [ ] **Spectacle shown, not claimed** — a real working engine exists.
- [ ] **60fps on a mid-range device** (not the dev machine). Below 50fps → simplified.
- [ ] **Canvas DPR-adapted** (retina not blurry).
- [ ] **Progressive enhancement** — page is complete and readable with the engine removed.
- [ ] **`prefers-reduced-motion`** freezes the engine to a still frame / static hero.
- [ ] **Motivated motion** — every animation has a one-sentence reason. ≤1 marquee.

## D. Layout Discipline (hard)

- [ ] **Hero fits the viewport** — headline ≤2 lines, subtext ≤20 words, CTA visible without scroll. Max 4 text elements.
- [ ] **Nav** single line, ≤80px tall.
- [ ] **Eyebrow count ≤ ceil(sections/3).**
- [ ] **≥4 layout families** on a long page; no family more than twice; ≤2 consecutive image+text zigzags.
- [ ] **Mobile collapse** declared per multi-column section. No horizontal scroll at 375px. `min-h-dvh` over `100vh`.

## E. Cheapness Scan (hard)

- [ ] No em-dashes in copy. No div-based fake screenshots. No gradient-text/glass/AI-purple as default.
- [ ] No fake-precise numbers without a source. No banned beige+brass default palette.
- [ ] No identical card grids. No numbered `01·02·03` unless a real sequence.
- [ ] **Real imagery** where the brief implies it (food/hotel/fashion/travel/product).
- [ ] Real SVG logos (not text wordmarks) on any "trusted by" wall.
- [ ] Fonts chosen with a reason — not a blind reach for Inter/Fraunces/Instrument Serif.

## F. Copy Self-Audit (hard)

- [ ] Re-read every visible string. No broken grammar, unclear referents, or AI-cute wordplay.
- [ ] One copy register per page. Quotes ≤3 lines with full attribution.
- [ ] One label per CTA intent across nav/hero/footer.

## G. Accessibility (hard)

- [ ] Contrast WCAG AA — body ≥4.5:1, large ≥3:1. Includes **buttons over photos** (scrim/stroke), placeholders, helper/error text, focus rings.
- [ ] Visible focus state on every interactive element; keyboard-reachable nav + CTAs.
- [ ] Button text fits one line at desktop; no wrapped CTAs.
- [ ] Touch targets ≥44px. Form labels above inputs (never placeholder-as-label).

## H. Performance (soft)

- [ ] Animate only `transform`/`opacity`. `will-change` sparingly.
- [ ] Heavy engines lazy-loaded. Responsive images (WebP/AVIF, `srcset`). CLS < 0.1.

---

### The four ship-tests (from overdrive thinking)

1. **wow** — would someone who hasn't seen it react?
2. **removal** — if you delete the engine, is the experience clearly worse?
3. **device** — still smooth on a phone / Chromebook?
4. **context** — does this spectacle actually serve *this* brand and audience, or is it showing off?

If "removal" or "context" fails, the spectacle is decoration, not finesse. Cut or rework it.

---

## I. Self-Grading Loop (run last, before saying "done")

Generate 5 sharp questions about your specific output, then answer each with concrete evidence from the code/copy you wrote — not a generic "yes." If any answer reveals a failure, fix it before shipping.

**Template — fill in with your actual output:**

1. **Engine check:** "Did I ship a working `[engine type]`, or is there a gradient blob/placeholder where the hero should be?" → [evidence]
2. **Soul check:** "Is the soul I picked (`[persona name]`) actually visible in the palette, typeface, and motion — or did I drift back to a generic aesthetic?" → [evidence]
3. **HARD BAN sweep:** "Does the copy contain any em-dashes? Are there any eyebrow labels on more than 1-in-3 sections? Any fake numbers?" → [evidence]
4. **Substrate check:** "Did I apply grain, type tension (negative tracking + weight contrast), and translucent borders to every section — or just the hero?" → [evidence]
5. **Dial honesty:** "Is the page I built actually `SPECTACLE=[n]` and `DENSITY=[n]`, or did I under-deliver on what I committed to?" → [evidence]

**A "yes" with no evidence = unverified = fail.** Re-read the output, quote the specific line or value that proves it.
