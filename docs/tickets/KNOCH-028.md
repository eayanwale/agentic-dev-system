# KNOCH-028 — Sanity CMS: Wire Services Page to Sanity

## Description
Replace hardcoded service listings on the services page with live data from Sanity. Adding a new service, reordering, or updating copy happens entirely in Studio — no code change needed.

## Acceptance Criteria
- [ ] `getServices()` added to `src/js/sanity.js`
- [ ] Services page fetches and renders: title, description, coverImage, features list, ctaLabel, in `order` asc sequence
- [ ] Cover images served from Sanity CDN via `imageUrl()` helper
- [ ] Adding or reordering a service in Studio reflects on page after refresh
- [ ] CTA button uses `ctaLabel` from Sanity (falls back to "Get in Touch" if empty)
- [ ] Page renders gracefully if fetch fails

## GROQ Query
```js
export function getServices() {
  return fetchQuery(`*[_type == "service"] | order(order asc)`)
}
```

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023
- Related: services page (Phase 3 scope)
