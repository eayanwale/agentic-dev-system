# KNOCH-041 — Mobile Sustainable Mode

## Status: DONE — merged to test (2026-05-07)
## Branch: feature/KNOCH-041-mobile-sustainable
## PR: https://github.com/eayanwale/knochmedia/pull/27 (merged)
## Priority: P0 (mobile site is breaking)
## Epic: EPIC-005 — Polish & Ship

## Title
Strip GSAP / Lenis / scroll-driven animation on mobile; ship a static, simple-transition fallback

## Description
Live mobile review of KNOCH-020's output found the cinematic stack still breaks on real phones — content not loading, glitchy scroll, transitions stuck on screen, sections overlapping. The KNOCH-020 pass gated *some* scroll-tied animations on mobile (interlude reveal, reel pin, project-others) but missed the long tail: hero scroll-exit parallax, frame parallax bg, tile-router expanding-tile transition, about chapter pinned timeline, inquiry/contact step transitions, etc.

Two specific bugs Enoch flagged on review:

1. **Back to works stuck** — clicking the back link on `/project.html` leaves the project image on screen and hides the rest of the page beneath it. Almost certainly bfcache restoring the previous page's `tile-router.js` clone + black veil (created at z-index 12000 right before navigation; never cleaned up because the page unloaded).
2. **Contact form overlap** — on mobile, the "skip the form" sidebar overlaps the "tell me about your day" form region. Step / sidebar layout doesn't collapse correctly under the breakpoint.

Beyond the specific bugs, the directive is broader: **make mobile a static, simple-transition experience**. No scroll-driven motion. No GSAP timelines tied to scroll. No Lenis. CSS-only transitions, ≤300 ms, where any motion is needed at all.

## Acceptance Criteria

**Global mobile gate:**
- [ ] Every JS module that uses GSAP scroll-driven animation has an early-return on `(max-width: 800px)` or sets initial state to the final visual.
- [ ] No `ScrollTrigger.create` runs on mobile except where its only job is layout (pinning) — and even those should fall back to natural document flow on mobile.
- [ ] Lenis already off on touch (KNOCH-016) — no change needed.

**Per-module mobile gates (audit + fix):**
- [ ] `hero.js`: scroll-exit parallax disabled on mobile. Loader + reveal timeline fine — those run once on load. Slideshow continues but with simpler crossfade (or static first frame if it's still glitchy).
- [ ] `frame.js`: parallax bg movement disabled on mobile. Counter count-up animation can stay (one-shot on enter) OR snap to final value if it glitches.
- [ ] `about.js`: chapter pinned timeline replaced on mobile by a simple stacked layout — chapters render top-to-bottom, no scrub, no opacity tween.
- [ ] `testimonial.js`: any scroll triggers gated; testimonial just renders the active one.
- [ ] `portfolio-grid.js`: tile parallax + reveal disabled on mobile. Tiles just appear.
- [ ] `tile-router.js`: expanding-tile transition skipped on mobile — plain `window.location.href` navigation. Also: `pageshow` listener with `event.persisted` check to wipe any leftover clone overlays from bfcache.
- [ ] `inquiry.js` + `contact-page.js`: step transitions become CSS-only fades (or simple show/hide) on mobile.
- [ ] `interlude.js`: already gated (KNOCH-020); confirm.
- [ ] `reel.js`: already gated (KNOCH-020); confirm intro animation is also gated.
- [ ] `project-page.js`: "Other works" reel pin already gated (KNOCH-020). Veil fade-in on entry stays (one-shot, fine).

**Specific bug fixes:**
- [ ] **Back to works stuck**: tile-router's clone + black veil get cleaned up on `pageshow` when `event.persisted === true` (bfcache restore). Better fix: skip the transition entirely on mobile (no clone created in the first place).
- [ ] **Contact form overlap**: investigate the "skip the form" sidebar overlapping the form region under the mobile breakpoint. Likely the sidebar's grid placement or sticky positioning isn't collapsing to single-column flow correctly.

**No regression on desktop:**
- [ ] Every change is mobile-gated. Desktop continues to get the full cinematic experience.
- [ ] Build clean.

## Design Notes

The simplest pattern for mobile gating: add a one-line `if (window.matchMedia('(max-width: 800px)').matches) return;` (or set initial state to final) at the top of every animation function. We're not refactoring the desktop code, just opting mobile out.

The `prefers-reduced-motion` paths in most modules already do exactly this — snap to final state. Mobile should follow the same shape: short-circuit to the final visual.

Where a module's "final state" needs CSS adjustments (e.g. about chapters need to stack top-to-bottom on mobile when the GSAP is gone), add a `@media (max-width: 800px)` rule that overrides `position: absolute; inset: 0` to `position: relative; height: auto`.

## Tradeoffs Considered

- **Per-module gating vs. central kill switch**: per-module is more invasive but lets us keep one-shot loader animations (hero reveal, frame counters) that don't break on mobile. A central kill would also strip those.
- **Strip vs. simplify**: stripping is faster to ship and easier to verify than reworking. The desktop experience is what carries the brand; mobile just needs to deliver the content reliably.

## Related Tickets

- KNOCH-020 (mobile pass — this is the second pass after live device review)
- KNOCH-016 (Lenis — already off on touch)
- KNOCH-019 (perf — image WebP work helps load reliability on mobile)
- KNOCH-021 (a11y — reduced-motion users get the same simple experience)
