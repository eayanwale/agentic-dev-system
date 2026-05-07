# KNOCH-005 — Hero Section: Loader + Reveal Sequence

## Status: SHIPPED — Phase 2 squash to main
## Priority: P0 (critical)
## Epic: EPIC-002 — Homepage

## Title
Hero Section: Film-Counter Loader, Full-Bleed Background, and Staggered Text Reveal

## Description
Build the full hero experience: a full-screen film-counter loader that counts "exposures" 00→36 over 1.8s while a progress bar fills, then fades out and triggers the hero reveal — a scale-down parallax background, staggered headline line-by-line reveal, and fade-in of meta text. This is the most critical first impression section.

## Acceptance Criteria
**Loader:**
- [ ] Full-screen fixed overlay `z-index: var(--z-loader)`; ink background
- [ ] Large Fraunces frame counter `18vw` counting 00→36 using GSAP tween on a proxy object
- [ ] Amber label above: `Loading roll · 36 exposures` — 11px mono, 0.3em LS
- [ ] Progress bar: 240px wide, 1px height; amber fill animates 0→100% in 1.8s `power2.inOut`
- [ ] After 2s: loader fades out with `gsap.to(loader, { autoAlpha: 0, duration: 1 })`, then `display: none`
- [ ] On `window` `load` event (not `DOMContentLoaded`)

**Hero layout:**
- [ ] `height: 100vh; width: 100vw; overflow: hidden`
- [ ] `.hero-bg`: `position: absolute; inset: 0; background-size: cover; background-position: center; filter: brightness(0.45) grayscale(0.4) contrast(1.15); transform: scale(1.1)` initially
- [ ] Film-grain overlay on `hero-bg::after` (from KNOCH-004)
- [ ] Hero content centered: meta tag, H1 headline, sub text

**Reveal sequence (triggered after loader fades):**
- [ ] GSAP timeline: `hero-meta` fades in (opacity 0→1, 0.8s)
- [ ] `.hero-bg` scales from 1.1→1 over 2.4s `power3.out` (starts at `t=0` of timeline)
- [ ] `.hero-headline .line span` elements: `translateY(110%)→0` staggered 0.12s, `expo.out` 1.2s, starting at `t=0.2`
- [ ] `#hero-sub` fades in at `t=1.4`

**Headline copy:**
```
Nothing
staged.       ← italic amber em
Everything
remembered.   ← italic amber em
```
Each line wrapped in `.line > span` for the clip-reveal (overflow hidden on parent)

**Hero exit on scroll:**
- [ ] `hero-bg` parallax: `yPercent: 0→25` scrubbed from `top top` to `bottom top`
- [ ] `hero-content` exit: `yPercent: 0→-40; opacity: 1→0` same scrub range

## Design Notes
The clip-reveal technique: each `.line` has `overflow: hidden`. The inner `span` starts at `translateY(110%)` (fully below the clip) and animates to `translateY(0)`. This creates the characteristic "slide up from beneath" headline reveal without any masking elements.

Sub text: `CINEMATIC PHOTOGRAPHY & FILM · SCROLL ↓` — all-caps mono, 0.25em LS, muted paper color.

Hero image is a placeholder Unsplash URL during development. Production images will be local assets from `src/assets/hero/`.

## Tradeoffs Considered
- `window.load` vs `DOMContentLoaded` for loader trigger: `load` waits for images, which is correct — we want the loader to genuinely cover the hero image loading. On cached/fast connections the loader still runs its 2s minimum for effect.
- Scale(1.1) initial hero-bg: prevents white edges during the scale-down reveal. The extra 10% scale is invisible to the user but prevents any flash of edge.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-003 (chrome must not appear until after loader), KNOCH-004 (cursor hidden during load)
- KNOCH-016 (Lenis must be initialized after loader completes to prevent scroll during load)
