# The Cheapness Blacklist — Anti-Slop

Merged from production-tested AI tells, absolute design bans, and training-data reflex-reject lists. These are what make AI-generated pages instantly recognizable as cheap. Scan against this list before shipping. Each entry: the tell → what to do instead.

> Philosophy: most of these are *defaults the model reaches for without thinking*. The fix is almost never "add more" — it's "name the reflex, reject it, commit to an intentional choice."

---

## 1. Banned Outright — `[HARD BAN]` (no override, no exceptions)

Failing any item below is a **shipping blocker**. Fix before delivery.

- `[HARD BAN]` **Em-dashes as a design flourish** in copy (kinetic pauses, em-dash bullets, dramatic asides). The single most-violated tell. Use periods, commas, or restructure the sentence. → ban applies to visible copy, not code.
- `[HARD BAN]` **Div-based fake screenshots** — hand-built "product preview" / fake dashboards / fake task lists / fake terminals out of `<div>`s. → use a real screenshot, generate one, build a real mini-component, or use editorial photography.
- `[HARD BAN]` **Gradient text** via `background-clip: text` as a default flourish. → single color, or weight/size emphasis. *(Exception: a deliberate duotone-soul move with the two locked accents.)*
- `[HARD BAN]` **AI-purple/blue glow** — purple gradient buttons, neon mesh backgrounds, glowing CTAs as default. → neutral base + one high-contrast accent. *(Exception: the brand brief explicitly asks for purple, executed with intent.)*
- `[HARD BAN]` **Fake-precise numbers** — `92%`, `4.1×`, `48k`, `5.8mm` invented for spec-aesthetic. → use real data, label as mock, or drop the number.

---

## 2. AI Tells (reject as default; override only with a stated reason)

**Layout & rhythm**
- `[HARD BAN]` **Eyebrow on every section** — the tiny uppercase wide-tracked label (`ABOUT`, `OUR PROCESS`, `THE HARDWARE`) above each headline. Present in 55–95% of AI output. → **max 1 eyebrow per 3 sections.** Usually the headline alone suffices. Mechanical check: count `uppercase tracking` small-caps labels; if `> ceil(sections/3)`, fail.
- **Numbered section markers** — `01 · About / 02 · Process / 03 · Pricing` as default architecture. → only for genuine sequences (a real 3-step process, a timeline). Otherwise drop.
- **Identical card grids** — 6 cards, each icon + title + paragraph, same size. → vary tile sizes, `repeat(auto-fit, minmax(280px,1fr))`, alternate full-width rows, give 2–3 cells real visual variation (image/gradient/pattern).
- **Section-layout repetition** — same layout family twice. → a layout family appears at most twice; ≥4 families on an 8-section page; max 2 consecutive image+text zigzags.
- **Split-header** — big left headline + small right explainer paragraph as a section header. → stack vertically, one focused message; split only when the right column carries a real visual.

**Visual & CSS**
- **Side-stripe borders** — `border-left: 3px` accent stripe on cards. → full border, bg tint, leading icon, or nothing.
- **Glassmorphism as default decoration.** → rare and purposeful only; when used, add a 1px inner border + inner shadow for real edge refraction, and a solid fallback under `prefers-reduced-transparency`.
- **Hero = text + gradient blob.** A blob is not a hero visual. → a real engine (see `hero-engines.md`) or real imagery.
- **Pure `#fff` / `#000`**, hard `#333` borders, untinted black drop shadows on light bg. → tinted neutrals, translucent borders, hue-matched shadows.
- **Mixed corner-radius systems** — round buttons in a square layout, etc. → pick one radius scale and lock it.

**Typography**
- **Reflex-default fonts** reached for blindly: `Inter, Fraunces, Instrument Serif, Playfair, Cormorant, Space Grotesk, Syne, DM Sans, Newsreader, Lora, IBM Plex *, Space Mono`. All good fonts, all over-used. → fine *with a reason*; otherwise rotate to a less-tired sans-display (Geist, Cabinet Grotesk, PP Neue Montreal, GT Walsheim, ABC Diatype).
- **Serif because "creative/premium."** The "creative brief = serif" reflex is a top tell. → default to sans-display; reach for serif only when genuinely editorial/luxury/heritage *and you can name why this serif fits this brand*.
- **Mixed-family emphasis** — injecting a serif word into a sans headline. → italic/bold of the same family.
- **Mono as lazy shorthand for "technical"**, all-caps body copy. → use sparingly and intentionally.

**Color**
- **Beige + brass + espresso** for any craft/heritage/artisan brief. The 2026 saturated default. Banned hex families as default:
  - bg: `#f5f1ea #f7f5f1 #fbf8f1 #efeae0 #ece6db #faf7f1 #e8dfcb`
  - accent: `#b08947 #b6553a #9a2436 #9c6e2a #bc7c3a #7d5621`
  - text: `#1a1714 #1a1814 #1b1814`
  - token names `--cream --sand --bone --paper --flour --linen` are themselves a tell.
  → saturated brand body (terracotta/oxblood/near-black), true off-white (chroma 0), or deep mid-tone. Rotate; never two warm-craft palettes in a row.
- **Accent drift** — a warm-grey site sprouting a blue CTA in section 7. → color lock: one accent owns the whole page.
- **Theme inversion mid-scroll** — a warm-paper section inside a dark page. → one theme, locked (unless a deliberate one-time scroll theme-switch).

**Content & copy**
- **Eyebrow-cluttered, buzzword copy**; cute-but-wrong wordplay; fake-craftsman labels; mock-poetic micro-meta ("elegant nothing"). → re-read every visible string; replace anything broken/unclear/AI-cute with a plain functional sentence.
- **"Jane Doe" placeholder data**, lorem ipsum left in. → realistic, specific content.
- **Long lists / 20-row spec tables with a hairline under every row.** → group into 2–3 chunks, card-per-spec, scroll-snap pills, or "top 5 + view all."
- **Quotes longer than 3 lines**, attribution as name-only (`— Sarah`). → ≤3 lines, name + role + company, real typographic quotes.
- **Duplicate CTA intent** — "Get in touch" + "Contact us" + "Let's talk" on one page. → one label per intent everywhere.

**Imagery**
- **Zero imagery on an image-implied brief** (food, hotel, fashion, travel, product). That's a bug, not minimalism. → real/generated photography; even restrained editorial needs 2–3 real images.
- **Hand-rolled decorative SVG illustrations** as default. → icon libraries for icons; for brand marks, a simple monogram only.
- **Plain text wordmarks** for a "trusted by" logo wall. → real SVG logos (Simple Icons `cdn.simpleicons.org/{slug}`), or a generated monogram for invented brands. Logos only — no category labels under them.

---

## 3. Motion Tells

- **Motion claimed, motion absent** — `SPECTACLE 7` but a static page. → ship working motion or drop the dial.
- **Unmotivated animation** — GSAP everywhere because it's available. → each animation states its reason (hierarchy/story/feedback/state) in one sentence, or it's cut.
- **>1 marquee per page.** → pick the one section the marquee serves; others get a different layout.
- **`window.addEventListener('scroll', …)`** driving animation / React state on every frame. → `ScrollTrigger`, `IntersectionObserver`, Motion `useScroll`, or CSS `animation-timeline: view()`.
- **`power1.out` / linear easing** on reveals (too fast = cheap). → `power3.out` / `cubic-bezier(.16,1,.3,1)` / spring.
- **No `prefers-reduced-motion` fallback.** → mandatory; freeze to a still frame.
- **Janky engine** — particle count too high, FBO resolution too high, cut-off ScrollTriggers. → profile on a mid device; below 50fps, simplify.

---

## 4. The 30-Second Self-Check

Before shipping, answer honestly:
1. **Anti-default named?** Did I identify the lazy aesthetic for this brief and beat it?
2. **One accent, locked?** Same accent every section?
3. **Eyebrow count ≤ ceil(sections/3)?**
4. **≥4 layout families?** No family more than twice?
5. **Real imagery** where the brief implies it?
6. **Spectacle shown** (if claimed)? Holds 60fps? Reduced-motion fallback?
7. **Copy clean?** No em-dashes, no fake numbers, no AI-cute strings?
8. **Contrast AA?** Buttons, placeholders, focus rings included?

Any "no" is unshipped work.
