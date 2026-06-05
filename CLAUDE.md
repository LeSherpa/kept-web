# kept-web

Single-file landing page for Kept (Telegram bot). One file: `index.html`.

## Stack
Vanilla HTML/CSS/JS. No build tools, no frameworks. Windows/PowerShell env.
Python available (`python`, not `python3`). PIL/Pillow installed.

## File structure (index.html ~1763 lines)
- `<head>` CSS: lines ~14–900
- `<body>` HTML sections (in order): hero → pixel-divider → margot-intro → pixel-divider → evolution → pixel-divider → pricing → footer
- `<script>` JS: bottom of file

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
- Borders: solid, not gradient
- Badges/labels: Press Start 2P font, 7–8px, solid fill, 2–4px radius max
- Pixel dividers (envelope sprite row) between every section
- Glow: purple only (`rgba(123,47,190,...)`). No amber/orange in background. One radial glow top-center in hero only.
- Grain texture: `body::after`, `opacity: 0.035`, fixed, z-index 9999, pointer-events none

### Brand
- Background: `#0A0A0C` (near-black, not pure black)
- Gradient: `#7B2FBE` → `#C026D3` → `#F97316` → `#F59E0B` (purple → orange → amber)
- Fonts: Inter (body), Press Start 2P (pixel labels, prices, badges)

---

## Section order (current + planned)
1. Hero
2. How it works ← TODO: not yet built
3. Margot + Telegram mockups
4. The Flame / Evolution
5. Pricing
6. Footer

---

## Key CSS facts
- Variables: `--bg:#0A0A0C  --bg-card:#111115  --gradient:135deg #6B21A8→#C026D3→#F97316`
- Fonts: Inter + Press Start 2P (Google Fonts)
- `.pixel-label`: Press Start 2P, 8px, #9B50D8
- `.margot-sprite`: position:relative, animation:margot-float. Pixel size set via `drawMargot(id, px, stage)`.
- `.evo-*` classes: `.evo-row`, `.evo-card`, `.evo-sprite-wrap`, `.evo-name`, `.evo-stage-num`
- `.evo-card.evo-hovered` controls hover glow (::before opacity) and float animation
- Spacing rule: section padding 100px 24px. Gap between section title and subtitle: 8px. Use inline styles with !important if class margins override.

---

## Key JS facts
- `delay(ms)` helper at top of script
- `drawMargot(containerId, px, stage)` — draws pixel sprite with stage-based colors via stageColors object. Stages: dormant/awakened/growing/flourishing/legendary. Do NOT use CSS filters for sprite color — colors are baked into the draw function.
- Hero typewriter: async IIFE, fires `triggerMargotReact()` 300ms after finish
- Telegram mockups: `tgRun(idx)`, `tgShowPlay(idx)`, `tgTokens[3]`, bodies `tg-chat-body-0/1/2`
- Photo: `max-kukurudziak-7SeMT33YVVM-unsplash.jpg` (local file, same dir)
- Evolution hover: `mouseenter` adds `evo-hovered` to card + fires one-shot bounce via rAF; `mouseleave` removes class. Hover does NOT change sprite color — glow and bounce only.

---

## Section notes

### Hero
- Typewriter headline: "Some things are worth keeping."
- Gradient text on "worth keeping"
- Single purple radial glow top-center only (amber glow removed)
- CTA button: pixel style, 4px radius, hard drop shadow

### How it works (TODO)
- 3 steps: "You keep it." → "Margot holds it." → "They feel it."
- Pixel icon per step (box-shadow drawn), Press Start 2P step numbers
- Horizontal layout with connector line between steps
- Does NOT duplicate Margot mockups — explains flow, mockups show product

### Margot section
- 3 Telegram mockups: FOR SOMEONE / FOR YOURSELF / RECEIVING, click-to-play
- Subtitle: Margot speaks first person — "I keep what matters. You tell me when. I handle the rest. 💅" (TODO: not yet updated)
- Sprite above mockups: px=6, awakened stage

### The Flame / Evolution
- 5 stage sprites in a row with gradient connector line
- Stages: DORMANT / AWAKENED / GROWING / FLOURISHING / LEGENDARY
- Stage colors defined in stageColors object inside drawMargot
- Piskel sprites will replace drawMargot canvas calls when ready — swap to <img> per stage

### Pricing (TODO: pixel redesign pending)
- Cards: 4px radius, hard pixel border box-shadow
- Prices in Press Start 2P
- "MOST POPULAR" badge: pixel-label style, solid #7B2FBE
- Free: €0 | Plus: €3.99/mo or €39.99/yr (2 months free)
- Free: text only, schedule up to 3 months, up to 2 invitees
- Plus: photos/audio/video/stickers, unlimited scheduling, up to 5 invitees

### Footer
- Leave as-is for now

---

## Do not touch
- Margot mockup section layout and animations
- Font imports
- CSS custom properties in :root
- Pixel divider markup (duplicate for new sections, never modify)
