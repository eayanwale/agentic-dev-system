# KNOCH-003 — Cinematic Chrome Navigation

## Status: SHIPPED — Phase 1 squash to main
## Priority: P0 (critical)
## Epic: EPIC-001 — Foundation

## Title
Fixed Chrome Navigation: Logo, Links, REC Timecode Indicator

## Description
Build the fixed full-viewport "chrome" UI layer from the cinematic reference (`final demo.html`). This is a three-column fixed overlay — `Knoch.` wordmark left, nav links center, live REC timecode right — all using `mix-blend-mode: difference` so it reads over any background. Includes the bottom timecode bar with scroll-progress indicator and frame counter.

## Acceptance Criteria
- [ ] `.chrome` element: `position: fixed; inset: 0; pointer-events: none; z-index: var(--z-chrome); mix-blend-mode: difference`
- [ ] Three-column grid layout: `[logo] [nav links] [rec indicator]`
- [ ] **Logo**: `Knoch.` in Fraunces serif with amber dot; links to `#` (top of page)
- [ ] **Nav links**: Work · Studio · Inquire — 11px uppercase mono, 0.2em letter-spacing; underline-slide hover (amber, 0.4s ease); smooth-scroll to section anchors
- [ ] **REC indicator**: pulsing amber dot + `REC · HH:MM:SS` timecode that counts up from page load; live-updating via `setInterval`
- [ ] **Bottom timecode bar**: fixed bottom, `K/M · 2026 · MARYLAND` left, scroll-progress bar center (amber fill tracking `window.scrollY`), `FRAME XX / 36` counter right
- [ ] Nav hides center links on mobile (`≤800px`); logo and REC remain
- [ ] Nav links remain accessible via keyboard (`:focus-visible` outlines)
- [ ] GSAP `gsap.quickTo` used for any animated nav state (not CSS transitions on the fixed layer position)
- [ ] Frame counter updates as user scrolls: `Math.ceil(progress * 36)` clamped 1–36

## Design Notes
From `final demo.html`:
- Chrome padding: `28px 36px` (desktop), `18px 20px` (mobile)
- Nav link font: `11px / 0.2em LS / uppercase`
- REC dot: 6×6px amber circle with `pulse` keyframe animation (1.6s, opacity 1→0.3)
- Scroll progress line: `height: 1px`, background `rgba(paper, 0.15)`, fill with `--p` CSS variable updated by JS
- Bottom bar uses same `mix-blend-mode: difference` so it reads over hero image

The "wow" interaction here: the entire chrome layer inverts against whatever is beneath it via `mix-blend-mode: difference`, so the white text automatically becomes dark ink over the light paper sections and stays light over dark photography — zero JS needed for color switching.

## Tradeoffs Considered
- `mix-blend-mode: difference` vs. JS-driven color switching: blend mode is GPU-composited and zero-JS. Downside: requires careful z-stacking — anything that shouldn't invert must be on its own compositing layer.
- Live timecode via `setInterval(fn, 1000)` vs. `requestAnimationFrame`: 1s interval is accurate enough and avoids draining CPU on continuous rAF for a cosmetic UI element.

## Related Tickets
- KNOCH-001, KNOCH-002 (depends on scaffold + tokens)
- KNOCH-005 (chrome must be hidden behind loader during load sequence, then fade in)
- KNOCH-016 (Lenis smooth scroll affects section anchors)
