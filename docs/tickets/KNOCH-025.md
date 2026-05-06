# KNOCH-025 — Sanity CMS: Wire Gallery/Reel Section

## Status: DONE — merged to test (2026-05-05)
## Branch: feature/KNOCH-007-horizontal-reel

## Description
Replace any hardcoded gallery/reel items in the horizontal reel (KNOCH-007) with live data from Sanity. Featured collections drive the homepage reel; all collections feed the /portfolio page.

## Acceptance Criteria
- [ ] Homepage reel (KNOCH-007) fetches featured gallery collections via `getFeaturedCollections()`
- [ ] Each reel card displays: cover image (via `imageUrl()` helper), collection title, category label
- [ ] Cover images served from Sanity CDN at 1500px wide, WebP format
- [ ] Horizontal scroll / GSAP reel animation (KNOCH-007) still works on dynamically rendered cards
- [ ] `ScrollTrigger.refresh()` called after cards are injected into the DOM
- [ ] Adding a new gallery collection in Sanity Studio and marking it `featured: true` adds it to the homepage reel on next page load — no code change needed
- [ ] Reordering collections via the `order` field in Studio reflects immediately on refresh

## Implementation Notes
- Fetch collections before GSAP reel initialisation — same pattern as KNOCH-024
- `imageUrl(coverImage, 1500)` for reel cards; use `800` for thumbnails if needed
- Category label can be a `<span>` overlaid on the cover image (already in KNOCH-007 design)

## Design Notes
- If no featured collections exist yet (Studio is empty), reel renders nothing — no broken layout
- Future: clicking a reel card could route to a full gallery page (Phase 3 scope, not this ticket)

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023, KNOCH-007
