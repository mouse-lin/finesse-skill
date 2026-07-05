# Inspiration Catalog — The Other 48 Pages

`examples/EXAMPLES.md` bundles 5 full HTML pages as read-only reference. The showcase corpus they're drawn from has grown to **53 pages** — this file catalogs the other 48 as **technique notes, not files**. No HTML is copied here (some of these pages carry Three.js/WebGL payloads that would bloat the skill for every install); instead each entry distills the *industry, engine, and one distinctive move* so you have a wider menu to pull from than the 5 deep-dives + the persona table alone.

**How to use this:** when `references/style-personas.md` gives you a persona, scan its section below for a page that solved a similar brief — steal the *move*, not the markup (you don't have the markup). When a brief doesn't map cleanly to any of the 10 personas, the entries flagged "own lane" below show it's fine to not fit the table.

Engine letters match `references/hero-engines.md`: **A** Three.js+GLSL · **B** Canvas 2D · **C** WebGL FBO shader · **D** GSAP ScrollTrigger · **E** CSS-only. A few newer pages reach for a library outside that list (anime.js, Motion) — flagged inline. Treat them as Engine D variants: same progressive-enhancement / reduced-motion / one-engine-per-page rules apply regardless of which library drives it.

---

## Cinematic Tech

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| helios | 深空天文 | A | 22K 粒子星系，冷静克制的尺度感——粒子数多但不喧宾夺主 |
| nexus | 分布式智能网格 | A | 粒子网络球 + 轨道环，节点连线随相机轻微漂移 |
| eclipse | 数据引力平台 | A | 黑洞引力盘 + 极向喷流，用物理隐喻讲抽象产品概念 |
| atlas | 全球节点网络 | B | 3D 地球 + 弧线路由动画，Canvas 伪 3D 不用 Three.js 也能有全球感 |
| volt | 纯电汽车 | D | 计数器 + 速度表数据可视化，GSAP 驱动的"仪表盘式" hero |
| cortex | AI 编码 Agent | B+D | Canvas 透视网格光带 + 假终端会话（打字机+扫描线），冷蓝/青双色锁定 |
| phantom | 电动 hypercar | B | 速度线随滚动速度实时增强（非固定动画），配合多层视差和 HUD 遥测数字 |

## Phosphor Terminal

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| cipher | 网络安全 / 零信任 | B | Matrix 字符雨 + terminal 美学，单一荧光绿贯穿全站 |

## Editorial Publication

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| lumen | 摄影工作室 | D | 黑白叙事 + 视差，克制的灰阶摄影排版 |
| harvest | 米其林中餐厅 | D | EB Garamond 大字重排版张力全场最强，菜单用四列 grid 完整还原节制感 |
| stratum | 影像档案馆 | E | 纯 CSS 3D 无尽横向长廊，滚动速度=巡游速度的体感映射，零依赖 ~12KB |
| solstice | 时装 SS26 系列 | E | 从零手写的滚动锁定横向 lookbook（Apple 式横向 scroll-jack），无任何库 |

## Warm Heritage

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| terra | 精品咖啡焙煎所 | D | 横向滚动叙事，是这条 persona 里最克制的一版 |
| oxide | 威士忌蒸馏厂 | B | 火焰粒子随生命周期变色（橙红→琥珀），Spectral 衬线带印版质感 |

## Gold Luxury

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| aura | 高级香氛 | D | 奢华排版 + scrub 滚动节奏 |
| apex | 顶级机械表 | B+D | 实时表盘 + 齿轮动画，Canvas 绘制精密感 |
| noma | 精密音响品牌 | B | 声学同心环 + 波形 hero，非仿产品照，靠图形语言表达"音频" |
| caliber | 瑞士机械制表 | B(anime.js) | 程序化 SVG 机芯图——齿轮系/发条阿基米德螺线用数学生成，唯一用 anime.js 而非 GSAP 的页面 |
| lumina | 光影摄影工作室 | D(Motion) | 滚动速度驱动的 marquee + 分栏视差画廊（不同列速度不同），唯一用 Motion 库的页面 |
| obsidian | 高端耳机品牌 | B | 滚动驱动的"爆炸视图"产品分解动画（Apple 风格），纯 vanilla JS 数学实现，无 GSAP |

## Brutal Typographic

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| kinetic | 动态设计工作室 | D | 横向滚动 + 动态排版，字体本身就是视觉主角 |
| riso | 独立创意厂牌/限量印本 | E | 纯 CSS 模拟孔版印刷（halftone 圆点 + 套色错位重影），零图片即可有真实印刷质感 |

## Quiet Luxury Minimal

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| monolith | 建筑事务所 | D | 极简视差，字体留白说话 |
| eden | 隐秘度假村 | D | Clip-path + 横向滚动的克制奢华 |
| meridian | 精品酒店集团 | D | 三处酒店分屏 alternating 布局，浅色主题里内容量最饱满的一版 |
| zenith | 高山滑雪俱乐部 | B | 雪花物理粒子（260个，独立速度+摇摆相位），浅色页面里最清爽的一版 |
| atelier | 极简器物品牌 | E | 纯排版 + 大留白，单色克制到刻意回避米色路线 |

## Organic / Botanical

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| bloom | 精品花艺工坊 | B+D | 有机粒子 + 浅色，浅色 persona 里技术含量最高的一版 |
| ripple | 睡莲静水浴礼 | B | Canvas 水面高度场模拟（双缓冲传播+折射偏移），点击激起真实涟漪扭曲 |
| serum | 纯净护肤仪式 | D | 真实产品摄影 + 环境光斑漂移，完全靠摄影驱动而非生成式图形 |
| petal | 花艺电商 | B | 落瓣粒子叠加真实黑底玫瑰摄影，简单但有效的"给素材加动感"范例 |

## Electric Nightlife

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| sonar | 电子音乐厂牌 | B | 声波可视化 + 霓虹，节奏感强 |
| forge | 游戏工作室 | A | 粒子火焰，Three.js 用在"能量感"而非纯写实 |
| neon | 赛博夜场 | B | 日文字符雨 + Glitch，赛博美学的教科书案例 |
| momentum | 动效设计工作室 | B+D | Canvas 流场丝带引擎（鼠标交互+永续流动）+ GSAP 横向 pin，墨黑配电光柠檬绿 |

## Scientific Emergence

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| aurora | 极光研究所 | B | 极光模拟 + 波形，科研气质的克制炫技 |
| helix | 生命科学 / 基因组研究 | A | DNA 双螺旋，几何精确性本身就是设计语言 |
| drift | 流式数据智能 | B | 流场粒子 + 噪声向量场，抽象数据概念的可视化范式 |
| flux | 流体动力学 | C | Navier-Stokes FBO 仿真，重量级 WebGL 多通道范例 |
| fissure | 反应扩散系统 | C | Gray-Scott 图灵纹理，生成式纹理而非预渲染图片 |
| verge | 引力涌现 | B | N-body 3000 粒子模拟，纯 Canvas 也能撑起大规模模拟感 |
| weave | 织物动力学 | B | Verlet 布料弹簧仿真，物理引擎级别的 Canvas 交互 |
| vanta | 光线行进 | C | Ray Marching + SDF，GLSL 硬核范例 |
| chroma | 薄膜干涉光学 | C | 菲涅耳虹彩效果，色彩本身作为核心叙事 |
| prism | 当代艺术装置 | E+B | CSS 3D 旋转正方体 + Canvas 光谱，两种轻量技术叠加出重量级观感 |

## Own Lane (No Persona Fit — That's Fine)

| Page | Industry | Engine | What to study |
|------|----------|--------|----------------|
| morph | 字形变形演示 | E | 实时字重/字宽控制（CSS variable font），排版本身即是全部内容，无 hero 引擎可言 |

## Teaching Pair — Deliberate Anti-Pattern Contrast

`folio.html`（Noa Hartman 作品集，克制的 bento grid，零动画但每处细节都对）和 `folio-v1.html`（同一人物同一文案的"反面版本"：默认字体、糊脸紫青渐变、全居中、塑料阴影、模板感——HTML 注释里明确写着"故意做得廉价"）是同一批语料里唯一一对**故意成对**的正/反例。审查一个"感觉很 AI"的页面时，把它和 `folio-v1` 的毛病列表对一遍，比空对着 `anti-cheap.md` 的清单更直观。

---

## A Note on Technique Diversity Since the Original 37

The original showcase corpus (referenced elsewhere in this skill as "37 pages") had one universal limitation: **zero real photography** — everything was CSS gradients or generative Canvas/WebGL standing in for imagery. The newer pages (serum, folio, petal, phantom, obsidian, solstice) break that pattern deliberately, pairing real stock photography (mostly Unsplash) with a lighter motion layer (glow drift, scroll-velocity reactions, exploded-view diagrams) instead of a full generative engine. When a brief is image-implied (food, hotel, fashion, skincare, travel — see the cheapness blacklist's "zero imagery" bug) and a fully generative hero would look thin, this photo-plus-light-motion lane is the reference point, not just "add a Three.js scene."
