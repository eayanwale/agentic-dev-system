# KNOCH-033 — Hero reel slideshow with filter pulse

## Status: IN PROGRESS
## Priority: P1 (polish)
## Epic: Hero / motion

## Title
Cycle the six reel stills as the hero background with a soft rack-focus crossfade.

## Description
Hero currently shows a single static `reel-01.png` background. Convert it into a slideshow that cycles through all six images in `src/public/assets/reel/` with a smooth filter-pulsed crossfade so the section feels alive even before the visitor scrolls.

Implementation:
- `.hero-bg` becomes a layer container; six `.hero-slide` children are inserted in JS, one per reel image. All share the parent's filter pipeline so colour treatment doesn't compound.
- The first slide starts at `opacity: 1` so the loader's `scale(1.1) → scale(1)` reveal still has something to render against.
- After the reveal timeline completes, a `setInterval` cycle crossfades the active slide out and the next slide in (`SLIDE_FADE_S = 1.6s`, `SLIDE_HOLD_MS = 5500`).
- During each crossfade, GSAP tweens two CSS variables on the parent — `--slide-blur` (0 → 2.4px → 0) and `--slide-bright` (0.45 → 0.55 → 0.45) — peaking at the crossfade midpoint. Reads as a soft rack-focus / projector dissolve rather than a hard fade.
- Page Visibility API pauses the cycle when the tab is hidden so we don't burn cycles on swaps the user can't see.
- `prefers-reduced-motion: reduce` skips the cycle entirely; reel-01 stays as a static bg.

## Acceptance Criteria
- [ ] All six reel images load from `/assets/reel/reel-01..06` (already in `src/public/` after KNOCH-031)
- [ ] First slide visible during loader/reveal — no blank flash during scale-down
- [ ] Crossfade plays every ~5.5s, lasts ~1.6s
- [ ] Blur + brightness peak at the crossfade midpoint, return to rest
- [ ] Cursor parallax (`mousemove → x/y` on `.hero-bg`) and scroll-exit (`yPercent: 25`) still work — slides inherit the parent's transform
- [ ] Tab-out → tab-in does not produce a queued burst of swaps
- [ ] Reduced motion → static reel-01, no cycle, no filter pulse

## Design Notes
- **Why CSS vars for the filter pulse instead of tweening the filter string** — GSAP can interpolate filter strings only when both ends use the exact same functions in the exact same order. Mixing in a blur for the pulse but not the rest would force a re-parse per frame; CSS vars sidestep that and let GSAP tween numeric values directly.
- **Why crossfade by opacity on stacked layers, not background-image swap** — swapping `background-image` triggers a fresh image fetch + decode on every cycle and produces a flicker on slow connections. Pre-mounted stacked layers are cheap (each image decoded once on init) and the GPU compositor handles opacity for free.
- **Slides as children of `.hero-bg`, not siblings** — the parent's `transform: scale(1.1)` reveal and the cursor-parallax `x/y` tweens automatically apply to every slide. No per-slide transform bookkeeping.

## Tradeoffs Considered
- **Ken-Burns per slide (subtle zoom while displayed)** — rejected for now: hero already has cursor parallax + scroll-exit yPercent moving the bg, plus the CSS scale(1.1) reveal. Adding per-slide zoom on top would feel busy; revisit if testimonials/feedback ask for it.
- **6 layered divs vs 1 div with JS-driven background-image swap** — chose layered divs (see above re: flicker + decode cost).
- **Random order vs sequential** — sequential 1→6 chosen so the loader/reveal frame (reel-01) anchors the cycle's start and the user sees images in a curated order.

## Related Tickets
- KNOCH-005 (hero loader + reveal — extended here)
- KNOCH-031 (Vite publicDir — reel images already moved)
