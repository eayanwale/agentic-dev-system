# KNOCH-007 — Horizontal Reel: Scroll-Hijacked Portfolio Carousel

## Status: TODO
## Priority: P0 (critical)
## Epic: EPIC-002 — Homepage

## Title
Horizontal Reel: Pinned Scroll-Driven Horizontal Portfolio Carousel

## Description
The signature "wow" section of the homepage. A horizontally-scrolling reel of portfolio cards controlled by vertical scroll — the section pins to the viewport, and scrolling translates the card track horizontally. Includes an intro panel, 6 portfolio cards with film-notch corner decorations, and a parallax "inner pan" effect on each card's background image as it enters frame.

## Acceptance Criteria
- [ ] `.reel` wrapper: `height: 100vh; overflow: hidden; position: relative`
- [ ] `.reel-track`: `display: flex; height: 100%; align-items: center; padding-left: 8vw; will-change: transform`
- [ ] ScrollTrigger: `trigger: .reel`, `start: 'top top'`, `end: () => "+=" + reelTrack.scrollWidth - window.innerWidth`, `pin: true`, `scrub: 0.8`, `invalidateOnRefresh: true`, `anticipatePin: 1`
- [ ] X translation: `gsap.to(reelTrack, { x: () => -(reelTrack.scrollWidth - window.innerWidth) })`
- [ ] **Intro panel** (60vw flex): large Fraunces "Selected work." headline with italic em, descriptor text, amber "→ → → Scroll horizontally" hint
- [ ] **6 portfolio cards** (38vw × 70vh each, 3vw gap): wedding, brand, sport, wedding, portrait, editorial
- [ ] Each card: film-notch corner decorations (4 corners, 18×18px L-brackets in paper color)
- [ ] Each card: `FRAME 0X` label top-left
- [ ] Each card: bottom overlay gradient + `.meta` (scene number, title, subtitle) — meta slides up on hover (`translateY(20px)→0, opacity 0→1`)
- [ ] Card background image: `filter: grayscale(0.3) brightness(0.85)` default; `grayscale(0) brightness(1)` on hover
- [ ] **Inner parallax**: each card's `.img` element tweens `x: 40→-40` scrubbed via `containerAnimation` (the reel tween) as the card passes through view
- [ ] Cards use `background-size: cover; background-position: center` — images to be replaced with local assets in production
- [ ] `ScrollTrigger.refresh()` called on `window` resize (with debounce)

## Design Notes
The `containerAnimation` parameter on each card's inner parallax ScrollTrigger references the parent horizontal tween. This chains the parallax to the horizontal scroll position rather than the page scroll position — an advanced GSAP pattern.

Card metadata:
1. SCENE 01 · WEDDING — Maya & James · Eastern Shore · 312 frames
2. SCENE 02 · BRAND — Annapolis Brewing · Campaign · 184 frames
3. SCENE 03 · SPORT — The Woodsmen · Garrett Co. · 421 frames
4. SCENE 04 · WEDDING — The Hartleys · DC · 278 frames
5. SCENE 05 · PORTRAIT — Senior Series · UMD · 96 frames
6. SCENE 06 · EDITORIAL — UMD Athletics · Spring · 254 frames

Trailing spacer of `8vw` after last card prevents abrupt end.

## Tradeoffs Considered
- Scroll-hijack vs. native horizontal scroll: Scroll-hijacking is the defining interaction of this portfolio. It's a deliberate design choice that signals premium intent. We mitigate UX concerns with the "→ → →" hint and by ensuring the section's scroll distance equals the horizontal content width, so no content is stranded.
- `scrub: 0.8` lag: chosen to feel physically weighty — like a reel of film has mass. `scrub: true` (instant) feels cheap; `scrub: 2` is too laggy on mobile.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-006 (appears after interlude)
- KNOCH-008 (pinned frame follows this section)
- KNOCH-010 (shares card hover aesthetic with portfolio grid)
- KNOCH-016 (Lenis smooth scroll must be paused/disabled while reel section is pinned to prevent interference)
- KNOCH-020 (mobile: cards become 80vw, inner parallax disabled)
