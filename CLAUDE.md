# Harman Banwait Portfolio

Personal portfolio for Harman Banwait, deployed on GitHub Pages.
Live: https://lebanwait.github.io/portfolio/ | Repo: lebanwait/portfolio (main branch, served from root)

## Project structure
- `index.html` — home page. Scroll-driven narrative with space-to-sky transition. All CSS/JS inline, images embedded as base64.
- `microlearn/` — MicroLearn case study (dark, green theme #35871f text / #7dd857 brand)
- `harvest-pierce-county/` — Harvest case study (cream light theme, brown #9e5e1c + leaf green #2f8f4f)
- `the-agile-mind/` — The Agile Mind case study (deep indigo, purple #9b82ff)
- Each folder has its own `index.html` and `preview.png` (og image). `demo.mp4` in microlearn.
- `.nojekyll` must stay (stops GitHub Pages from rendering README instead of index).

## Home page architecture (index.html)
Flow: full-screen hero (space scene, nav hidden) -> scroll reveals nav -> "Who I am" story section -> project cards -> skills -> contact.

Key systems, all vanilla JS/CSS, no dependencies:
- **Entrance**: page loads dark (hero::after veil, `darkLift` 3.8s). Name emerges centered via `emerge` keyframes (blur+scale, 4.4s). Supporting lines stagger in after 5s. Scroll cue last at 6.4s.
- **Palette interpolation**: scroll position drives a dark->sky-blue transition. `paint(t)` in the inline script lerps CSS custom properties (--ink, --text, --panel, --line, --spring, --navbg, etc.) between DARK and SKY objects. Starts at 0.18×viewport, range 0.5×viewport. Also drives `--spaceop` (space layer fades out) and `--skyop` (day layer fades in, second half).
- **Space layer** (`.space`, fixed, dark phase): twinkling stars (box-shadow fields), procedural moon (embedded PNG, rendered from a height map with bump-lit craters — regenerate with numpy/PIL if it needs changes, do not try SVG filters, they were rejected), small bright distant sun, ringed planet, red planet, drifting debris, two comets on long cycles, rocket with steady afterburner climbing on a 75s loop.
- **Sky layer** (`.sky`, fixed, day phase): soft sun, faint daytime moon, three drifting clouds, three gliding birds, distant jet (side-profile SVG) with contrail on a 100s loop.
- **Reveal system**: `.reveal` fades up on IntersectionObserver; `.stagger` staggers children (cards, skills).
- **Nav**: fixed, hidden until scrollY > 0.62×viewport, then slides in. Link color is `var(--muted)`/`var(--text)` so it adapts to the palette (never hardcode gold in nav).
- All animations have `prefers-reduced-motion` fallbacks: nav always visible, content static, birds/jet/comets/rocket hidden.

## Design system
- Fonts: Bricolage Grotesque (display), Inter (body), JetBrains Mono (labels/eyebrows)
- Home dark palette: ink #0a0d0c, gold spring #f5b73f. Sky palette: #a8d5f2 bg, navy #0e2034 text, amber #ad6e08 accent.
- `.card-media.ml` background is hardcoded #7dd857 to match the baked logo tile — do not switch it to var(--ml).
- UW letter coloring (purple/gold) was tried and REMOVED — do not reintroduce.

## Writing rules (apply to all copy)
- Short sentences, active voice, no adverbs ending in -ly, no em dashes ever, no buzzwords.
- No two bullets start with the same word.
- The "Who I am" story copy is Harman's own words verbatim — do not rewrite without asking.
- Honest framing always: MicroLearn = real code written; TAM = product direction, FigmaMake built it; Harvest = pure PM, no code.
- Omar Schwartz is never credited in public materials.

## Workflow
- Discuss and get approval on content/direction BEFORE building.
- Verify visual output (render/screenshot) before presenting.
- Deploy = copy repo contents to root (never a wrapper folder), push, hard-refresh.

## Current state / open items
- Home page: done through the space-to-sky version with all graphics.
- Project pages: still have the old static treatment. Open decision: carry the hero/scroll animation language onto them (each keeping its own theme).
- Considered but not built: faster entrance on repeat visits (sessionStorage skip), parallax orbs, marquee skills strip.
- LinkedIn preview: preview.png at root must be regenerated if the hero copy changes (LinkedIn caches; re-scrape via Post Inspector with ?v=2 trick).

## The bigger vision
This portfolio doubles as proof-of-concept for a generative web pipeline: structured intake (purpose, content, content types, page count, page plans, inspiration refs, personal material) -> Claude interprets and builds -> component library (entrance, palette interpolation, reveal system, ambient scene layers, procedural assets) -> verification loop -> zip/deploy. The reusable pieces live in index.html today; extracting them into a component library is the next phase. This pipeline is itself a future case study.
