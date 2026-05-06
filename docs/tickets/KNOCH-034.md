# KNOCH-034 — Smoke wisps on per-character text hover

## Status: IN PROGRESS
## Priority: P2 (polish)
## Epic: Motion / interaction

## Title
Add a subtle upward smoke trail to the proximity-based char-hover effect.

## Description
The proximity char-hover currently fades the fill to transparent, fades in a hairline stroke, scales up slightly, and applies a small blur. This ticket adds a fourth dimension — three stacked `text-shadow` layers per character that climb upward and dissipate as the cursor approaches, reading as soft vapor / smoke rising off the letterform.

Affects every `.headline-hover` element (hero headline, interlude manifesto, frame headline, reel intro headline, archive headline, testimonial-related headlines).

Mechanics:
- Three text-shadow copies per character, all with x: 0 (no horizontal jitter)
- Y offsets `-2t / -5t / -9t` px (climbs higher with intensity)
- Blur radii `5t / 11t / 18t` px (more diffuse as it rises)
- Opacity `0.32t / 0.18t / 0.09t` (fades into nothing)
- Colour slightly desaturated (×0.92) toward warm grey so it reads as smoke against the paper/amber chars rather than a colour-matched glow

All values are intensity-proportional so the smoke fades in and out smoothly with cursor distance — same GSAP proxy that drives the existing color/stroke/scale/blur transitions handles the new shadow.

## Acceptance Criteria
- [ ] Hover any `.headline-hover` element on desktop → upward smoke wisps appear, intensifying as the cursor closes in
- [ ] Effect is visible but not loud — cap is "subtle"; no harsh halos
- [ ] Smoke colour stays in the paper/amber family for non-em / em respectively
- [ ] No regression on the existing color/stroke/scale/blur effect — they stack
- [ ] `prefers-reduced-motion: reduce` skips the entire char-hover (existing behaviour, no change)
- [ ] Touch / coarse pointer skips (existing behaviour)
- [ ] Reset path (when `t < 0.004`) clears `text-shadow` along with the other props — no residue when cursor leaves

## Design Notes
- **Why three shadow layers, not one** — a single shadow reads as a glow. Three at progressive offsets and decreasing opacity reads as motion / dissipation. Number stops at three because the perf cost of text-shadow scales linearly with layer count and three is enough to suggest depth.
- **Why upward only (Y negative, X zero)** — smoke rises. Adding horizontal jitter would suggest wind, which fights the cinematic-still aesthetic.
- **Why desaturate toward grey at ×0.92** — pure paper/amber shadows look like a glow. Slight greying nudges the reading toward vapor without breaking the colour palette.

## Tradeoffs Considered
- **SVG `feTurbulence + feDisplacementMap` for an actual procedural smoke shader** — rejected as overkill for a hover effect: the displacement filter is already used for the hero grain reveal and char-hover.js stops it on hover-in to avoid filter conflicts. Re-enabling a turbulence filter on hover would re-trigger that conflict and add per-char filter pipeline cost.
- **CSS-only approach via `:hover { text-shadow: ... }`** — rejected because the proximity intensity (`t`) is computed in JS per-frame; baking a fixed shadow into CSS would lose the smooth distance-based fade-in/out that's the whole point of the existing system.
- **Adding a separate `smoke-hover` class users can opt into per-element** — rejected as YAGNI: every existing `.headline-hover` benefits from the smoke; there's no element where the smoke would be unwanted.

## Related Tickets
- KNOCH-029 (per-character hover — original implementation, modified here)
- KNOCH-030 (signature features — the existing color/stroke/scale/blur effect was tuned here)
