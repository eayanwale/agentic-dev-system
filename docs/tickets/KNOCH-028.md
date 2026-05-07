# KNOCH-028 — Sanity CMS: Wire Services Page to Sanity

## Status: ⏸ DEFERRED

The fetch helper (`getServices()`) is already in `src/js/sanity.js:39` and the `service` schema is registered in `studio/sanity.config.js`. The page-render half of the ticket has no surface to land on — the build never created `src/services.html` and the homepage doesn't carry a services section. Matches the pattern of the other deferred CMS tickets (KNOCH-026 / 029 / 030) and the deferred Phase 4 ticket (KNOCH-018) where the surface didn't fit the build. Revisit post-launch if a public services page becomes worth the SEO + scope-clarity tradeoff. The contact form's budget tier selector (KNOCH-014) is the current pricing-conversation surface — see [contact.html:180-187](../../src/contact.html#L180).

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
