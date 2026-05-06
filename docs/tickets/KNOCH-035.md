# KNOCH-035 — Testimonial spotlight: wider reveal, warmer filter

## Status: IN PROGRESS
## Priority: P2 (polish)
## Epic: Polish

## Title
Reveal more of the testimonial background image on hover so the section reads as atmospheric, not voyeuristic.

## Description
The hover spotlight on the testimonial section was a 220 px circle at 0.58 opacity over a `grayscale(0.6) brightness(0.45) blur(2px)` filter. The combination of small reveal area, low brightness, and 2 px blur made the image read as muddy / peering-through-fog — the user described it as "creepy". This ticket widens the spotlight, lifts the bg opacity peak, eases the CSS filter, and pulls the four hardcoded magic numbers into named constants so future tuning is one-line.

Changes:
- `SPOT_RADIUS`  220 → 460 px (larger reveal area)
- `SPOT_FALLOFF` 80% → 90% (slightly harder edge, less haze)
- `BG_OPACITY`   0.58 → 0.75 (more visible, still dim enough to read as atmospheric)
- `.testimonial-bg` filter:
  - grayscale 0.6 → 0.4 (more colour survives)
  - brightness 0.45 → 0.7 (faces / details no longer crushed)
  - blur 2px → 1px (sharper, less foggy)

Plus a small refactor — `buildSpotMask(x, y)` helper replaces the radial-gradient string literal that was duplicated in three places.

## Acceptance Criteria
- [ ] Hover testimonial section → background reveals a visibly larger circle around the cursor than before; image reads as atmospheric not muddy
- [ ] Mouse leaves → bg fades to 0 within 0.4 s (unchanged)
- [ ] Slide change while hovered crossfades the new image at the new BG_OPACITY (0.75)
- [ ] No regression on testimonial typing/un-typing scroll behaviour, dot nav, auto-advance
- [ ] No regression on `prefers-reduced-motion` (existing skip path)
- [ ] No console errors

## Design Notes
- **Why bigger circle, not full bg reveal** — the cursor-tracking spotlight is core to the section's character; removing it would lose the "explore by hovering" cue. Just made the reveal generous instead of pinhole.
- **Why 0.75 not full 1.0 opacity** — testimonial section is text-led; a fully visible bg would compete with the quote. 0.75 with the eased filter sits just behind the type without fighting it.
- **Why named constants** — four magic numbers were duplicated at three call sites. Tuning required hunting; now one constant per knob and a single helper for the mask string.

## Tradeoffs Considered
- **Remove spotlight entirely, fade whole bg in on hover** — rejected: loses the cursor-tracking interaction the user has implicitly endorsed by not asking for its removal.
- **Brighter still (brightness 0.85+)** — rejected for now: would compete with the quote text. Easy to push higher in one constant if user still wants more.
- **Different image per testimonial via blend mode rather than mask swap** — out of scope; current crossfade-on-goTo is fine.

## Related Tickets
- KNOCH-009 / KNOCH-024 (testimonial section, original spotlight implementation)
- KNOCH-032 (testimonial background images now use index-based testimonial-NN.jpg)
