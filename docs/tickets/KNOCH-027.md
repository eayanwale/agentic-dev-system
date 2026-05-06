# KNOCH-027 — Sanity CMS: Wire About Page to Sanity

## Description
Replace hardcoded content on `about.html` (KNOCH-013) with live data fetched from Sanity. The `aboutContent` document type is a singleton — only one document of this type exists; Enoch edits it directly in Studio.

## Acceptance Criteria
- [ ] `getAboutContent()` added to `src/js/sanity.js`
- [ ] `about.html` fetches and renders: headline, subheadline, bio, headshot, specialties, yearsExperience
- [ ] Headshot served from Sanity CDN via `imageUrl()` helper
- [ ] Updating bio or headline in Studio reflects on page after refresh — no code change needed
- [ ] Page renders gracefully if fetch fails (static fallback copy)

## GROQ Query
```js
export function getAboutContent() {
  return fetchQuery(`*[_type == "aboutContent"][0]`)
}
```

## Design Notes
- Singleton pattern — only one `aboutContent` document. Studio shows it directly, no list view needed.
- `__experimental_actions: ['update', 'publish']` in schema prevents accidental creation of duplicates (no "Create new" button)

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023, KNOCH-013
