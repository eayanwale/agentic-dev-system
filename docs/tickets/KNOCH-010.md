# KNOCH-010 — Portfolio Grid (The Archive)

## Status: SHIPPED — Phase 2 squash to main
## Priority: P1 (high)
## Epic: EPIC-002 — Homepage

## Title
Portfolio Grid: Asymmetric 12-Column Contact-Sheet Layout with Hover Effects

## Description
The "archive" section — an asymmetric CSS grid of 7 portfolio tiles at varying sizes that mimics a photographic contact sheet. Each tile has a full-bleed image, hover color-reveal, and slide-up metadata overlay. Tiles animate in on scroll with a staggered y-offset reveal. Each tile's image has a subtle parallax inside the tile as the user scrolls past.

## Acceptance Criteria
- [ ] Section: `padding: 14rem 8vw 10rem`
- [ ] Section header: Fraunces `"The archive."` (italic amber on "archive.") left-aligned; right-aligned meta `"047 PROJECTS · INDEXED / BY DATE — REQUEST FULL CATALOG"` in 10px mono
- [ ] Header bottom border: `1px solid rgba(paper, 0.1)`, `margin-bottom: 5rem`
- [ ] `.grid`: `display: grid; grid-template-columns: repeat(12, 1fr); grid-auto-rows: 8vw; gap: 1rem`
- [ ] 7 tile spans (from reference):
  - `.t1`: col 1–7, row span 5 (large feature)
  - `.t2`: col 7–10, row span 3
  - `.t3`: col 10–13, row span 3
  - `.t4`: col 7–13, row span 2 (wide banner)
  - `.t5`: col 1–5, row span 3
  - `.t6`: col 5–9, row span 4
  - `.t7`: col 9–13, row span 4
- [ ] Each `.tile`: `position: relative; overflow: hidden; background: #1a1a1a`
- [ ] Each `.tile .img`: `position: absolute; inset: 0; background-size: cover; background-position: center; filter: grayscale(0.5) brightness(0.7); transition: filter 0.7s var(--ease-cinematic), transform 1.2s var(--ease-cinematic); will-change: transform`
- [ ] Hover: `filter: grayscale(0) brightness(1); transform: scale(1.04)`
- [ ] `.tile .label`: bottom overlay, `translateY(20px) opacity: 0` default → `translateY(0) opacity: 1` on hover, gradient bg
- [ ] Label contents: `№ 0XX · CATEGORY` (amber mono), title (Fraunces 22px), subtitle (mono 10px muted)
- [ ] **Scroll reveal**: each tile enters with `y: 80 → 0, opacity: 0 → 1`, `1.2s expo.out`, `delay: i * 0.05` stagger, trigger `top 88%`
- [ ] **Image parallax inside tile**: `yPercent: -8 → 8` scrubbed from `top bottom → bottom top` on each tile
- [ ] Header elements reveal: `y: 40 → 0, opacity: 0 → 1`, stagger 0.15, trigger `top 85%`

## Design Notes
Project content for 7 tiles:
1. `№ 047 · WEDDING` — "A long afternoon, Eastern Shore" · October 2025
2. `№ 046 · BRAND` — "Hands & water" · Annapolis
3. `№ 045 · SPORT` — "Day three of the cup" · Garrett Co.
4. `№ 044 · WEDDING` — "DC townhouse" · August 2025
5. `№ 043 · PORTRAIT` — "Class of '25" · College Park
6. `№ 042 · EDITORIAL` — "A study in red wine & candle" · Annapolis
7. `№ 041 · WEDDING FILM` — "The Hartleys — trailer" · Cinematic edit · 2:14

Tile `.t7` (The Hartleys trailer) will link to the YouTube integration (KNOCH-017) — clicking it opens a lightbox modal rather than navigating away.

The `8vw` `grid-auto-rows` makes tiles proportionally scale with the viewport. On mobile, all tiles become `grid-column: span 12; grid-row: span 1` with `28vw` row height (from reference).

## Tradeoffs Considered
- Fixed asymmetric spans vs. masonry: Fixed spans reproduce the editorial contact-sheet feel exactly as designed. True masonry would shuffle layout and break the visual composition. Accept the constraint.
- `will-change: transform` on `.img`: adds a compositing layer per tile (7 layers). Acceptable on this page since the images are the primary content. Remove if perf profiling shows memory pressure.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-009 (testimonial precedes grid)
- KNOCH-011 (filter system overlays this grid)
- KNOCH-012 (project detail view triggered from tiles)
- KNOCH-017 (tile 7 integrates YouTube lightbox)
- KNOCH-020 (mobile grid collapses to single column)
