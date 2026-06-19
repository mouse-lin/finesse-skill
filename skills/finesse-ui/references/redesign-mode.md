# Redesign Mode — Upgrading an Existing Page

When the brief is "make this better" (not "build from scratch"), follow a different flow: **audit first, then target the highest-leverage fixes**. Never full-rebuild for a single complaint. Identify which module is responsible and adjust only that.

---

## Step 1: Detect the Mode

Redesign is triggered when any of these are present:
- Existing HTML/CSS/JS to work from
- Screenshots or URLs of a live page
- Words like "redesign", "improve", "polish", "fix", "upgrade"

Once detected, **do not start generating new code.** Go to Step 2.

---

## Step 2: Audit (Seven Categories)

Scan the existing page against these categories. Note failures — do not fix yet.

### 2.A Typography
- [ ] Display headings have negative tracking + line-height < 1.0?
- [ ] Weight contrast between heading and body (e.g., 800 vs 300)?
- [ ] Orphaned last words? (`text-wrap: balance` / `text-wrap: pretty` needed?)
- [ ] Mixed-family emphasis? (serif word injected into a sans headline)
- [ ] Font choices have a stated reason, or are they reflex-defaults?

### 2.B Color & Substrate
- [ ] Pure `#fff` / `#000` present? Neutrals untinted?
- [ ] Grain layer missing?
- [ ] Accent color drifts across sections?
- [ ] Hard `#333` borders instead of translucent?
- [ ] Default beige+brass palette?

### 2.C Layout & Rhythm
- [ ] Same layout family used more than twice?
- [ ] Eyebrow count > ceil(sections / 3)?
- [ ] Hero has > 4 text elements?
- [ ] Nav taller than 80px?
- [ ] Mobile layout declared per multi-column section?

### 2.D Components & Interaction States
- [ ] Card CTA buttons misaligned vertically in a grid?
- [ ] Loading state missing (spinner-only, or nothing)?
- [ ] Empty state is a blank void?
- [ ] Error state omits the fix instruction?
- [ ] Pricing table columns start feature lists at different Y positions?

### 2.E Data & Numbers (product register)
- [ ] Numbers in tables not right-aligned with `tabular-nums`?
- [ ] Optical compensation missing on icon-buttons or circular elements?
- [ ] Charts use 3D, rotated labels, or pie > 5 slices?
- [ ] Color as the only data signal (missing texture/icon/label)?

### 2.F Motion
- [ ] `window.addEventListener('scroll', …)` driving animation on every frame?
- [ ] No `prefers-reduced-motion` fallback?
- [ ] `power1.out` or linear easing on reveals?
- [ ] > 1 marquee?

### 2.G Strategic Omissions
- [ ] No custom 404 page?
- [ ] Footer missing legal links (Privacy, Terms)?
- [ ] No skip-to-content link?
- [ ] Placeholder data ("Jane Doe", lorem ipsum) still present?
- [ ] Dead-end user flows (no way back from a sub-page)?

---

## Step 3: Set Protection Rules

Before any edits, declare:
- **What must not change:** any element the client/user considers identity-defining (logo, hero copy, brand color, existing sections that work).
- **Scope boundary:** "fix only X" vs "full redesign within existing structure."

Violations of protection rules require explicit user approval before proceeding.

---

## Step 4: Fix Priority Order

Apply fixes in this sequence (highest leverage → lowest risk first):

| Priority | Fix | Why first |
|----------|-----|-----------|
| 1 | **Typography** — font swap, tracking, line-height, weight contrast | Biggest visual improvement, zero structural risk |
| 2 | **Color palette cleanup** — tinted neutrals, translucent borders, accent lock | Second-biggest immediate impact |
| 3 | **Hover/active/focus states** — all interactive elements | Quick wins, high usability impact |
| 4 | **Layout & spacing** — section padding, eyebrow count, layout family diversity | Mid-risk, significant polish |
| 5 | **Component upgrades** — replace reinvented controls, add skeleton loading, align card CTAs | Requires structural edits |
| 6 | **Loading/empty/error states** | Often missing entirely |
| 7 | **Typographic precision** — orphan fix, optical compensation, pricing table alignment | Fine polish, low risk |

---

## Step 5: Never-Silent Rule

Changes that **require explicit user approval** before proceeding:
- Removing a section
- Changing the primary font family
- Changing the accent color
- Restructuring nav or information architecture
- Any change the user marked as protected in Step 3

State the change and reason, then wait for a green light.

---

## Step 6: Post-Fix Verification

After fixes, run the standard **§8 Pre-Flight** (`preflight.md`) — the same gates apply to redesigns. A redesign that passes the audit but fails Pre-Flight is not done.

Additionally:
- [ ] **Regressions:** did any working section break? Test every section, not just the ones you touched.
- [ ] **Before/after delta:** can you articulate in one sentence what the biggest improvement is? If not, the fix was too diffuse.
- [ ] **Scope kept:** did you change only what was agreed in Step 3?

---

## Iteration Guide (same as new builds)

After delivery, map feedback to the correct module — never rebuild from scratch for a single complaint.

| User says | Action |
|-----------|--------|
| "too plain / boring" | Raise SPECTACLE +2, upgrade engine type |
| "wrong vibe" | Re-run §2 with a different persona |
| "feels generic" | Name and reject the current soul, pick non-obvious persona |
| "change the colors" | Re-run color strategy in `design-dna.md`, maintain accent lock |
| "feels slow / heavy" | Lower SPECTACLE, switch to Engine E (CSS-only), reduce particle count |
| "needs to work on mobile" | Declare layout per multi-column section, `min-h-dvh`, 44px touch targets |
