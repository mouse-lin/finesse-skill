# Workflow UI — Wizards, Publish Flows, Consoles, Settings

The **product register's second job.** `product-ui.md` covers pages you *read* — dashboards, analytics, monitoring. This file covers pages you *operate*: a merchant publishing a campaign, an admin configuring a rule, an ops user filing a batch job, anyone filling a long form that produces a real, consequential thing.

The distinction matters because the two have different failure modes. A dashboard fails by being **unreadable**. A workflow page fails by being **unfinishable** — the user abandons at step 3, or submits something wrong and doesn't find out until it's live.

> **Inherits everything from `product-ui.md`:** the §0 product substrate (tinted page, hairline-or-none cards, whisper tinted shadows), §4 form rules, §5 interaction states, §6 component vocabulary, §8 tokens/density, §9 anti-patterns. Palette from `product-palettes.md` (a merchant console is usually a **light** set with a **dark rail** — §4.A there). **Not** brand grammar: no grain, no vignette, no `clamp()` hero type, no hero engine. SPECTACLE 1–3; DENSITY 6–8.

---

## 0. Is this a workflow page?

Yes, if the page's job is any of:

- **Create / publish** — post a task, launch a campaign, list a product, file a claim, schedule a send.
- **Configure** — rules, permissions, integrations, pricing tiers, automation.
- **Review & approve** — an inbox of things to accept/reject with a detail pane.
- **Settings** — account, team, billing, notifications.

The tell: the primary action is a **commit** (发布 / 保存 / 提交 / 上线), it's **consequential**, and the user needed to think before pressing it.

If the page is a form but the commit is trivial (a login, a search filter, a profile name), you don't need this file — `product-ui.md` §4 is enough.

---

## 1. The Workflow Shell

The sixth morphology, alongside `product-ui.md` §1's five dashboard shells. Three columns, and the third is what most implementations forget.

```
┌──────┬─────────────────────────────────────┬────────────────┐
│      │  topbar: ‹back  title   ①─②─③─④    │                │
│ rail ├─────────────────────────────────────┤   aside        │
│      │                                     │                │
│ nav  │  the form column                    │  live preview  │
│      │  (numbered section cards)           │  · or ·        │
│      │                                     │  summary       │
│      │                                     │  · or ·        │
│      ├─────────────────────────────────────┤  help          │
│      │  sticky action bar: [draft] [COMMIT]│                │
└──────┴─────────────────────────────────────┴────────────────┘
```

**Metrics.** Rail `230–250px` (`product-ui.md` §1's sidebar, unchanged). Form column `1fr`, capped at **`720–820px` of readable width** — a form field stretched to 1400px is unfillable; the eye loses the label-to-input link. Aside `300–380px`, `position:sticky; top:{topbar+16px}`. Gap `16–20px`.

**The step rail (multi-step only).** Live in the **topbar**, not the sidebar — the sidebar is app navigation, the steps are *this task's* progress, and conflating them makes users think a step is a page they can lose. Numbered circles + label, connected by a 1px line. Three states: **done** (accent fill + check, clickable — always let users go back), **current** (accent ring + accent label), **todo** (`--panel-2` fill, `--ink-3` label, not clickable until reachable). Never more than **4–5 steps**; beyond that, collapse into sections on one page (§2) — a 7-step wizard is a design failure, not a thorough one.

**Single-page vs. stepped.** Default to **one page with numbered sections** (§2). Step it only when: the form is genuinely long (>15 fields), later steps *depend* on earlier answers, or each step needs its own validation gate. Stepping hides the total cost from the user, which raises abandonment — pay that cost only when it buys something.

**Mobile.** The aside is not a column — it collapses to a **bottom sheet** triggered by a "预览 / Preview" button in the sticky bar. The form column goes full width. The step rail becomes a compact `2/4` counter + progress line. Never try to keep three columns below the tablet breakpoint.

---

## 2. Numbered Section Cards

The workhorse. Each logical group of fields is one card, on the product substrate (`product-ui.md` §0.1), with a numbered badge.

- **Badge:** `24–28px` square, `--r-sm`, `--accent-soft` background, accent-colored numeral, `font-weight:700`. It's an **ordinal**, not decoration — which is what separates it from the `01 · 02 · 03` marker slop banned in `anti-cheap.md`. The number here means "the second thing you do", and the pre-submit checklist (§5) refers back to it. If your numbers don't earn that, drop them and use plain headings.
- **Header row:** badge + section title (`15–16px / 600`) + optional right-aligned helper link ("管理常用" / "Learn more"). Optional one-line description in `--ink-3` beneath.
- **Section spacing:** `14–18px` between cards; `20–24px` internal padding. Fields inside a card: `16–20px` apart.
- **Optional / advanced sections collapse.** Progressive disclosure (`product-ui.md` §7): show the 80% path expanded, fold "高级设置 / Advanced" into an accordion, closed by default, with a count badge when it holds non-default values ("高级设置 · 2 项已改") so a collapsed section can never hide a surprise.

**Field layout inside a card.** `product-ui.md` §4 defaults to label-above. A dense config form is the stated exception: **label-left** (`120–140px` label column, right-aligned, `--ink-2`, with the `*` in `--down`) reads faster when there are 15+ short fields, and it's what admin users expect. Pick one and hold it for the whole page. Two fields per row is fine at desktop for short inputs; collapse to one at tablet. Helper text sits **under the input**, `12px`, `--ink-3` — and it explains a *consequence* ("超过该时间将无法报名"), never restates the label.

---

## 3. The Field Kit

The controls a workflow page needs that a dashboard doesn't. Each is a standard affordance — build them accessible, don't reinvent (`product-ui.md` §9).

- **Character counter** — `12/30` bottom-right *inside or under* the input, `--ink-3`. Turns `--warn` at 90%, `--down` at the limit. Counts **grapheme clusters**, not `.length` (an emoji or a CJK character isn't 2 chars to a user). Never block typing at the limit; let them overshoot and show the error — silently swallowing keystrokes reads as a broken keyboard.
- **Upload dropzone** — dashed `1.5px` border at `--border`-ish alpha, `--panel-2` fill, `--r`, `100–120px` tall, centered icon + "上传封面" + a spec line ("JPG/PNG · 16:9 · ≤5MB") in `--ink-3`. **The spec line is not optional** — an upload that only reveals its constraints in an error message is a trap. Hover/drag-over: accent border + `--accent-soft` fill. Always pair with an alternate path (**"从素材库选择"**) as a sibling button — dropzones are hostile to users who don't have the file locally.
- **Chip multi-select** (platforms, tags, categories) — pill buttons, `--panel-2` at rest, `--accent-soft` fill + accent border + accent text when selected. Selected state must survive greyscale (add a check glyph or a weight change) — color alone is banned (`product-ui.md` §9). A trailing `+ 新增` chip when the set is user-extensible.
- **Number stepper** — right-aligned `tabular-nums` value, up/down carets stacked at `11px` each on the right edge. **Always allow typing.** Stepper-only fields are hostile at any scale above 20. Show the unit outside the input (`元`, `个`), not as placeholder text.
- **Radio card group** — the pattern for a **consequential, mutually-exclusive choice with tradeoffs** (分发方式: 公开大厅 / 自己派 / 单个中介 / 定价矩阵). Each card: radio dot (top-left) + title (`14px/600`) + **one line of consequence** in `--ink-3` (*"任务公开展示在平台大厅，达人可见并参与"*). Selected: accent border (`1.5px`), `--accent-soft` fill, and the radio filled. `grid-template-columns:repeat(auto-fit,minmax(210px,1fr)); gap:12px`. **This is the single highest-value control on a workflow page** — it turns an opaque radio list into a decision the user can actually make. Use it whenever the options differ in *outcome*, not just in *label*. Keep it to ≤4; beyond that, use a plain radio list with helper text.
- **Date/time range** — two inputs, not one, each with a calendar icon and a helper line stating the consequence of the boundary. Validate the ordering (end > start) **on blur of the second field**, not on submit.
- **Money** — always `tabular-nums`, always a visible currency symbol, always the unit outside the field. Amounts derived from other amounts belong in the summary panel (§6), never in an input the user might think is editable.

---

## 4. The Aside — live preview, summary, or help (pick one, commit)

The third column is what makes a workflow page feel like a product instead of a Google Form. Three legitimate jobs; **one page picks one**.

### 4.A Live preview — *the strongest option when the form produces something a third party will see*

A merchant writing a task sees the consumer-facing card. A CMS author sees the rendered page. Legitimate under `anti-cheap.md`'s mockup boundary **because the mock IS the product's own output** — this is not a fake screenshot standing in for a real one (`product-ui.md` §6 makes the same allowance for a CMS site-preview pane).

- **Bind it to the form, live.** A preview that updates on submit is a lie about what it is. Debounce text at `~150ms`; images update on upload-complete.
- **Render empty state as the real empty state.** An unfilled title shows a `--panel-2` skeleton block, not lorem ipsum and not "标题". The user must be able to see *what a consumer sees when they leave a field blank*.
- **Match the target medium's frame.** A phone-shaped viewport (`~300px` wide, `--r-lg`, hairline border) for an H5/mobile output. Don't render mobile output in a desktop card — the line breaks lie.
- **Label it honestly.** A one-line disclaimer at the bottom: *"预览内容仅供参考，实际展示以线上为准"*. If the preview is approximate, say so; users make pricing decisions off these.

### 4.B Summary / receipt — *when the form produces a commitment (money, quantity, time)*

A recap of the decisions made so far, in commit order, so the user can check without scrolling. Read-only, `--ink-2` labels, `--ink-1` values, hairline dividers. Every row links back to its section (§5's jump pattern).

### 4.C Help / context — *when the form is genuinely hard*

Not a FAQ dump. The **current field's** guidance, swapped on focus. If you can't keep it that responsive, put the guidance in helper text under the field and drop the column — a static help sidebar is where content goes to be ignored.

---

## 5. The Pre-Submit Check — *the most-missed pattern in the register*

A dedicated card, last section before the action bar, listing **every requirement with its live status**. This is the difference between a form that punishes you at submit and one that walks you to the door.

```
⑦ 发布检查        ⚠ 请检查以下内容，确认信息完整准确    发现 1 项问题 ⌄
   ✓ 任务标题      已填写
   ⚠ 任务封面      未上传封面                          去上传 →
   ✓ 佣金与名额    单人佣金 ¥100，总名额 50
   ✓ 时间设置      招募截止 2024-06-05 23:59
```

- **Every row states the *value*, not just "OK".** "✓ 已填写" is worthless — "✓ 单人佣金 ¥100，总名额 50" lets the user proofread without scrolling back. This is the whole point of the pattern.
- **Every failing row carries a jump link** (`去上传 →`) that scrolls to the section, focuses the field, and flashes the section card. This is `product-ui.md` §5's *"form-level error: lists issues + locations"* — the link **is** the location.
- **Status is live**, recomputed as the user types — not populated on a failed submit.
- **Three states only:** ✓ pass (`--up`) · ⚠ blocking (`--down`, blocks commit) · ⓘ advisory (`--warn`, does *not* block — "未设置封面会降低曝光"). Keep advisories visually distinct from blockers, or users learn to ignore both.
- **Never disable the commit button as the sole feedback.** A greyed-out 发布 with no explanation is the cruelest pattern in admin UI. Either the button is enabled and submitting surfaces the blockers here, **or** it's disabled *and* this card says exactly why. `product-ui.md` §9: disabled is for unmet conditions, never a mystery.

---

## 6. Derived Summary Panels

When the form's inputs imply a number the user is accountable for — a budget, a total, a headcount, a send volume — **compute it and show the arithmetic.**

```
⑥ 预算估算 ⓘ
   预算方式    [按总预算] [按单价预估]
   预算总额    ¥ 5,000.00        ← --ink-1, 22–26px, tabular-nums, accent-tinted
   ─────────────────────────
   单人佣金             ¥ 100.00
   平台服务费 (5%)      ¥ 250.00     ← --ink-3 rows: the breakdown
   预计可招募达人        约 50 个
```

- **Show the components, not just the total.** A total with no breakdown is a number the user has to trust. A breakdown is a number they can check — and it's where they catch their own typo.
- **Recompute live**, `tabular-nums`, and animate the digits only if the change was user-initiated (a short count-up on the total; never a bounce). No animation on load — this is a derived value, not a KPI reveal.
- **Fees and cuts are stated explicitly**, with the rate visible (`平台服务费 (5%)`). Hiding a fee until checkout is a dark pattern (`commerce-ui.md` §6) and it's the same offense here.
- **When an input is missing, show the formula, not `¥0.00`** — "填写单人佣金与名额后估算" beats a confident, wrong zero.

---

## 7. Commit, Draft, and Undo

- **Sticky action bar.** `position:sticky; bottom:0`, full width of the form column, `--bg` with a `backdrop-filter:blur(8px)`, a hairline **top** border, `14–16px` padding. Secondary action left or center (`保存草稿`, outline variant), primary right (`发布任务`, accent fill + `--glow`). **Never more than one primary.**
- **Draft is not optional** on any form over ~10 fields. Autosave on a `~3s` debounce **and** an explicit button; show `已保存 · 14:32` in `--ink-3` next to the buttons. A workflow page that loses a half-hour of typing to a refresh has failed regardless of how it looks.
- **Confirm before consequence, not before effort.** A `AlertDialog` on 发布 (it costs money, it's public) — yes. A confirm on 保存草稿 — no. Word it with the *consequence*, not "Are you sure": *"将向 50 位达人公开此任务，预算 ¥5,000 将被冻结。"*
- **Submitting state:** the primary button shows an inline spinner and its label becomes the present participle (`发布中…`); the bar stays put; the form goes `aria-busy`. Never a full-page blocking overlay.
- **After commit:** a page-level confirmation with the **key facts** (任务 ID, 生效时间, 冻结金额) + a next action — not a toast that vanishes before it's read (`product-ui.md` §5). For consequential commits, name the undo window if one exists.

---

## 8. Anti-Patterns *(add to `product-ui.md` §9)*

- **Disabled submit with no stated reason.** → §5's check card.
- **Validation only on submit.** → validate on blur; the check card is live.
- **A wizard where a page would do.** Steps hide the total cost and raise abandonment. → numbered sections on one page unless §1's three conditions hold.
- **A preview that isn't live**, or that shows lorem ipsum where the user's empty field will be. → bind it, and render the real empty state.
- **A total with no breakdown.** → §6.
- **Radio list where the options have different *consequences*.** → §3's radio card group. This is the fix for "the form is technically complete but nobody knows which one to pick."
- **Losing the form on refresh.** → draft + autosave.
- **Numbered badges as decoration.** If the numbers don't refer to a real order that something else (the check card, the step rail) points back at, they're the `01 · 02 · 03` slop from `anti-cheap.md`. Drop them.
- **A form column at full viewport width.** → cap at 720–820px.
- **Brand grammar smuggled in** — grain, vignette, a giant `clamp()` page title, a gradient hero band above the form. → `product-ui.md` §0 substrate; this is a tool, not a landing page.

---

## 9. Workflow Pre-Flight *(in addition to the shared §8 and `product-ui.md` §10)*

- [ ] Form column capped at a readable width; label position (above **or** left) picked once and held.
- [ ] Step rail (if stepped) lives in the topbar, ≤5 steps, completed steps clickable.
- [ ] Every required field marked; every helper line states a **consequence**, not a restatement.
- [ ] Every upload states its spec (format · ratio · size) **before** the user picks a file, and offers a non-local alternative.
- [ ] Consequential exclusive choices use **radio cards with a consequence line**, not a bare radio list.
- [ ] **Pre-submit check card present**, live, showing values (not "OK"), with jump-to-fix links on every failure, and blockers visually distinct from advisories.
- [ ] Any derived number (budget, total, headcount) shows its **breakdown** and every fee's rate.
- [ ] Draft saves — autosave + explicit button + a visible "saved at" timestamp.
- [ ] Commit is confirmed with its **consequence** spelled out; submitting state is inline, not a blocking overlay; success is a page-level confirm with key facts + next action.
- [ ] All five interaction states shipped, including the preview's empty state.
- [ ] Mobile: aside → bottom sheet; three columns never survive to tablet; touch targets ≥44px.
- [ ] Palette from `product-palettes.md`, substrate tinted, no raw hex outside `:root`.
