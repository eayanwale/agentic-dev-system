# KNOCH-008 — Pinned Frame: Full-Bleed Parallax + Animated Studio Stats

## Status: TODO
## Priority: P1 (high)
## Epic: EPIC-002 — Homepage

## Title
Pinned Frame Section: Parallax Background with Scroll-Triggered Animated Counters

## Description
A full-bleed image section with `position: sticky` parallax background that slowly pans as the section scrolls. Left column has the studio's brand statement; right column has three animated number counters (Projects, Frames Developed, Dates Left) that count up when scrolled into view. This is the "studio credibility" section.

## Acceptance Criteria
- [ ] `.pinned-frame`: `height: 200vh; position: relative` (the extra 100vh provides scroll room for the sticky panel to move through)
- [ ] `.sticky`: `position: sticky; top: 0; height: 100vh; overflow: hidden`
- [ ] `.bg`: `position: absolute; inset: -10%` (bleed to prevent white edges during parallax); `background-size: cover; will-change: transform`
- [ ] Background parallax: `gsap.to('.bg', { yPercent: -15, scale: 1.05 })` scrubbed `top bottom → bottom top`
- [ ] Film-grain overlay on `.overlay` (same pattern as hero — `feTurbulence` SVG)
- [ ] Two-column content grid: `grid-template-columns: 1fr 1fr; padding: 0 8vw; align-items: center`
- [ ] **Left col**: amber meta tag `— THE STUDIO · BY THE NUMBERS`, then Fraunces headline `"Eight years. Twelve thousand frames. One pair of hands."`
- [ ] **Right col**: three stat blocks stacked, right-aligned
- [ ] **Counter animation**: each `.stat .n[data-count]` element counts from 0 to `data-count` value over 2.2s `power3.out` when scrolled into view (`once: true` — no re-trigger)
- [ ] Stats: `47` Projects 2018–2026, `12884` Frames developed, `6` Dates left · 2026
- [ ] Italic amber `<em>` wrapper on first and third stat numbers
- [ ] `.big` headline reveal: `y: 60 → 0, opacity: 0 → 1` on scroll enter, 1.4s `expo.out`
- [ ] Number formatting: stats over 999 use `toLocaleString()` (e.g., `12,884`)

## Design Notes
The `inset: -10%` on `.bg` is critical — without it, the parallax `yPercent` movement reveals the container edge. The 10% overshoot on all sides means the image bleeds 10% in every direction, giving the parallax 15% vertical travel room.

The "dates left" counter is dynamic — it should be a JS variable (`const DATES_LEFT = 6`) at the top of the module so it's easy to update. Same for total projects.

Font sizes: Stat numbers `clamp(40px, 5vw, 72px)` Fraunces 300. Stat labels `10px mono 0.25em LS uppercase`.

## Tradeoffs Considered
- `position: sticky` vs. GSAP pinning for the bg: sticky is CSS-native and doesn't require JS scroll listeners for the panel itself. The parallax tween on the bg element is the only GSAP involvement — this is lighter than a full ScrollTrigger `pin` which adds DOM wrappers.
- `once: true` on counter: counters should not recount on scroll-back. The initial count-up is the payoff; re-triggering on upward scroll would feel glitchy.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-007 (follows horizontal reel)
- KNOCH-009 (testimonial follows this section)
- KNOCH-004 (film grain overlay)
