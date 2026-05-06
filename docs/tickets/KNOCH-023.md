# KNOCH-023 — Sanity CMS: JS Content-Fetch Layer

## Status: DONE — merged to test (2026-05-05)
## Branch: feature/KNOCH-022-023-sanity-cms-setup

## Description
Create a thin, reusable JS module that fetches content from Sanity's CDN API. All sections that consume CMS data (testimonials, gallery) import from this module — no raw fetch calls scattered across files.

## Acceptance Criteria
- [ ] `src/js/sanity.js` created with a `fetchQuery(groq)` helper
- [ ] Project ID and dataset read from a config object (sourced from a `<meta>` tag or inline config in `index.html` — no build step needed)
- [ ] `getTestimonials()` exported — returns testimonials sorted by `order` asc
- [ ] `getGalleryCollections()` exported — returns all collections sorted by `order` asc
- [ ] `getFeaturedCollections()` exported — returns only `featured: true` collections
- [ ] Fetch errors caught and logged; functions return `[]` on failure (graceful degradation)
- [ ] No external dependencies — vanilla `fetch()` only

## Implementation Notes

### Config pattern (no build step)
In `index.html`:
```html
<meta name="sanity-project-id" content="YOUR_PROJECT_ID" />
<meta name="sanity-dataset" content="production" />
```

In `src/js/sanity.js`:
```js
const PROJECT_ID = document.querySelector('meta[name="sanity-project-id"]').content
const DATASET = document.querySelector('meta[name="sanity-dataset"]').content
const BASE = `https://${PROJECT_ID}.api.sanity.io/v2024-01-01/data/query/${DATASET}`

async function fetchQuery(groq) {
  const url = `${BASE}?query=${encodeURIComponent(groq)}`
  const res = await fetch(url)
  const { result } = await res.json()
  return result ?? []
}
```

### GROQ queries
```js
// Testimonials
export const getTestimonials = () =>
  fetchQuery(`*[_type == "testimonial"] | order(order asc)`)

// All gallery collections
export const getGalleryCollections = () =>
  fetchQuery(`*[_type == "galleryCollection"] | order(order asc)`)

// Featured only (homepage reel)
export const getFeaturedCollections = () =>
  fetchQuery(`*[_type == "galleryCollection" && featured == true] | order(order asc)`)
```

### Image URL helper
Sanity images need URL construction from their asset reference:
```js
export function imageUrl(source, width = 1500) {
  const ref = source?.asset?._ref ?? ''
  const [, id, dims, ext] = ref.split('-')
  return `https://cdn.sanity.io/images/${PROJECT_ID}/${DATASET}/${id}-${dims}.${ext}?w=${width}&auto=format`
}
```

## Design Notes
- Using meta tags avoids a build step while keeping the project ID out of JS source (it's not a secret, but keeps config centralised)
- Sanity's CDN (`api.sanity.io`) is globally distributed — no performance concern
- `auto=format` on image URLs serves WebP to supporting browsers automatically

## Related Tickets
- Depends on: KNOCH-022
- Blocks: KNOCH-024, KNOCH-025
