# KNOCH-026 — Sanity CMS: Migrate Hero Images to Sanity CDN

## Description
Upload the 6 hero images currently served from `src/assets/` into Sanity's image CDN. Update the hero section to pull image URLs from Sanity so new hero images can be added/swapped via Studio without touching code.

## Acceptance Criteria
- [ ] All 6 hero images uploaded to Sanity Studio as `heroImage` documents
- [ ] `heroImage` schema defined with: `image` (Sanity image type), `altText` (string), `headline` (string), `order` (number)
- [ ] Hero section JS fetches hero images from Sanity via a `getHeroImages()` query
- [ ] Images served from `cdn.sanity.io` at 1500px, WebP format
- [ ] Hero slideshow/reel cycles through images in `order` asc sequence
- [ ] Existing hero animations (KNOCH-005) unchanged
- [ ] Adding a new hero image in Studio and setting its `order` includes it in rotation on next load

## heroImage Schema
```js
{
  name: 'heroImage',
  title: 'Hero Images',
  type: 'document',
  fields: [
    { name: 'image', title: 'Image', type: 'image', options: { hotspot: true } },
    { name: 'altText', title: 'Alt Text', type: 'string' },
    { name: 'headline', title: 'Headline', type: 'string' },
    { name: 'order', title: 'Display Order', type: 'number' }
  ]
}
```

## Current Hero Images + Headlines to Migrate
| File | Headline |
|------|----------|
| DSC01665-Enhanced-NR-09a7a269-1500.jpg | Capturing the Essence of You |
| DSC05719-3d99fbdd-1500.jpeg | Capturing Your Worship Spirit |
| DSC06111-e9653e4f-1500.jpg | Capturing Your Timeless Love |
| DSC00299-6343c7ee-1500.jpg | Capturing Your Forever Moments |
| DSC01365-85127bf5-1500.jpg | Capturing Your Unique Craft |
| IMG_6212-d33d2886-1500.jpg | (assign or leave as variant) |

## Design Notes
- Local `src/assets/` hero images can be removed after migration is confirmed working
- This ticket completes the CMS migration — after this, zero content lives in the codebase

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023
- Related: KNOCH-005 (hero section implementation)
