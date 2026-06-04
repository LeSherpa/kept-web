# kept-web

Single-file landing page for Kept (Telegram bot). One file: `index.html`.

## Stack
Vanilla HTML/CSS/JS. No build tools, no frameworks. Windows/PowerShell env.
Python available (`python`, not `python3`). PIL/Pillow installed.

## File structure (index.html ~1763 lines)
- `<head>` CSS: lines ~14–900
- `<body>` HTML sections (in order): hero → pixel-divider → margot-intro → pixel-divider → evolution → pixel-divider → pricing → features → testimonials → faq → footer
- `<script>` JS: bottom of file

## Key CSS facts
- Variables: `--bg:#0A0A0C  --bg-card:#111115  --gradient:135deg #6B21A8→#C026D3→#F97316`
- Fonts: Inter + Press Start 2P (Google Fonts)
- `.pixel-label`: Press Start 2P, 8px, #9B50D8
- `.margot-sprite`: position:relative, animation:margot-float. Pixel size set inline via `drawMargot(id, px, stage)`.
- Hero enhancements (1–4) are clearly commented blocks — easy to remove.
- `.evo-*` classes: evolution section — `.evo-row`, `.evo-card`, `.evo-sprite-wrap`, `.evo-name`, `.evo-stage-num`
- `.evo-card.evo-hovered` controls hover glow (::before opacity) and float animation

## Key JS facts
- `delay(ms)` helper at top of script
- `drawMargot(containerId, px, stage)` — draws pixel sprite with stage-based colors. Stages: dormant/awakened/growing/flourishing/legendary. hero=6px, intro=6px, evo sprites=6px with explicit stage arg.
- Hero typewriter: async IIFE, fires `triggerMargotReact()` 300ms after finish
- Telegram mockups: `tgRun(idx)`, `tgShowPlay(idx)`, `tgTokens[3]`, bodies `tg-chat-body-0/1/2`
- Photo: `max-kukurudziak-7SeMT33YVVM-unsplash.jpg` (local file, same dir)
- Evolution hover: `mouseenter` adds `evo-hovered` to card + fires one-shot bounce via rAF; `mouseleave` removes class

## Pricing (current)
- Free: Unlimited text surprises, Schedule up to 3 months, Surprise notifications, Up to 2 invitees
- Plus €3.99/mo or €39.99/yr: Everything in Free, Unlimited scheduling, Up to 5 invitees, Photos/audio/video & stickers

## Edit conventions
- Use Python scripts (`_patch.py`) for large replacements with Unicode/emoji — Edit tool struggles with curly apostrophes and emoji in long strings.
- Always `python` not `python3` on this machine.
- Commit after each logical change. Branch `hero-backup` = snapshot before hero enhancements.
- No need to read whole file each session — grep for the specific section or line range needed.
