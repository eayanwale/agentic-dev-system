# KNOCH-015 — Footer

## Status: SHIPPED — Phase 3 squash to main
## Priority: P2 (medium)
## Epic: EPIC-003 — Secondary Pages
## Branch: feature/KNOCH-015-footer

## Title
Footer: Three-Column Credits Bar with Social Links and Legal

## Description
The footer appears on all pages. It follows the cinematic reference design: a dark ink bar with a 3-column grid — studio info left, direct contact center, versioning/roll-number right. Includes social links to Instagram and YouTube. Light, understated — intentionally not a heavy footer with lots of nav links (the homepage-style full footer from the Aperture concept is reserved for the standalone pages).

## Acceptance Criteria
**Minimal footer (homepage — "credits" style):**
- [ ] `padding: 4rem 36px 2rem`; `border-top: 1px solid rgba(paper, 0.08)`
- [ ] 3-column grid:
  - Left: `"Knoch · A Working Studio"` (bold paper), `"College Park, MD"`, `"East Coast available"`
  - Center: `"Direct line"` (bold), `240.714.6933`, `enoch@knochmedia.com`
  - Right (text-right): `"Roll № 07 / 2026"` (bold), `"v 02.04 — last updated [month]"`, `"—"`
- [ ] Font: 10px mono, 0.2em LS, uppercase, `rgba(paper, 0.4)` — strong labels in paper color
- [ ] Email and phone are `<a href>` links (mailto / tel)
- [ ] Social links row below grid: Instagram icon + `@knochmedia` → Instagram profile; YouTube icon → channel
- [ ] Roll number and version should be easy to update (const at top of a shared `footer.js` module or inline in HTML comment)

**Expanded footer (about.html, portfolio.html, contact.html — "sitemap" style):**
- [ ] 4-column grid: Studio info | Work links | Studio links | Contact
  - Work: Weddings, Brand & Sports, Portraits
  - Studio: About, Journal (placeholder), Process
  - Contact: email, phone, Instagram
- [ ] Same dark styling, slightly more padding `padding: 5rem 8vw 3rem`
- [ ] Copyright line at bottom: `© 2026 Knoch Media · All rights reserved`

## Design Notes
The "Roll № 07 / 2026" in the minimal footer is a cinematic affectation — it implies the studio is on its 7th roll of film this year, reinforcing the film-photography brand voice. Update the number manually each month.

Instagram and YouTube icons: use inline SVG (not icon font) for performance and styling control. Icons should be `18×18px`, paper color at 40% opacity, full opacity on hover.

The footer does not include a nav repetition on the homepage — the fixed chrome nav handles all navigation. Footer links are present only on secondary pages.

## Tradeoffs Considered
- Two footer variants vs. one universal: The minimal credits bar matches the homepage's cinematic restraint; the secondary pages can afford a fuller footer since they don't have the scroll-storytelling nav. Sharing a base `footer.js` module ensures consistency.

## Related Tickets
- KNOCH-001 (shared across all pages)
- KNOCH-003 (footer social links complement chrome nav)
- KNOCH-017, KNOCH-018 (YouTube and Instagram links live in footer)
