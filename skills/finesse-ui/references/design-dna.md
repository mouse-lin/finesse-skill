# Design DNA — The Premium Substrate

The physical layer that separates an expensive-looking page from a cheap one. Distilled from a 37-page showcase corpus across every industry. Apply consistently; these are recipes with real values, not inspiration.

> Reason in **OKLCH** internally for palettes; emit hex if the stack needs it. Design light + dark together, test contrast in each, never just invert.

---

## 1. Token Convention

Every page declares a small, named token set. Dark is the default (28 of 37 pages); light is the rarer, harder register.

**Dark page tokens** (typical ranges):
```css
:root {
  --bg:      #04060D;            /* near-black, tinted toward brand hue. Range #020806–#0C0A08 */
  --surface: #0F0F16;            /* one step up from bg */
  --ink:     #EAEDF8;            /* never pure #fff. Range #E8D5B0 (warm) – #F5F5F5 */
  --muted:   #7A7A90;            /* secondary text */
  --dim:     #4A4A5E;            /* tertiary / hairlines */
  --accent:  #6C63FF;            /* THE color. Owns the whole page. */
  --accent2: #FF3B8E;            /* optional second, for duotone souls only */
  --border:  rgba(255,255,255,.07);
}
```

**Light page tokens:**
```css
:root {
  --bg:   #F4F1E9;              /* tinted off-white, never #fff. Range #EFEFED–#F8F6F2 */
  --ink:  #1A1714;             /* near-black, never #000 */
  --muted:#857F73;
  --dim:  #C0C0BE;             /* light hairlines */
  --border: rgba(0,0,0,.07);
}
```

Rules: no `#fff`/`#000` anywhere. Tint neutrals a few points toward the brand hue (add 0.005–0.015 OKLCH chroma). One accent, locked across the whole page.

**Color naming convention (three-part — mandatory for any named token):**
Every color in a finesse system must have all three: `Descriptive name (hex) — functional role and constraint`.

```
Void (#04060D)       — Page background. Near-black tinted toward brand hue.
Star Dust (#EAEDF8)  — Primary text. Slightly cool; never pure white.
Pulse (#6C63FF)      — Accent. Owns the page — no second accent unless duotone.
Iron (#4A4A5E)       — Hairlines and tertiary text only.
```

Never write just a hex, just a name, or just a role. The triple makes tokens self-documenting and prevents silent drift when values change across files.

---

## 2. Grain (mandatory)

A fixed SVG `feTurbulence` noise overlay. Static, ~2KB inline, but it's the single biggest "this isn't flat vector slop" signal.

```css
.grain, body::before {
  content: '';
  position: fixed; inset: 0; z-index: 1; pointer-events: none;
  opacity: .032;   /* .025–.05. Dark pages lower, busy pages higher */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```
- `baseFrequency` `0.75` = fine grain (default), `0.9` = coarse/grittier.
- `numOctaves` `2` = light, `4` = richer texture.
- Light pages: keep it, drop opacity to `.02–.03`.

---

## 3. Vignette

Radial darken on dark heroes — creates an optical focal point, guides the eye.

```css
.vig {
  position: absolute; inset: 0; z-index: -1; pointer-events: none;
  background: radial-gradient(ellipse 90% 70% at 50% 46%,
    transparent 46%, rgba(4,6,13,.86) 100%);
}
```
Move the focus `at {x}% {y}%` toward where the hero subject sits.

---

## 4. Typography Tension

The look is **tight, large, confident**. High weight contrast does most of the work.

```css
h1 {
  font-size: clamp(56px, 9vw, 160px);
  letter-spacing: -.045em;        /* negative tracking on display */
  line-height: .9;                /* .86–.95, compressed */
  font-weight: 800;               /* or 900 */
}
body { font-weight: 300; }        /* extreme contrast against the heading */

.label {                          /* small caps metadata */
  font-size: 11px;
  letter-spacing: .22em;          /* wide tracking, the inverse of display */
  text-transform: uppercase;
}
.count {                          /* numbers / metrics */
  font-family: 'JetBrains Mono', monospace;
  letter-spacing: -.04em;
  font-variant-numeric: tabular-nums;
}
```

**Emphasis inside a heading:** use italic or bold of the **same** family. Never inject a random serif word into a sans headline — mixed-family emphasis is amateur.

---

## 5. Font Pairings (soul → type)

| Pairing | Soul / industry | Notes |
|---------|-----------------|-------|
| Inter + JetBrains Mono | tech, fintech, science | the workhorse; mono carries labels/metrics |
| Cormorant Garamond + Inter + JetBrains Mono | luxury, fragrance, watches | high-contrast classical serif, italic for emphasis |
| Playfair Display + Spectral + Inter | editorial, film, journals | display serif + text serif; pairs with grayscale photography |
| Fraunces / EB Garamond + Inter | heritage, coffee, whisky, fine dining | warm editorial weight (note: Fraunces is over-used — justify it) |
| Anton + Inter | fashion week, music, culture | super-heavy display, mix-blend tricks |
| Bebas Neue + Barlow / Barlow Condensed | sport, alpine, bold campaign | condensed extreme-weight contrast |
| Raleway 100–900 + JetBrains Mono | architecture, minimal studio | one family across a huge weight range |

> `Inter` and `Fraunces` are excellent but over-defaulted. Use them with a stated reason, not reflexively. Sans-display (Geist, Cabinet Grotesk, PP Neue Montreal, Space Grotesk) is a strong, less-tired default for "premium modern."

---

## 6. Palette Families (soul → color)

| Family | Bg | Accent(s) | Souls |
|--------|----|-----------|-------|
| Cinematic cool | `#04060D` | cyan `#22e3ff` / magenta `#ff2d8e` | astronomy, AI, crypto, music |
| Phosphor mono | `#020806` | neon-green `#00DC50` only | quant, security, terminal |
| Warm heritage | `#070402` | amber `#C87820` + ember `#FF6820` | whisky, coffee, fire, craft |
| Gold luxury | `#0C0A08` | gold `#B8922A` + `#E8C870` | watches, fragrance, fine dining |
| Earth | `#2A1B11` | terracotta `#C0562F` + olive `#6E6A3E` | coffee, organic, artisan |
| Editorial light | `#F8F6F2` | rust `#A8331F` (single) | magazine, film, publication |
| Quiet luxury light | `#F0F0EE` | sage `#2D5A3D` (single) | architecture, hotels, wellness |

Strategy ladder (pick one, commit): **Restrained** (accent ≤10%) → **Committed** (one color 30–60%) → **Full** (3–4 named roles) → **Drenched** (the surface IS the color). Restrained suits product; Committed/Drenched suit brand heroes.

> **Beige+brass+espresso is the saturated AI default of 2026.** Token names like `--cream / --sand / --bone / --paper` are themselves a tell. For a "warm, traditional" brief, reach for a saturated brand body (terracotta, oxblood, near-black), a true off-white (chroma 0), or a deep mid-tone — not near-white warm-tinted cream.

---

## 7. The High-Craft Detail Kit

What manufactures "premium," beyond layout:

- **Layered depth** — `engine(z0) · grain(z1) · vignette · content(z5)`. Never one flat plane.
- **Tinted shadows** — `box-shadow: 0 0 80px rgba(0,0,0,.8)`; on light pages tint the shadow to the bg hue, never pure black.
- **Translucent borders** — `rgba(255,255,255,.07–.22)` / `rgba(0,0,0,.06–.08)`. Hard `#333` lines read cheap.
- **Backdrop blur** — `backdrop-filter: blur(8px)` on nav and glass chips (sparingly, with a 1px inner border for real edge refraction).
- **Generous rhythm** — `section { padding: 120px 52px; max-width: 1280px; margin: 0 auto; }`. Symmetric gutters, centered axis.
- **Grayscale photography** — `filter: grayscale(1) contrast(1.06)`, reveal color on hover for editorial souls.
- **Custom easing** — hover `transform: scale(1.04)` (not 1.1) with `cubic-bezier(.16,1,.3,1)`. Subtlety reads as expensive.
- **Reveal stagger** — `opacity 0→1`, `y 34→0`, `duration .7`, `ease power3.out` (never `power1` — too fast = cheap).
- **Gradient text** — allowed ONLY as a deliberate duotone soul move (`background-clip: text` with the two locked accents), never as a default flourish.

---

## 8. Minimal Boilerplate

```html
<!doctype html>
<html lang="zh">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap');
  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
  :root { --bg:#08080C; --ink:#EEEEF5; --muted:#7A7A90; --dim:#4A4A5E; --accent:#6C63FF; --border:rgba(255,255,255,.07); }
  html { scroll-behavior:smooth; }
  body { background:var(--bg); color:var(--ink); font-family:'Inter',system-ui,sans-serif; -webkit-font-smoothing:antialiased; overflow-x:hidden; }
  body::before { content:''; position:fixed; inset:0; z-index:1; pointer-events:none; opacity:.032;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E"); }
  .content { position:relative; z-index:5; }
  @media (prefers-reduced-motion: reduce) { *{animation:none!important;transition:none!important;} canvas{opacity:.55;} }
</style>
</head>
<body><div class="content"><!-- hero + sections --></div></body>
</html>
```
