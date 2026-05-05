# KNOCH-016 — Smooth Scrolling Integration (Lenis)

## Status: IN PROGRESS
## Priority: P0 (critical)
## Epic: EPIC-001 — Foundation

## Title
Smooth Scroll: Lenis Integration with GSAP ScrollTrigger Synchronization

## Description
Install and configure Lenis for smooth, physics-based scroll on desktop. Lenis must be synchronized with GSAP's ScrollTrigger via a `requestAnimationFrame` ticker loop — otherwise ScrollTrigger triggers at wrong positions. Lenis must be disabled during the loader, paused during the pinned horizontal reel section (to prevent double-scroll fighting), and disabled entirely on mobile/touch devices.

## Acceptance Criteria
- [ ] `lenis` installed as npm package
- [ ] Lenis initialized after loader completes (not before): `const lenis = new Lenis({ duration: 1.2, easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)), direction: 'vertical', gestureDirection: 'vertical', smooth: true })`
- [ ] GSAP ticker synchronization: `gsap.ticker.add((time) => { lenis.raf(time * 1000) })` and `gsap.ticker.lagSmoothing(0)`
- [ ] ScrollTrigger proxy: `ScrollTrigger.scrollerProxy(document.body, { scrollTop(value) { ... lenis.scrollTo(value) ... }, getBoundingClientRect() { return { top: 0, left: 0, width: window.innerWidth, height: window.innerHeight } } })`
- [ ] `lenis.on('scroll', ScrollTrigger.update)` to keep triggers in sync
- [ ] **Lenis disabled on touch/mobile**: `window.matchMedia('(pointer: coarse)').matches` check at init — if touch device, skip Lenis entirely (native scroll performs better)
- [ ] **During horizontal reel**: `lenis.stop()` when reel ScrollTrigger `onEnter`, `lenis.start()` when `onLeaveBack`/`onLeave` — prevents Lenis smooth-scroll from fighting the pinned section
- [ ] Lenis exported from `src/js/lenis.js` so other modules can call `lenis.stop()`/`lenis.start()`
- [ ] Scroll-to-anchor links (nav items) use `lenis.scrollTo('#section-id', { duration: 1.5, easing: ... })` instead of native anchor behavior

## Design Notes
The Lenis + GSAP sync pattern is the recommended approach from the GSAP docs for smooth-scroll libraries. The critical detail is `gsap.ticker.lagSmoothing(0)` — without this, GSAP's lag compensation fires during slow frames and causes scroll jitter.

The `duration: 1.2` gives a slightly longer deceleration than the GSAP default, matching the "cinematic weight" feel of the design. Fine-tune during implementation — could go to 1.4 for extra weight.

Easing function `1.001 - 2^(-10t)` is the standard Lenis exponential ease-out. Do not change it without testing across trackpad, mouse wheel, and keyboard scroll on both Mac and Windows.

## Tradeoffs Considered
- Lenis vs. no smooth scroll: Lenis is essential to the premium feel — native scroll on the scroll-storytelling sections feels jarring. The tradeoff is JS complexity and the need to sync with ScrollTrigger. This is a solved problem with the ticker proxy pattern.
- Disable on touch vs. degrade gracefully: disabling completely on touch devices is the right call. Lenis on touch adds latency and competes with iOS momentum scroll. Native touch scroll is already smooth on modern phones.

## Related Tickets
- KNOCH-001 (Lenis installed as npm dep)
- KNOCH-005 (loader completes before Lenis starts)
- KNOCH-007 (horizontal reel requires Lenis pause)
- All scroll-animated sections depend on this being correctly initialized
