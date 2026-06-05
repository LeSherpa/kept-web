# kept-web

Single-file landing page for Kept (Telegram bot). One file: `index.html`.

## Stack
Vanilla HTML/CSS/JS. No build tools, no frameworks. Windows/PowerShell env.
Python available (`python`, not `python3`). PIL/Pillow installed.

## File structure (index.html ~2421 lines)
- `<head>` CSS: lines ~14–1260
- `<body>` structure (in order):
  - `<header class="site-nav">` — fixed nav (wordmark + links + CTA)
  - `<section id="hero">` — typewriter headline, Margot sprite, LEARN MORE scroll cue
  - pixel-divider
  - `<section id="margot-intro">` — MEET MARGOT kicker, sprite + speech bubble, 3-step pixel icons
  - pixel-divider
  - `<section id="how-it-works">` — HOW IT WORKS label, "See it in action." headline, 3 Telegram mockups
  - pixel-divider
  - `<section id="evolution">` — THE FLAME label, evo row with 5 sprite stages
  - pixel-divider
  - `<section id="pricing">` — PRICING kicker, billing toggle, 2 pixel-style cards
  - `<footer>`
- `<script>` JS: bottom of file (~line 1820+)

## Edit conventions
- Use Python scripts (`_patch.py`) for large replacements with Unicode/emoji — Edit tool struggles with curly apostrophes and emoji in long strings.
- Always `python` not `python3` on this machine.
- Commit after each logical change. Branch `hero-backup` = snapshot before hero enhancements.
- No need to read whole file each session — grep for the specific section or line range needed.

---

## Design language — COMMITTED DIRECTION
Lo-fi / pixel art aesthetic throughout. This is intentional and consistent. Do not introduce modern SaaS patterns.

### Rules
- Border-radius: 4px max on interactive elements. No 50px pill buttons.
- Buttons: hard pixel drop shadow `box-shadow: 4px 4px 0 #4A1A8A`. Hover: `5px 5px 0`, `translate(-1px,-1px)`. Active: `2px 2px 0`, `translate(2px,2px)`.
- Borders: solid, not gradient.
- Badges/labels: Press Start 2P font, 7–9px, solid fill, 2–4px radius max.
- Pixel dividers (envelope sprite row) between every section.
- Glow: purple only (`rgba(123,47,190,...)`). No amber/orange in background. One radial glow top-center in hero only.
- Grain texture: `body::after`, `opacity: 0.035`, fixed, z-index 9999, pointer-events none.
- Alternating section backgrounds: hero=`--bg`, margot-intro=`--bg-panel`, how-it-works=`--bg`, evolution=`--bg-panel`, pricing=`--bg`, footer=`--bg`.

### Brand
- Background: `--bg: #0A0A0C` / `--bg-panel: #0D0D11` (alternating panel tone)
- Gradient: `#7B2FBE` → `#C026D3` → `#F97316` → `#F59E0B` (purple → orange → amber)
- Fonts: Inter (body), JetBrains Mono (headlines — `--font-display`), Press Start 2P (pixel labels, nav links, prices, badges)

---

## Design token system (`:root`)
All major values are tokenised. Use tokens — avoid raw px values for sizing/spacing.

### Type scale
- `--text-xs` 0.75rem · `--text-sm` 0.875rem · `--text-base` 1rem · `--text-lg` 1.125rem · `--text-xl` 1.375rem
- `--text-2xl` clamp(1.6rem,4vw,2.2rem) · `--text-3xl` clamp(2.4rem,6vw,4rem) · `--text-lead` clamp(1rem,2.5vw,1.2rem)
- `--font-display`: JetBrains Mono (used on hero-headline, section-title, price-amount)

### Pixel type scale (Press Start 2P)
- `--pixel-xs` 8px · `--pixel-sm` 9px · `--pixel-md` 11px

### Spacing (8px base)
- `--space-1..20` (4px → 80px). `--section-py: 80px` is the standard section vertical padding.

### Other
- `--radius: 4px` · `--bg-card: #111115` · `--bg-panel: #0D0D11`

---

## Key CSS classes

### Nav
- `.site-nav` — fixed header, frosted-glass on scroll (`.nav-scrolled`)
- `.nav-inner` — 3-col grid (1fr auto 1fr): wordmark | links | CTA
- `.nav-wordmark` — JetBrains Mono, justify-self: start
- `.nav-links a` — Press Start 2P 8px, hover `#C026D3`, active `.nav-active`
- `.nav-cta` — gradient bg, pixel shadow, Press Start 2P 8px

### Hero
- `#scroll-indicator` — `<a>` anchor to #margot-intro, "LEARN MORE" label, scroll-bob animation, fades out past 60px scroll

### Margot intro
- `.margot-intro-top` — flex row: sprite+label left, speech bubble right
- `.margot-intro-left` — flex col, centered
- `.margot-speech-bubble` — fixed 300×90px, flex-centered, typewriter text, tail pointing left
- `.margot-speech-text` — Press Start 2P 9px, text-align center, height auto, overflow visible
- `.hiw-row` — 3-step horizontal flex, max-width 860px, gradient connector line
- `.hiw-step` — icon wrap (96px canvas) + step number + title + desc

### Evolution
- `.evo-row`, `.evo-card`, `.evo-sprite-wrap`, `.evo-name`, `.evo-stage-num`
- `.evo-card.evo-hovered` controls hover glow and bounce animation

### Pricing
- `.price-card` — 4px radius, pixel border + shadow. `.featured` — purple border, magenta shadow
- `.price-control` — 40px fixed-height spacer (keeps cards height-aligned)
- `.billing-toggle` / `.billing-opt` / `.billing-opt.billing-active` — monthly/annual toggle
- `.popular-badge` — amber corner badge, appears on annual via `.badge-visible`
- `.price-label` — Press Start 2P 9px. `.price-amount` — JetBrains Mono 2.6rem 800wt
- `.price-features li::before` — pixel SVG checkmark (crispEdges)

### Shared
- `.pixel-label` — Press Start 2P, `--pixel-xs`, #9B50D8, uppercase
- `.pixel-divider` — envelope sprite row, z-index 2, transparent bg
- `.px-env-center` — larger center envelope, glow filter
- `.reveal` / `.reveal.visible` — fade+translate on scroll (IntersectionObserver, threshold 0.12)
- `.reveal-children.visible > *:nth-child(n)` — staggered delays 0.05–0.37s

---

## Key JS facts
- `delay(ms)` helper at top of script
- `drawMargot(containerId, px, stage)` — draws pixel sprite via stageColors. Stages: dormant/awakened/growing/flourishing/legendary. Do NOT use CSS filters — colors are baked in.
- `drawEnvelope / drawLock / drawHeart` — pixel canvas icons for HIW steps (px=6, 96px wrap)
- Hero typewriter: async IIFE, fires `triggerMargotReact()` 300ms after finish
- Speech bubble typewriter: IntersectionObserver (threshold 0.5), 65ms/char, `\n` → `<br>`, fires 300ms after bubble enters view
- Telegram mockups: `tgRun(idx)`, `tgShowPlay(idx)`, `tgTokens[3]`, bodies `tg-chat-body-0/1/2`
- Evolution hover: `mouseenter` adds `evo-hovered` + one-shot bounce via rAF; `mouseleave` removes class
- Nav scroll state: adds `.nav-scrolled` past 40px scroll
- Nav active section: IntersectionObserver rootMargin `-40% 0px -55% 0px`, clears all on hero
- Scroll indicator: fades out past 60px, fades back in near top
- Billing toggle: switches `#plus-price` / `#plus-period` text; shows `.badge-visible` on annual
- General reveal observer: adds `.visible` class to `.reveal` and `.reveal-children` elements

---

## Section notes

### Hero
- Typewriter headline: "Some things are worth keeping." — gradient text on "worth keeping"
- Margot sprite (px=6, default/awakened), purple radial glow, grain overlay
- CTA → t.me/getkeptbot, Press Start 2P font
- LEARN MORE scroll anchor, bob animation, hides on scroll

### Meet Margot (#margot-intro)
- Kicker: "MEET MARGOT" (pixel-md)
- Left: Margot sprite + "✦ MARGOT ✦" label
- Right: speech bubble — "I keep what matters.\nYou tell me when." — typewriter on scroll
- Below: "Three steps. That's it." headline + 3-step HIW row (envelope/lock/heart icons)

### How it works (#how-it-works)
- Kicker: "HOW IT WORKS" (pixel-md)
- Headline: "See it in action."
- 3 Telegram mockups: FOR SOMEONE / FOR YOURSELF / RECEIVING — click-to-play

### The Flame (#evolution)
- Kicker: "THE FLAME" (pixel-md)
- 5 stage sprites: DORMANT / AWAKENED / GROWING / FLOURISHING / LEGENDARY
- Gradient connector line, hover glow + bounce
- Piskel sprites will replace drawMargot canvas calls — swap to `<img>` per stage when ready

### Pricing (#pricing)
- Kicker: "PRICING" (pixel-md), headline "Simple pricing"
- Free card: €0/forever, 4 features, "Get started free" CTA
- Plus card: billing toggle (Monthly €3.99 / Annual €39.99 — 2 months free), MOST POPULAR badge on annual, "Start free 7-day trial" CTA
- Both cards have `.price-control` spacer for height parity

### Footer
- Logo, tagline, links (Privacy / Terms / Contact), copyright
- Leave as-is for now

---

## Responsive breakpoints
- `≤ 900px`: Telegram mockups stack vertically
- `≤ 760px`: Nav links hidden, hero headline smaller, HIW row stacks, speech bubble tail hidden, evo row compresses
- `≤ 560px`: Evo row wraps, connector hidden
- `≤ 480px`: Section padding reduced, pricing grid 1-col

---

## Do not touch
- Telegram mockup section layout and animations (`tg-*` classes)
- Font imports (`<link>` in `<head>`)
- CSS custom properties in `:root`
- Pixel divider markup (duplicate for new sections, never modify existing dividers)
- `drawMargot` function internals / stageColors object
