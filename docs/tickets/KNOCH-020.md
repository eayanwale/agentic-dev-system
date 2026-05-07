# KNOCH-020 — Responsive / Mobile Adaptations

## Status: 🚀 SHIPPED — Phase 5 squash to main (v0.5.0)
## Branch: feature/KNOCH-020-mobile
## PR: https://github.com/eayanwale/knochmedia/pull/26 (merged)
## Priority: P1 (high)
## Epic: EPIC-005 — Polish & Ship

## Title
Responsive Design: Mobile-First Breakpoints and Touch Interaction Adaptations

## Description
Every section designed desktop-first in the reference must degrade gracefully to mobile. This is not a cosmetic pass — several core interactions need fundamentally different implementations on touch: the horizontal reel becomes a native horizontal scroll snap carousel, the word-by-word scrub becomes a simple IntersectionObserver fade, and the cursor is hidden. This ticket covers all breakpoints and touch-specific fallbacks.

## Acceptance Criteria
**Breakpoints:**
- [ ] Mobile-first CSS — base styles are mobile, `@media (min-width: 800px)` adds desktop enhancements
- [ ] Primary breakpoint: `800px` (matches reference)
- [ ] Secondary breakpoint: `1200px` for large-desktop type scaling
- [ ] Tertiary: `480px` for small phones (tighten padding, reduce font sizes by ~15%)

**Navigation (≤800px):**
- [ ] Center nav links hidden: `.nav-center { display: none }`
- [ ] Hamburger menu button appears — opens a full-screen overlay nav with the same links
- [ ] Hamburger: 3-line icon, amber color, top-right position
- [ ] Overlay nav: ink background, Fraunces 300 large links (Work, Studio, Inquire), closes on link click or ×
- [ ] GSAP open/close: overlay `autoAlpha: 0 → 1`, links stagger up from `y: 40`

**Hero (≤800px):**
- [ ] Headline font-size uses `clamp()` so it already scales — verify no overflow at 375px width
- [ ] `.hero-sub` text wraps cleanly; "SCROLL ↓" stays on same line as cinematic text

**Horizontal reel (≤800px):**
- [ ] ScrollTrigger pin and horizontal translation DISABLED on mobile
- [ ] `.reel-track` becomes `display: flex; overflow-x: scroll; scroll-snap-type: x mandatory; -webkit-overflow-scrolling: touch`
- [ ] Each `.reel-card` gets `scroll-snap-align: start; flex: 0 0 85vw`
- [ ] `.reel-intro` becomes `flex: 0 0 90vw`
- [ ] Inner image parallax (`containerAnimation`) disabled on mobile
- [ ] Lenis already disabled on touch devices (KNOCH-016), so no conflict

**Interlude (≤800px):**
- [ ] Word-by-word scroll scrub replaced with a single `IntersectionObserver` fade-in for the whole quote block — `opacity: 0 → 1, transform: translateY(20px) → 0` over 0.8s
- [ ] All `.word` spans removed (or their opacity set to 1) when scrub is disabled

**Portfolio grid (≤800px):**
- [ ] All tile spans reset: `.t1, .t2, ... { grid-column: span 12; grid-row: span 1 }`
- [ ] `grid-auto-rows: 28vw` (from reference)
- [ ] Image parallax inside tiles disabled (GSAP scroll triggers not created on mobile)
- [ ] Tile hover label: always visible at 50% opacity on mobile (no hover state on touch)

**About split layout (≤800px):**
- [ ] `grid-template-columns: 1fr` — images stack above text
- [ ] Sticky left column becomes normal flow

**Contact form (≤800px):**
- [ ] `grid-template-columns: 1fr` — sidebar moves below form
- [ ] Sidebar collapses to a sticky bottom bar: `"Book a call →"` button pinned at bottom of screen

**Footer (≤800px):**
- [ ] `grid-template-columns: 1fr` — all cols stack
- [ ] `.credits .right { text-align: left }`
- [ ] Expanded footer becomes 2-column (`1fr 1fr`) then single col at `480px`

**General touch adaptations:**
- [ ] `cursor: auto` on `body` — browser cursor restored
- [ ] `.cursor { display: none }` on touch
- [ ] Tap target minimum `44×44px` on all interactive elements (WCAG 2.5.5)
- [ ] Remove `mix-blend-mode: difference` from chrome if it causes rendering issues on older Android WebView

## Design Notes
The mobile reel transformation is the most complex change: from a GSAP-pinned horizontal scroll to a native CSS scroll snap. Both provide the same UX — horizontal card browsing — but native scroll snap performs better on mobile and doesn't require JS. The `scroll-snap-type: x mandatory` with `scroll-snap-align: start` is the standard pattern.

Test on real devices: iPhone SE (375px, smallest modern iPhone), iPhone 15 Pro (390px), Samsung Galaxy S23 (360px), iPad Air (820px — should behave like desktop).

The breakpoint at `800px` is intentionally chosen to match the reference's media query. Do not adjust it without updating all reference values.

## Tradeoffs Considered
- Mobile-first CSS vs. desktop-first with overrides: mobile-first is specified in CLAUDE.md. It requires writing base styles for mobile and progressively enhancing for desktop — harder to retrofit if you write desktop first.
- Disabling animations on mobile vs. providing lighter versions: for the horizontal reel and word-scrub, disabling is the right call. These interactions require precise scroll control that touch devices don't provide. Simple fade-ins are not a degradation — they're appropriate for the medium.

## Related Tickets
- All section tickets (this ticket touches every component)
- KNOCH-003 (mobile nav hamburger)
- KNOCH-007 (reel becomes CSS snap scroll)
- KNOCH-006 (scrub becomes IntersectionObserver)
- KNOCH-016 (Lenis disabled on touch)
- KNOCH-021 (accessibility of mobile hamburger nav)
