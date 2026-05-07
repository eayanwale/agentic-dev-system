# KNOCH-011 — Portfolio Filter System

## Status: SHIPPED — Phase 3 squash to main
## Priority: P2 (medium)
## Epic: EPIC-003 — Secondary Pages
## Branch: feature/KNOCH-011-portfolio-filter
## PR: #20 (dev → test) — merged
## Test report: docs/test-reports/KNOCH-011-test-report.md

## Title
Portfolio Page: Category Filter Tabs with Animated Grid Re-Layout

## Description
The standalone `portfolio.html` page ("The work.") needs a category filter system — tabs for All, Weddings, Brand, Sports, Portraits. Filtering hides/shows tiles with a smooth GSAP animation (fade + slight y shift), not a jarring instant hide. Filter state persists in URL hash so links are shareable.

## Acceptance Criteria
- [ ] Filter tabs: `All · Weddings · Brand · Sports · Portraits` — 11px text, active tab has amber underline + amber color, inactive tabs are muted
- [ ] Active tab tracked via `data-filter` attribute on tab elements
- [ ] Each portfolio tile has `data-category="wedding|brand|sport|portrait|film"` attribute
- [ ] Filter logic: on tab click, tiles not matching category animate out (`opacity: 0, y: 20, duration: 0.3`), tiles matching animate in (`opacity: 1, y: 0, duration: 0.5, stagger: 0.05`) — non-matching tiles get `display: none` after exit animation completes
- [ ] "All" tab shows all tiles
- [ ] URL hash updates on filter change: `#weddings`, `#brand`, etc.
- [ ] On page load, read hash and apply correct filter
- [ ] Count label updates: `"Showing X of 47 projects"` below the grid
- [ ] "Load more →" button appears when filtered count < total — clicking it adds the next batch of project cards (static HTML for now, no server pagination)
- [ ] Keyboard accessible: tabs are `<button>` elements, filter works without mouse
- [ ] Filter tabs scroll into a horizontal scroll container on mobile (no line break)

## Design Notes
From `knoch_portfolio_page_redesign.html`, the portfolio page has:
- Header: `"The work."` left + filter tabs right on desktop, stacked on mobile
- Asymmetric masonry grid matching the homepage archive layout
- `"Showing 8 of 47 projects"` + `"Load more →"`

Category labels used in tile metadata should match filter tab values exactly (normalized to lowercase).

The animation sequence on filter: first animate out non-matching tiles simultaneously, then immediately animate in matching tiles. Do NOT wait for exit to complete before starting entry — overlap feels snappier.

## Tradeoffs Considered
- GSAP filter animation vs. CSS `display: none`: CSS can't animate from/to `display: none`. GSAP's `autoAlpha` (opacity + visibility) handles this cleanly without leaving invisible-but-space-consuming elements.
- Client-side filter vs. server: All 47 projects should be pre-rendered in the HTML. Client-side filter is instant and requires no backend. "Load more" loads the rest from a hidden `<div>` in the DOM.

## Related Tickets
- KNOCH-001 (portfolio.html as second entry point in Vite config)
- KNOCH-010 (shares grid layout)
- KNOCH-012 (filter should persist when returning from detail view)
- KNOCH-021 (accessibility: `aria-pressed` on filter tabs, live region for count update)
