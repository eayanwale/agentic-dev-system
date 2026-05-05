# KNOCH-004 — Custom Cursor & Film-Grain Overlay

## Status: IN REVIEW
## Priority: P1 (high)
## Epic: EPIC-001 — Foundation

## Title
Custom Cursor with Magnetic Grow State and CSS Film-Grain Overlay

## Description
Replace the browser cursor with a custom amber-ring cursor that uses GSAP `quickTo` for smooth lag-behind tracking. On hover over interactive elements (links, cards, tiles, buttons), the cursor grows to 80px with a translucent fill. Also implement the CSS SVG film-grain noise overlay that sits on the hero and pinned-frame backgrounds.

## Acceptance Criteria
- [ ] `.cursor` element: `position: fixed; 24×24px; border: 1px solid var(--amber); border-radius: 50%; pointer-events: none; z-index: var(--z-cursor); mix-blend-mode: difference`
- [ ] Inner 2px amber dot via `::before` pseudo-element
- [ ] Mouse position tracked with `gsap.quickTo(cursor, "x", { duration: 0.35, ease: "power3" })` separately for x and y
- [ ] `.cursor.grow` state: `80×80px`, `background: rgba(232,162,58,0.08)` — triggered on `mouseenter` for all `a`, `.reel-card`, `.tile`, `.cta .button` elements
- [ ] Cursor hidden on mobile (`≤800px`): `body { cursor: auto }` + `.cursor { display: none }`
- [ ] Film-grain overlay: inline SVG `feTurbulence` filter applied as `background-image` on `.hero-bg::after` and `.pinned-frame .overlay` — `baseFrequency: 0.9`, `numOctaves: 2`, `opacity: 0.15–0.18`
- [ ] Grain overlay does not re-render on scroll (static SVG data URI, not canvas)
- [ ] Cursor becomes `cursor: pointer` fallback if JS fails (progressive enhancement)

## Design Notes
The cursor uses `mix-blend-mode: difference` so it inverts against both light and dark backgrounds identically to the chrome nav.

Film grain is implemented as a CSS `background-image` using an inline SVG data URI with `<feTurbulence>` — no texture image file needed, keeping bundle weight at zero. The grain appears subtly over the full-bleed hero image and pinned frame to give the cinematic feel from the reference.

The `grow` state transition uses CSS `transition: width 0.3s, height 0.3s, background 0.3s` — intentionally slower than cursor movement to create a "catch-up" feel.

## Tradeoffs Considered
- GSAP quickTo vs. CSS transition for cursor position: quickTo provides smooth easing and handles rapid mouse movement better than a CSS `transition` (which snaps when the transition restarts mid-move).
- SVG data URI grain vs. PNG texture: data URI adds 0 bytes to the image request waterfall and is infinitely scalable. Downside: no randomization per-load. Accept this — static grain is indistinguishable to users.

## Related Tickets
- KNOCH-001, KNOCH-002 (scaffold + tokens)
- KNOCH-005 (cursor must be hidden while loader is active)
