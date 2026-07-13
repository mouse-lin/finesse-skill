# Product Palettes — Neutrals, Accents, and Ready-to-Paste Sets

The **product register**'s color library. `design-dna.md` §6 is a *soul → color* map for **brand** pages: a background and an accent, which is all a brand page needs. A dashboard, console, or SaaS app needs more — a **tinted neutral ramp** (which is 80% of the pixels), an accent that survives both light and dark, a soft tint for chips and focus rings, and semantic up/down kept **separate** from the brand.

Four layers, in the order you actually build them:

1. **§2 Neutral ramp** — pick one of 5 tinted families. This is the substrate. Get it right and the page already looks expensive.
2. **§3 Accent library** — pick one (+ an optional partner hue) from 16, with light/dark variants and text-on-accent contrast already worked out.
3. **§4 Curated sets** — 12 complete domain-driven builds, if you'd rather not mix your own.
4. **§5 Known SaaS palettes** — the ones the industry actually uses (Linear, Stripe, Supabase, Grafana…), with an honest note on what borrowing one costs you.

> **Why this file exists.** `product-ui.md` §0.2 bans the reflex `#3B82F6` blue + `#22C55E` green + `#F97316` orange triad — but banning a default without shipping alternatives means the model reaches for it anyway. Pick from here, lock it, and the flat-white-card tell (`product-ui.md` §9) becomes structurally hard to commit.

---

## 1. The token contract

Every palette in this file fills exactly these roles. Nothing in your CSS is a raw hex — it's one of these.

```css
:root{
  /* substrate — from §2 */
  --page:      ;  /* body. NEVER #fff/#000. Tinted toward the accent's hue. */
  --bg:        ;  /* cards & panels sitting on --page */
  --panel-2:   ;  /* insets: icon chips, table headers, code blocks, inputs */
  --border:    ;  /* hairline, 3–8% ALPHA — never a hard #ccc / #333 */

  /* ink — from §2 */
  --ink-1:     ;  /* headings, big numbers */
  --ink-2:     ;  /* body, table cells */
  --ink-3:     ;  /* labels, captions, axis text — still ≥4.5:1 on --bg */

  /* brand — from §3 */
  --accent:    ;  /* ONE. primary button, active nav, key series */
  --accent-2:  ;  /* optional partner hue: 2nd series, 2nd CTA. Omit for a mono set. */
  --accent-soft:; /* 8–16% tint: chips, badges, selected row, focus ring */
  --on-accent: ;  /* text/icon ON the accent fill — white or near-black, see §3 */
  --glow:      ;  /* rgba(accent,.28–.40) — primary CTA / key card ONLY, once per screen */

  /* semantic — functional, NOT the identity */
  --up:        ;
  --down:      ;
  --warn:      ;

  /* elevation — tinted toward the ink, never pure black */
  --shadow:    0 1px 3px rgba(R,G,B,.05);           /* whisper — on every card */
  --shadow-lg: 0 40px 90px -50px rgba(R,G,B,.35);   /* floating shell / modal ONLY */
}
```

Radius tiers are palette-independent — keep `product-ui.md` §0.1's (`--r: 16–22px` · `--r-sm: 11–15px` · `--r-lg: 32px`).

---

## 2. Neutral Ramps — the layer that actually does the work

**A dashboard is mostly grey.** The accent is maybe 5% of the pixels; the neutrals are the rest. A *tinted* ramp is the single highest-leverage move in product UI — and a `#f5f5f5` / `#e5e5e5` / `#737373` pure-grey ramp is the flat, cheap default that `product-ui.md` §9 names.

**Pick the family whose tint matches your accent's hue.** Warm accent → warm ramp. Green accent → sage ramp. This is what "tint the substrate to the brand" means in practice.

### Cool Slate — for blue · indigo · violet · sky accents
```
50 #f6f7f9   100 #eceff3   200 #dde2e9   300 #c4ccd8   400 #97a2b3   500 #6f7c91
600 #566175  700 #454e5e   800 #333a46   900 #23272f   950 #14171c
```

### True Neutral — for any accent; the safe pick when the brand is loud
```
50 #f7f7f6   100 #efefee   200 #e2e2e0   300 #cbcbc8   400 #9d9d99   500 #777773
600 #5c5c59  700 #4a4a47   800 #373736   900 #262625   950 #171716
```

### Warm Stone — for orange · amber · brick · rose · gold accents
```
50 #f8f6f2   100 #f0ece5   200 #e3ddd2   300 #cec5b5   400 #a39885   500 #7f7462
600 #63594b  700 #4f473c   800 #3b352d   900 #29241e   950 #191612
```

### Sage — for green · emerald · teal · lime accents
```
50 #f2f5f2   100 #e7ece7   200 #d6ded6   300 #bcc8bc   400 #8fa192   500 #6a7d6e
600 #536357  700 #435046   800 #333d36   900 #232a25   950 #141815
```

### Lilac — for violet · purple · magenta accents
```
50 #f6f5fa   100 #eeecf5   200 #e0dcee   300 #c8c2de   400 #9d95bb   500 #776f96
600 #5d5678  700 #4b4561   800 #393449   900 #282433   950 #17141e
```

### Wiring a ramp into the token contract

```css
/* LIGHT — the page is step 100, cards are white. The card floats; the page recedes. */
--page:{100}  --bg:#ffffff  --panel-2:{50}   --border:rgba({900-as-rgb},.07)
--ink-1:{950} --ink-2:{700} --ink-3:{500}

/* DARK — the page is 950, panels step UP, never down. */
--page:{950}  --bg:{900}    --panel-2:{800}  --border:rgba(255,255,255,.07)
--ink-1:{50}  --ink-2:{300} --ink-3:{400}
```

Two rules that break most dark modes:
- **Dark panels get *lighter* as they come forward** (page 950 → card 900 → inset 800). Shadow does almost nothing on dark; **lightness** is the depth cue.
- **`--ink-3` on dark is step 400, not 500.** Small grey label text on a dark card is the most-failed contrast check in the register. Verify 4.5:1 against `--bg`, not against `--page`.

---

## 3. Accent Library

Mix any of these with any §2 ramp (respect the hue-match note). Each row gives the **light** value (used on a white/near-white card), the **dark** value (on a `900` panel — always lighter and more saturated; never reuse the light value), and what text color survives on top of the accent fill.

| Accent | Light | Dark | `--accent-soft` | `--on-accent` | Notes |
|--------|-------|------|-----------------|---------------|-------|
| **Indigo** | `#4A46C9` | `#7B77F0` | `rgba(74,70,201,.10)` | white | The SaaS workhorse. Pair with coral, not green. |
| **Violet** | `#5B4BD1` | `#9B85FF` | `rgba(91,75,209,.10)` | white | Flat violet ≠ the banned "AI-purple **glow**". Keep SPECTACLE ≤4. |
| **Purple** | `#7C3F9E` | `#B57BD8` | `rgba(124,63,158,.10)` | white | Deeper, more editorial than violet. |
| **Cobalt** | `#2D5BD8` | `#5E8CFF` | `rgba(45,91,216,.10)` | white | The deliberate platform blue. See §4 Set 9 before using. |
| **Sky** | `#0E7FA8` | `#3FC5E8` | `rgba(14,127,168,.11)` | white | Lighter, friendlier than cobalt. |
| **Teal** | `#176C6B` | `#2FD3C5` | `rgba(23,108,107,.10)` | white | Cool but not blue — the best "not-blue" escape. |
| **Emerald** | `#1E8A5F` | `#3FCB86` | `rgba(30,138,95,.11)` | white | ⚠ Collides with `--up`. Move `--up` to a blue-green, or pick another accent. |
| **Forest** | `#2F6B4F` | `#63C795` | `rgba(47,107,79,.10)` | white | Darker, more "institution" than emerald. Same `--up` caution. |
| **Lime** | `#6F8F0F` | `#B6E018` | `rgba(111,143,15,.13)` | **near-black** | White text on lime fails AA. Always dark text on the fill. |
| **Amber** | `#B77A16` | `#E8A33D` | `rgba(183,122,22,.12)` | **near-black** | Same — white on amber fails AA at body size. |
| **Orange** | `#D9642A` | `#FF8A4C` | `rgba(217,100,42,.11)` | white | Burnt, not safety-cone. The merchant/ops default. |
| **Brick** | `#A8402C` | `#E0705A` | `rgba(168,64,44,.10)` | white | ⚠ Collides with `--down`. See §7. |
| **Coral** | `#DB4B3E` | `#FF6B5A` | `rgba(219,75,62,.10)` | white | ⚠ Same red collision. Best used as `--accent-2`, not `--accent`. |
| **Rose** | `#B4485E` | `#F07C93` | `rgba(180,72,94,.10)` | white | The grown-up alternative to reflex hot-pink. |
| **Magenta** | `#A93D80` | `#E87BC0` | `rgba(169,61,128,.10)` | white | Rare in product UI — that's the point. |
| **Mono** | `#2B3039` | `#D7DAE0` | `rgba(43,48,57,.08)` | white / near-black | No accent at all. Buttons are ink; the *only* color on the page is semantic. Ruthless, hard to do, unmistakable when it lands. |

**Partner hue (`--accent-2`) rule:** if you take one, take it from **across the wheel**, not next door — indigo+coral, teal+clay, violet+amber, green+gold. Two adjacent hues (blue + teal) read as an accident. And a partner hue is for *a second data series or a secondary CTA* — not for decorating a third of the page.

---

## 4. Curated Sets — 12 complete builds

Paste-ready. Each is a §2 ramp + a §3 accent, already collision-checked.

### Set 1 · **Sage Ledger** — forest green on sage paper *(light)*
*Finance ops · tax · procurement · supply chain · B2B back-office.* The non-blue answer to Trust-SaaS.
```css
:root{
  --page:#eef0ec; --bg:#fff; --panel-2:#f3f5f1; --border:rgba(20,32,26,.07);
  --ink-1:#14201a; --ink-2:#3c4a43; --ink-3:#6d7d74;
  --accent:#2F6B4F; --accent-2:#C08A3E; --accent-soft:rgba(47,107,79,.10);
  --on-accent:#fff; --glow:rgba(47,107,79,.32);
  --up:#2C7A8C; --down:#C0453C; --warn:#B77A16;   /* --up moved to blue-green: accent is already green */
  --shadow:0 1px 3px rgba(20,40,30,.05); --shadow-lg:0 40px 90px -50px rgba(20,40,30,.35);
}
```

### Set 2 · **Amber Ops** — burnt orange on warm paper *(light)*
*Merchant consoles · 商家工作台 · e-commerce back-office · logistics · order management.* Warm and operational rather than corporate. The strongest default for a merchant admin.
```css
:root{
  --page:#f4f0e9; --bg:#fff; --panel-2:#faf6f0; --border:rgba(48,32,20,.07);
  --ink-1:#1f1710; --ink-2:#4a3f34; --ink-3:#82715f;
  --accent:#D9642A; --accent-2:#2E6B63; --accent-soft:rgba(217,100,42,.11);
  --on-accent:#fff; --glow:rgba(217,100,42,.34);
  --up:#2F7A55; --down:#C0453C; --warn:#B77A16;
  --shadow:0 1px 3px rgba(60,38,20,.05); --shadow-lg:0 40px 90px -50px rgba(60,38,20,.34);
}
```

### Set 3 · **Rose Clay** — rose + warm gold on blush paper *(light)*
*Beauty · fashion · lifestyle brand back-offices · creator/influencer platforms.*
```css
:root{
  --page:#f5eeee; --bg:#fff; --panel-2:#faf3f3; --border:rgba(48,24,28,.07);
  --ink-1:#231519; --ink-2:#4d3a3e; --ink-3:#87696f;
  --accent:#B4485E; --accent-2:#C1913C; --accent-soft:rgba(180,72,94,.10);
  --on-accent:#fff; --glow:rgba(180,72,94,.30);
  --up:#3B7D5C; --down:#BE4238; --warn:#B77A16;
  --shadow:0 1px 3px rgba(60,28,36,.05); --shadow-lg:0 40px 90px -50px rgba(60,28,36,.32);
}
```

### Set 4 · **Teal & Clay** — teal + terracotta on cool paper *(light)*
*CRM · project/resource tools · SaaS middle-office.* The cool-warm pair means charts never need a random third hue.
```css
:root{
  --page:#e9eeed; --bg:#fff; --panel-2:#f1f5f4; --border:rgba(16,38,38,.07);
  --ink-1:#0f2322; --ink-2:#3a4b4a; --ink-3:#6b7d7c;
  --accent:#176C6B; --accent-2:#C4633F; --accent-soft:rgba(23,108,107,.10);
  --on-accent:#fff; --glow:rgba(23,108,107,.30);
  --up:#2C7A62; --down:#C0453C; --warn:#B77A16;
  --shadow:0 1px 3px rgba(19,61,74,.05); --shadow-lg:0 40px 90px -50px rgba(19,61,74,.34);
}
```

### Set 5 · **Violet Console** — violet on lilac paper *(light)*
*AI platforms · data/ML tooling · automation builders.* This is violet **as a flat product accent on a tinted light page** — not the banned AI-purple *glow on a dark hero* (`anti-cheap.md`). Keep SPECTACLE 1–4 and it stays on the right side of the line.
```css
:root{
  --page:#eeecf7; --bg:#fff; --panel-2:#f5f3fb; --border:rgba(28,22,56,.07);
  --ink-1:#171334; --ink-2:#403a63; --ink-3:#6f6890;
  --accent:#5B4BD1; --accent-2:#D98A3C; --accent-soft:rgba(91,75,209,.10);
  --on-accent:#fff; --glow:rgba(91,75,209,.32);
  --up:#2F7A55; --down:#C0453C; --warn:#B77A16;
  --shadow:0 1px 3px rgba(30,20,70,.05); --shadow-lg:0 40px 90px -50px rgba(30,20,70,.35);
}
```

### Set 6 · **Mint & Lavender** — mint + lavender on pale green paper *(light)*
*Healthcare · wellness · edtech · HR.* Calm and low-arousal. The alternative to the cyan+green everyone lands on.
```css
:root{
  --page:#e9f0ec; --bg:#fff; --panel-2:#f1f7f3; --border:rgba(18,40,32,.07);
  --ink-1:#132420; --ink-2:#3c4e48; --ink-3:#6c7f78;
  --accent:#2E9E7B; --accent-2:#8E86D8; --accent-soft:rgba(46,158,123,.11);
  --on-accent:#fff; --glow:rgba(46,158,123,.30);
  --up:#2C7A8C; --down:#C24C42; --warn:#B77A16;   /* --up moved: accent is green */
  --shadow:0 1px 3px rgba(20,50,40,.05); --shadow-lg:0 40px 90px -50px rgba(20,50,40,.32);
}
```

### Set 7 · **Brick & Olive** — brick red on oat paper *(light)*
*Restaurants · retail POS · hospitality · store/branch management.* Close to signage and print — reads as a tool for a real-world business.
**Collision handled:** the accent is red-family, so `--down` is pushed to a cooler, darker red and `--accent` desaturated. Verify they're still distinguishable side by side (§7).
```css
:root{
  --page:#f3efe6; --bg:#fff; --panel-2:#f9f6ef; --border:rgba(44,30,22,.07);
  --ink-1:#1e1712; --ink-2:#4a3e35; --ink-3:#7f7263;
  --accent:#9C4A38; --accent-2:#5C5A3A; --accent-soft:rgba(156,74,56,.10);
  --on-accent:#fff; --glow:rgba(156,74,56,.28);
  --up:#3E7A52; --down:#C22E22; --warn:#B77A16;   /* --down: pure, saturated red — reads as ALARM, not brand */
  --shadow:0 1px 3px rgba(56,36,24,.05); --shadow-lg:0 40px 90px -50px rgba(56,36,24,.33);
}
```

### Set 8 · **Indigo & Coral** — indigo + coral on lilac-grey paper *(light)*
*Content platforms · campaign/ad consoles · creator marketplaces.* A blue-family set that earns it: coral partner instead of the reflex green+orange, and a tinted page.
```css
:root{
  --page:#edeff8; --bg:#fff; --panel-2:#f4f6fc; --border:rgba(24,28,60,.07);
  --ink-1:#141830; --ink-2:#3c4361; --ink-3:#6b7391;
  --accent:#4A46C9; --accent-2:#FF6B5A; --accent-soft:rgba(74,70,201,.10);
  --on-accent:#fff; --glow:rgba(74,70,201,.32);
  --up:#2F7A55; --down:#DB4B3E; --warn:#B77A16;
  --shadow:0 1px 3px rgba(24,28,70,.05); --shadow-lg:0 40px 90px -50px rgba(24,28,70,.34);
}
```

### Set 9 · **Cobalt Trust** — the deliberate platform blue *(light)*
*Only when the brand or the parent platform mandates blue* — a merchant console inside a blue platform, a bank's internal tool, an enterprise design system you don't own. **Blue is not banned; unexamined blue is.** Take this set and you owe the substrate tint + tinted shadow, or you've shipped the exact flat-white tell `product-ui.md` §9 names.
```css
:root{
  --page:#edf0f6; --bg:#fff; --panel-2:#f4f6fa; --border:rgba(20,32,56,.07);
  --ink-1:#121a2b; --ink-2:#3a465c; --ink-3:#69768c;
  --accent:#2D5BD8; --accent-2:#E08A2E; --accent-soft:rgba(45,91,216,.10);
  --on-accent:#fff; --glow:rgba(45,91,216,.30);
  --up:#2F7A55; --down:#C0453C; --warn:#B77A16;
  --shadow:0 1px 3px rgba(20,34,64,.05); --shadow-lg:0 40px 90px -50px rgba(20,34,64,.34);
}
```

### Set 10 · **Graphite & Lime** — lime on graphite *(dark)*
*Developer tools · CI/CD · infra · logs.* The non-blue dev-tool set. Lime fills take **near-black** text.
```css
:root{
  --page:#0b0c0b; --bg:#141613; --panel-2:#1b1e19; --border:rgba(255,255,255,.07);
  --ink-1:#eef1e9; --ink-2:#b3b9ab; --ink-3:#8a9184;
  --accent:#B6E018; --accent-2:#5FD3B2; --accent-soft:rgba(182,224,24,.13);
  --on-accent:#12140d; --glow:rgba(182,224,24,.30);
  --up:#6FD48A; --down:#FF6B5A; --warn:#E8A33D;
  --shadow:0 1px 3px rgba(0,0,0,.35); --shadow-lg:0 40px 90px -50px rgba(0,0,0,.7);
}
```

### Set 11 · **Midnight & Amber** — amber on midnight *(dark)*
*Treasury · trading · financial dashboards · anything read at 2am.* Amber carries the brand so green/red stay strictly semantic — a red cell always means "down", never "brand".
```css
:root{
  --page:#08090f; --bg:#12141c; --panel-2:#1a1d27; --border:rgba(255,255,255,.07);
  --ink-1:#eceef5; --ink-2:#a8adbd; --ink-3:#848aa0;
  --accent:#E8A33D; --accent-2:#7B8CFF; --accent-soft:rgba(232,163,61,.13);
  --on-accent:#141008; --glow:rgba(232,163,61,.30);
  --up:#3FCB86; --down:#FF5F56; --warn:#E8A33D;
  --shadow:0 1px 3px rgba(0,0,0,.4); --shadow-lg:0 40px 90px -50px rgba(0,0,0,.75);
}
```

### Set 12 · **Steel Cyan** — cyan on cold steel *(dark)*
*Monitoring · IoT · fleet · NOC walls.* High legibility at distance; the accent doubles as the "live" signal.
```css
:root{
  --page:#080b0c; --bg:#111618; --panel-2:#182022; --border:rgba(255,255,255,.07);
  --ink-1:#e8f0f1; --ink-2:#a3b1b4; --ink-3:#828f93;
  --accent:#2FD3C5; --accent-2:#F0A868; --accent-soft:rgba(47,211,197,.13);
  --on-accent:#06110f; --glow:rgba(47,211,197,.28);
  --up:#3FCB86; --down:#FF6257; --warn:#F0A868;
  --shadow:0 1px 3px rgba(0,0,0,.38); --shadow-lg:0 40px 90px -50px rgba(0,0,0,.72);
}
```

> **Any light set goes dark** (and vice versa): keep the hue, re-derive. Page → the ramp's 950; `--bg` → 900; ink inverted through three levels; **accent lifted in lightness** until it clears 4.5:1 on `--bg` (use the §3 dark column). Never just swap black and white — `product-ui.md` §8: *dark mode tested independently, not inverted*.

### 4.A The two-tone shell — light content, dark rail *(the merchant-console default)*

Most 商家后台 / admin back-offices are **light content with a dark sidebar**, not a fully dark app. That's a *shell* decision layered on a *light* set: take any light set, paint the rail from its dark counterpart.

```css
--rail:#141210;               /* near-black, tinted to the SAME hue as --page */
--rail-2:#1d1a16;             /* hover / group header */
--rail-ink:#a89c8e;
--rail-ink-active:#ffffff;
--rail-active:var(--accent);  /* accent fill, or a 3px accent left-bar */
--rail-border:rgba(255,255,255,.06);
```

Three rules: the rail is tinted to the **same hue** as the page (a warm page never gets a cold blue-black rail); the accent appears in the rail **only** on the active item; the rail is the **only** dark surface — don't sprinkle dark cards into the light content.

---

## 5. Known SaaS Palettes — what the industry actually uses

These are the recognizable ones. They're here because they're proven and because you'll be asked for "something like Linear" — not because copying one gives you a soul. **Borrowing a known identity is a legitimate choice for an internal tool and a weak one for a product with its own brand.** If you take one, say so out loud (SKILL.md §0.D) rather than drifting into it.

| Look | Substrate | Accent | Reads as | The tax |
|------|-----------|--------|----------|---------|
| **Linear** | near-black `#08090A`, panels `#141516`, hairlines at 6% | violet `#5E6AD2` | precise, keyboard-first, engineered | Everyone in dev-tools now looks like this. Instantly derivative. |
| **Stripe** | white + cool-slate ramp, generous whitespace | indigo-violet `#635BFF` | trustworthy, financial-grade | The whitespace is the point — copy the color without the density discipline and you get a bland form. |
| **Supabase** | near-black `#1C1C1C`, `#2A2A2A` panels | emerald `#3ECF8E` | open-source, developer-native | Green-on-black is crowded (Vercel, Supabase, every terminal). |
| **GitHub Dark** | `#0D1117` page, `#161B22` panels | blue `#2F81F7` | familiar, utilitarian, zero-friction | Maximum familiarity, zero distinctiveness. Sometimes exactly right. |
| **Grafana** | `#111217` page | orange `#F46800` | ops, alerting, live | Orange-on-dark is strongly owned by Grafana in the monitoring space. |
| **Datadog** | white content, purple chrome | purple `#632CA6` | enterprise observability | Heavy chrome; borrow the color, not the density. |
| **Notion** | warm paper `#F7F6F3`, ink `#37352F`, near-zero accent | none (ink is the accent) | calm, document-first, quiet | Hard mode: with no accent, hierarchy must come from type and spacing alone. Beautiful when it lands, mush when it doesn't. |
| **Vercel / mono** | tinted white `#FAFAFA` ↔ tinted black `#0A0A0A` | none — pure contrast | ruthless, confident | Our craft floor forbids literal `#fff`/`#000` — tint both ends a few points. See §3 "Mono". |
| **Intercom / HubSpot** | white + warm grey | orange `#FF7A59` / `#FF5C35` | friendly, sales-and-support | Very close to the banned reflex orange — pull it darker/burnt (`#D9642A`) to escape. |
| **Figma-ish multi** | white, light grey chrome | blue + purple + red + orange as *functional* colors | creative tool, multi-tenant | Multi-accent only works when each color **means** something (a tool, a user, a layer type). As decoration it's slop. |

**The pattern under all of them:** one accent, an aggressively tinted neutral ramp, hairline borders, and near-zero decoration. That structure — not the specific hue — is what makes them read as expensive. You can hit it with any accent in §3.

---

## 6. Charts get their own ramp

`--accent` + `--accent-2` cover 1–2 series. Beyond that, **derive from the accent's hue** — don't reach for a rainbow (`product-ui.md` §3: ≤5–6 colors, no pure red/green pairing).

- **Categorical (≤6):** accent → accent-2 → rotate hue in even steps **at constant lightness**, so no series reads as more important than another. Reason in OKLCH, emit hex.
- **Sequential** (heatmap, intensity): one hue, 5–7 lightness steps.
- **Diverging** (above/below target): `--down` … neutral … `--up`. **Never** the brand accent — a diverging scale *means* something.
- **Never** let `--up`/`--down` appear as a categorical series color. If green means "growth" in a KPI chip and also "Region B" in the bar beside it, the page is lying.

---

## 7. Collision Check — 30 seconds, before shipping

- [ ] **`--accent` doesn't collide with `--up`/`--down`.** Green accents fight `--up`; red/brick/coral accents fight `--down`. **Semantics are load-bearing — move the accent, not the semantic** (desaturate it, or shift `--up` to a blue-green as Sets 1 and 6 do). Put a KPI delta chip next to the primary button and look: if you can't tell "brand" from "went up" at a glance, it's broken.
- [ ] **`--ink-3` clears 4.5:1 on `--bg`.** The most-failed check in the register — small grey label text on a white card, or on a dark panel.
- [ ] **`--accent` clears 3:1 on `--bg`** if it carries meaning (a data series, an active nav state), and **`--on-accent` clears 4.5:1 on the accent fill**. Lime and amber fills need near-black text; everything else in §3 takes white.
- [ ] **`--border` is an alpha, not a hex grey** — it has to sit correctly on both `--bg` and `--panel-2`.
- [ ] **`--glow` appears at most once per screen.**
- [ ] **No raw hex outside `:root`.** A stray `#3B82F6` from a pasted component is exactly how the generic triad creeps back in. Grep for it.

---

## 8. Quick map — domain → set

| Domain | Set | The default you're beating |
|--------|-----|---------------------------|
| Merchant / e-commerce back-office · 商家工作台 | **2 Amber Ops** (3 Rose Clay if the brand is beauty/fashion) | platform blue |
| Finance ops, tax, procurement, B2B admin | **1 Sage Ledger** | Trust-SaaS blue |
| Beauty, fashion, lifestyle, creator platforms | **3 Rose Clay** | hot pink |
| CRM, project/resource, SaaS middle-office | **4 Teal & Clay** | the blue+green+orange triad |
| AI / ML / automation platform | **5 Violet Console** | purple *glow* on dark (banned) |
| Health, wellness, edtech, HR | **6 Mint & Lavender** | clinical cyan |
| Restaurant, retail POS, hospitality | **7 Brick & Olive** | generic safety-cone orange |
| Content / campaign / ad console | **8 Indigo & Coral** | plain indigo, no partner hue |
| Brand or parent platform mandates blue | **9 Cobalt Trust** | *itself* — pay the substrate-tint tax |
| Dev tools, CI/CD, infra, logs | **10 Graphite & Lime** | VS Code blue-grey / Linear violet |
| Treasury, trading, financial (dark) | **11 Midnight & Amber** | green-on-black terminal cliché |
| Monitoring, IoT, fleet, NOC | **12 Steel Cyan** | the same green-on-black |
| "Make it look like {Linear/Stripe/Notion}" | **§5** | nothing — but name the borrow out loud |

**Rotate, don't repeat** (SKILL.md §2): if the last product page used Amber Ops, this one doesn't — unless a `PRODUCT.md` locks it.
