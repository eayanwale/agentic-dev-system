# KNOCH-024 — Sanity CMS: Wire Testimonials Section

## Status: QA PASSED

## Description
Replace the hardcoded testimonials in KNOCH-009's section with live data fetched from Sanity. The section renders dynamically on page load using the content-fetch layer from KNOCH-023.

## Acceptance Criteria
- [x] Testimonials section fetches from Sanity on page load via `getTestimonials()`
- [x] All 5 real testimonials render correctly (Denise Bard, Rapha Records, Zach Albright, Mont Alto Woodsmen, Joseph Williams)
- [x] Display order matches the `order` field set in Sanity Studio
- [x] Section renders a loading state (skeleton or opacity fade) while fetch is in flight
- [x] If fetch fails, section falls back to displaying nothing gracefully (no broken layout)
- [x] GSAP scroll animations (from KNOCH-009) still trigger correctly on dynamically rendered cards
- [x] Adding or reordering a testimonial in Sanity Studio reflects on site after page refresh — no code change needed

## Implementation Notes
- Call `getTestimonials()` before initializing GSAP ScrollTrigger on that section — animations must be registered after DOM nodes exist
- Use `ScrollTrigger.refresh()` after content renders to recalculate scroll positions
- Template the card HTML in JS using a simple map over the results array

## Design Notes
- No avatar images — current site doesn't use them, keep it clean
- Company clients (Rapha Records, Mont Alto Woodsmen) display `clientName` only — no secondary label needed

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023, KNOCH-009
