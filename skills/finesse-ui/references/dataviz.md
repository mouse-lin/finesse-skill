# Data Visualization — Selection, Accessibility, Libraries

Deep material behind `product-ui.md` §3. Load this when the brief needs more than the 9-row starter table — a dashboard with 5+ distinct chart types, an unfamiliar data shape, or an a11y-sensitive product (gov/healthcare/finance). **Product register by default** — SPECTACLE stays low, charts serve clarity, not decoration. A brand page that wants one animated stat/counter moment is Hero Engine territory (`hero-engines.md`), not this file: this file governs pages where **the data itself is the content**.

> Still inherits the substrate (`design-dna.md`) and the blacklist (`anti-cheap.md`). A chart can be dense and correct without being ugly — but never decorate over the data.

---

## 1. Selection Matrix (25 types, grouped)

Pick by the question the data answers, never by what looks striking. Each row: what it's for, when to reach past it, and how the render strategy changes with volume.

### Trend & time

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Line** | continuous trend, rate of change over time | <4 points (use a stat card); >6 series (use small multiples) | <1k pts: SVG · ≥1k: Canvas + downsample · >10k: aggregate to intervals |
| **Area (single/stacked)** | trend + magnitude together | proportion-over-time with >5 series (switch to 100% stacked) | same as line |
| **Streaming area** | live ops/IoT monitoring, ≥1Hz updates | update rate <1/min (use periodic line) | Canvas/WebGL; buffer last 60–300s, downsample on scroll; **pause/resume control mandatory** |
| **Line + confidence band** | historical + model forecast, non-technical audience | no historical baseline; confidence too low to be useful | keep history 30–90 days, forecast horizon ≤30% of visible range |
| **Line + anomaly markers** | monitoring for outliers/spikes | anomalies are predefined categories (use highlighted bar) | mark anomalies as a separate data layer, not a color swap |

### Compare & rank

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Bar (vertical/horizontal)** | discrete category comparison, ranking | >15 categories (horizontal + scroll, or paginate) | <20: vertical · 20–50: horizontal · >50: paginated table |
| **Radar / spider** | one entity across a fixed attribute set (feature comparison) | >8 axes; precise reading needed (use grouped bar instead) | 2–3 datasets max, 5–8 axes; always ship a grouped-bar alternative |
| **Bullet (compact)** | 3–10 KPIs in a dashboard grid, space-constrained | single KPI with emphasis (use gauge) | scales to any count in a grid layout |
| **Waterfall** | additive components building to a total (P&L, variance) | changes aren't additive; >12 bars (roll minor items into "Other") | 4–12 bars optimal |

### Part-to-whole & proportion

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Pie / donut** | ≤5 categories, one dominant segment | >5 categories; slices within 5% of each other (indistinguishable) | max 6 slices, else 100% stacked bar |
| **Waffle / pictogram** | percentage-of-whole, more visually engaging + more accessible than pie | >5 categories; exact values matter more than impression | 10×10 grid standard |
| **Sunburst** | nested proportions where hierarchy AND size both matter | >3 levels deep; mobile | filter to top-N before rendering past 500 nodes |

### Distribution & correlation

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Scatter / bubble** | relationship between two continuous variables, clusters/outliers | variables are categorical (use grouped bar); <20 points | <500: SVG · 500–5k: Canvas @ .6–.8 opacity · >5k: hexbin/aggregate |
| **Box plot** | spread, median, outliers across groups | <20 points per group (distribution isn't meaningful yet) | any sample size — it's already aggregated |
| **Heat map** | intensity/density on a 2D grid, time×category patterns | <20 cells (use bar); exact values must be read (use table) | up to ~10k cells; calendar heatmap caps at 365 |

### Hierarchy & flow

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Treemap** | proportional structure within a hierarchy (budget breakdown) | depth >3 levels; siblings need precise comparison | <200 nodes SVG, 200–1k Canvas, else pre-filter |
| **Sankey** | quantity flow between named nodes, multi-source/target | flow forms loops (use network graph); <3 pairs | <50 flows SVG, ≥50 Canvas, >200 aggregate minor flows |
| **Funnel** | sequential conversion / drop-off across defined stages | stages non-sequential, values don't monotonically decrease | 3–8 stages optimal; group the tail into "Other" |
| **Decomposition tree** | drilling a metric into contributing factors (BI/root-cause) | no causal parent-child relation; audience wants a summary not exploration | ≤5 levels deep, ≤20 visible nodes/level, lazy-load deeper |
| **Network graph** | mapping connections/topology between entities | >500 nodes without pre-clustering; precise counts needed | ≤100 SVG, 101–500 Canvas, >500 requires clustering/LOD first |

### Geographic & spatial

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Choropleth / bubble map** | regional/location distribution is the core insight | regions vary wildly in size (misleads visually — use bar instead) | <1k regions SVG, ≥1k Canvas/WebGL, global = tile-based |
| **3D scatter / surface** | scientific/engineering data where the Z-axis is essential | a 2D projection says the same thing; mobile; any standard product dashboard | WebGL required; LOD past 50k points |

### KPI & specialized

| Chart | Best for | Avoid when | Volume / render |
|-------|----------|-------------|------------------|
| **Gauge** | one KPI vs. one target/threshold, dashboard summary | no benchmark exists; 3+ KPIs at once (use bullet grid) | one metric per gauge |
| **Candlestick / OHLC** | financial time-series, trading context only | non-financial audience (use line) | Canvas for real-time; paginate historical by range, ≤500 candles visible |
| **Word cloud** | exploratory NLP/sentiment overview, frequency-weighted keywords | precise values matter; corpus <50 items; screen-reader context | 50–5000 terms; top-N filter beyond that |
| **Process map** | event-log analysis, bottleneck/deviation detection | no event-log data; audience wants a static flowchart | <30 nodes SVG, 30–100 Canvas, >100 filter to top variants |

---

## 2. Accessibility Grade & Mandatory Fallback

A grade, **not** a WCAG conformance level — this rates how accessible the chart *type* is structurally, before you've done any work. Chart color contrast still separately meets the AA floors in `design-dna.md` §1.

- **Robust** — reads fine for screen readers/colorblind users once you label it. Table fallback is a nicety.
- **Conditional** — accessible if you add one specific mitigation (shape/pattern, not color alone).
- **Fragile** — structurally hard; a **table/list fallback is mandatory**, not optional.
- **Never primary** — cannot be made accessible as the sole representation; ship it only as a supplementary view alongside a table.

| Chart | Grade | Mandatory fallback |
|-------|-------|---------------------|
| Line / area | Robust | Differentiate series by line-style, not color alone; data table togglable |
| Streaming area | Conditional | Pause/resume control; current value as large text KPI; `prefers-reduced-motion` freezes it |
| Line + confidence band | Conditional | Toggle actual/forecast; legend must name the line style, not just the color |
| Line + anomaly markers | Conditional | Shape marker (not color) for anomalies; text annotation per event |
| Bar (any orientation) | Robust | Value labels visible by default, not hover-only |
| Radar / spider | Conditional | Grouped-bar alternative is mandatory, not optional, past 5 axes |
| Bullet (compact) | Robust | Values always visible as text; range labeled with threshold text, not color position |
| Waterfall | Conditional | Directional icon per bar, not color-coded direction alone |
| Pie / donut | **Fragile** | Percentage data table + stacked-bar alternative are mandatory in any a11y-first context |
| Waffle / pictogram | Conditional | `aria-label` per cell; percentage text always visible (better baseline than pie) |
| Sunburst | **Fragile** | Collapsible indented list with percentages + breadcrumb of current drill-down |
| Scatter / bubble | Conditional | Data table alternative; combine color + shape per group |
| Box plot | Robust | Stats summary table (min/Q1/median/Q3/max); outlier count in the subtitle |
| Heat map | Conditional | Numeric value on hover + a downloadable row/column table |
| Treemap | **Fragile** | Collapsible tree table as the primary view; treemap as a supplementary visual only |
| Sankey | **Fragile** | Flow table (source → target → value); keyboard-traversable node list |
| Funnel | Robust | Explicit conversion % as text per stage; linear list view as fallback |
| Decomposition tree | Conditional | Keyboard expand/collapse; screen reader announces node value + % contribution |
| Network graph | **Never primary** | Adjacency-list table always ships alongside; tree view when structure allows |
| Choropleth / bubble map | Conditional | Text labels for major regions; keyboard navigation between regions |
| 3D scatter / surface | **Never primary** | Mandatory 2D projection + data table; do not ship as the only view in product UI |
| Gauge | Robust | Numeric value + % of target as visible text, not color position alone |
| Candlestick / OHLC | Conditional | OHLC data table; fill-vs-outline distinguishes bullish/bearish, not color alone |
| Word cloud | **Fragile** | Sortable frequency+sentiment list ships as the primary output; cloud is supplementary |
| Process map | Conditional | Path-summary table (variant → frequency → duration); top-3 bottlenecks as text |

---

## 3. Library Recommendation

Pick by **rendering approach**, not by chart name — most chart types share a library once you know which bucket they're in. finesse pages often ship as a single static file (`design-dna.md` §8 boilerplate); default to a CDN-loadable library unless the brief is already inside a React/component app.

| Bucket | Chart types | Vanilla / CDN | Component framework |
|--------|-------------|----------------|----------------------|
| **Declarative, config-first** (the majority) | line, area, bar, pie/donut, radar, waterfall, gauge, bullet, box plot | Chart.js, ApexCharts, ECharts | Recharts, Nivo (React) |
| **Custom / full control** | treemap, sunburst, sankey, network graph, decomposition tree, process map | D3.js directly | D3.js (same — these rarely have a good off-the-shelf component) |
| **Real-time / streaming / financial** | streaming area, candlestick/OHLC, anomaly monitoring | Lightweight Charts (TradingView), CanvasJS | same |
| **Geographic** | choropleth, bubble map | D3-geo, Leaflet | Mapbox GL; Deck.gl once past ~1k regions or point-cloud scale |
| **3D / spatial** | 3D scatter/surface | Three.js | Three.js + react-three-fiber if already in a React app |
| **Text-derived** | word cloud | D3-cloud | Nivo, Highcharts wordcloud module |

**Rule of thumb:** don't reach for D3 raw unless the chart is in the "custom / full control" bucket — for the declarative bucket it's more code for the same result, and finesse's own philosophy (craft, not needless complexity) says use the tool that gets you there with less surface area to get wrong.

---

## 4. Color & Motion (extends `design-dna.md`)

- **No generic chart-library defaults.** Derive the categorical/sequential/diverging scale from the page's **locked accent** (§3 Color lock in `SKILL.md`), not the library's default blue/orange/green — a chart in the brand's own palette reads as designed, not templated.
- **Never color alone.** Every legend/status/series distinction pairs color with a second channel: line-style, shape, direct label, or pattern fill. This is the throughline across nearly every "Conditional"/"Fragile" row in §2 above.
- **Diverging scales** (heat maps, anomaly bands, financial red/green): keep both ends readable for red-green colorblindness — pair with a shape or explicit +/- sign, don't rely on hue alone even with a "safe" diverging palette.
- **Real-time/streaming charts inherit the engine discipline** from `SKILL.md` §4: `prefers-reduced-motion` freezes the chart to its current frame or swaps to a static last-known-value card; a live chart is not exempt from the reduced-motion mandate just because it's "just data."

---

## 5. Anti-Patterns (add to `product-ui.md` §9 / `anti-cheap.md`)

- **3D pie/donut** — always banned, no exception (worse-than-2D on every axis: reading and a11y both).
- **Pie with >5 slices** used as the primary (not supplementary) view.
- **Color as the only signal** anywhere in a chart — status, series, or legend.
- **Div-based fake charts/dashboards** — same ban as fake screenshots in `anti-cheap.md`; if there's no real data, don't fake a chart shape with static bars.
- **Word cloud as the sole output of an NLP feature** — always ship the ranked list.
- **A blank chart with no "no data" state** — same rule as an empty table (`product-ui.md` §2).

---

## 6. Product Pre-Flight Addendum

Add to `product-ui.md` §10:

- [ ] Chart type matches the question (§1), not just visual preference.
- [ ] Chart's a11y grade (§2) checked — if Fragile/Never-primary, the mandatory fallback ships alongside, not instead of.
- [ ] Colors derived from the locked accent, never a library default; no color-only distinctions.
- [ ] Any real-time chart has pause/resume + respects `prefers-reduced-motion`.
