# Style Personas — Industry → Soul

finesse's value is **soul diversity**: the same method must produce visually unrelated pages for different briefs. This is the lookup that prevents every page collapsing into one default aesthetic. Pick the closest persona, then bend it with the brief — never ship the row verbatim.

> Process: (1) name the industry, (2) name the *lazy default* for that industry, (3) pick a persona that beats it, (4) lock palette + type + accent, (5) choose the hero engine that fits.

---

## The Persona Matrix

| Persona | Fits | Palette family | Type pairing | Hero engine | Signature effect |
|---------|------|----------------|--------------|-------------|------------------|
| **Cinematic Tech** | astronomy, AI, neural, crypto, deep-tech | cinematic cool (cyan/magenta on near-black) | Inter + JetBrains Mono | A · Three.js particles | additive-blend glow, scroll-coupled camera |
| **Phosphor Terminal** | quant/fintech, security, infra, dev-tools | phosphor mono (single neon-green) | JetBrains Mono-forward + Inter | B · Canvas data viz | CRT scanlines, flicker, live ticker |
| **Editorial Publication** | magazine, film, journal, photography | editorial light (cream/ink) | Playfair + Spectral + Inter | D · GSAP scroll-reveal | grayscale photography, ruled hairlines, drop-cap |
| **Warm Heritage** | whisky, coffee, craft, distillery | warm heritage (amber/copper/ember) | Fraunces / EB Garamond + Inter | B · Canvas fire/particles | ember particles, letterpress weight, vignette |
| **Gold Luxury** | watches, fragrance, fine dining, jewelry | gold luxury (dual-gold on near-black) | Cormorant Garamond + JetBrains Mono | D · GSAP scrub + parallax | gold gradient text (duotone), slow reveals |
| **Brutal Typographic** | fashion week, music, culture, streetwear | bone/black + one hot accent | Anton / Bebas + Inter | E · CSS mix-blend / D · GSAP | oversized type, `mix-blend-mode: difference`, outline+fill |
| **Quiet Luxury Minimal** | architecture, hotels, wellness, design studio | quiet light (off-white/forest sage) | Raleway 100–900 + JetBrains Mono | E · CSS mask/parallax | dual-layer reveal, extreme weight range, max whitespace |
| **Organic / Botanical** | floristry, skincare, food, sustainability | earth + blush, lighter | Spectral + Inter | B · Canvas organic particles | soft particle drift, grayscale-to-color hover |
| **Electric Nightlife** | clubs, festivals, gaming, esports | duotone neon on black | Anton / Space Grotesk + mono | A/B · particles + glitch | glitch, japanese-char rain, neon bloom |
| **Scientific Emergence** | research, simulation, generative art, data-art | violet/cyan on deep black | Space Grotesk + JetBrains Mono | C · WebGL FBO | fluid / reaction-diffusion / ray-march |

---

## Aesthetic Lanes That Are Saturated (earn them or avoid)

These are not banned, but they are the *training-data defaults*. If you reach for one without a stated reason, the brand becomes invisible. Rotate; never ship the same lane twice in a row.

- **Editorial-typographic** — display serif (often italic) + small mono labels + ruled separators + monochrome restraint + no imagery. Every Stripe-adjacent and Notion-adjacent brand lives here now. Beat it unless the brief is *genuinely* a publication.
- **Beige-brass craft** — warm cream bg + brass/clay/oxblood accents + espresso text. The reflexive reach for any "artisan/heritage/cookware" brief. (See `anti-cheap.md` for banned hex families.)
- **AI-purple glow** — purple/blue gradients, glowing buttons, neon mesh backgrounds. The reflexive reach for any "AI/SaaS" brief.

**Anti-default move:** for each brief, write one line — *"The default for {industry} is {lane}. I'm rejecting it for {chosen persona} because {reason}."*

---

## Color Strategy Ladder

Pick one level and commit the whole page to it:

1. **Restrained** — tinted neutrals + accent ≤ 10%. (product surfaces, quiet luxury)
2. **Committed** — one saturated color at 30–60%. (most brand heroes)
3. **Full** — 3–4 named color roles, used deliberately. (campaigns, festivals)
4. **Drenched** — the surface *is* the color. (brand-hero, launch, maximal)

`SOUL` dial maps roughly: 1–3 → Restrained, 4–6 → Committed, 7–10 → Full/Drenched.

---

## Type Selection Rules

- **Display weight contrast is the cheapest premium signal.** Pair an extreme weight (800/900) against a light body (300). Same family or a deliberate pairing — never random.
- **Scale ratio ≥ 1.25** between type steps. A flat 1.1 ladder reads as "default browser sizes" = cheap.
- **Body line length 65–75ch.** `text-wrap: balance` on headings, `pretty` on paragraphs.
- **Mono for data/labels** (metrics, timestamps, code, eyebrows) — but obey the eyebrow cap (§ anti-cheap).
- **Reflex-reject before choosing:** `Inter, Fraunces, Instrument Serif, Space Grotesk, Syne, DM Sans, Playfair, Cormorant` are all fine fonts and all over-defaulted. Using one is OK *with a reason*; reaching for one blindly is the tell. When in doubt, rotate to a less-tired sans-display (Geist, Cabinet Grotesk, PP Neue Montreal, GT Walsheim).
