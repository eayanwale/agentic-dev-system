# KNOCH-022 — Sanity CMS: Project Init + Schema Definitions

## Status: DONE — merged to test (2026-05-05)
## Branch: feature/KNOCH-022-023-sanity-cms-setup

## Description
Initialize a Sanity project inside the repo and define schemas for all manageable content types. This is the foundation ticket — all subsequent CMS tickets depend on it.

## Acceptance Criteria
- [ ] `studio/` directory created at repo root via `sanity init`
- [ ] Sanity project linked to Enoch's account and `production` dataset created
- [ ] `testimonial` schema defined and deployed to Sanity Studio
- [ ] `galleryCollection` schema defined and deployed to Sanity Studio
- [ ] All 5 real testimonials entered into Sanity Studio manually
- [ ] `.env` file at repo root holds `SANITY_PROJECT_ID` and `SANITY_DATASET`
- [ ] `studio/` has its own `.gitignore` excluding `node_modules/`
- [ ] `SANITY_PROJECT_ID` and `SANITY_DATASET` added to `.gitignore` or stored safely (no secrets committed)

## Schema Definitions

### testimonial
```js
{
  name: 'testimonial',
  title: 'Testimonials',
  type: 'document',
  fields: [
    { name: 'clientName', title: 'Client Name', type: 'string' },
    { name: 'clientType', title: 'Client Type', type: 'string',
      options: { list: ['Individual', 'Company'] }
    },
    { name: 'quote', title: 'Quote', type: 'text' },
    { name: 'order', title: 'Display Order', type: 'number' }
  ]
}
```

### galleryCollection
```js
{
  name: 'galleryCollection',
  title: 'Gallery Collections',
  type: 'document',
  fields: [
    { name: 'title', title: 'Collection Title', type: 'string' },
    { name: 'slug', title: 'Slug', type: 'slug', options: { source: 'title' } },
    { name: 'category', title: 'Category', type: 'string',
      options: { list: ['Wedding', 'Portrait', 'Sports & Events', 'Worship', 'Brand & Commercial'] }
    },
    { name: 'coverImage', title: 'Cover Image', type: 'image', options: { hotspot: true } },
    {
      name: 'photos',
      title: 'Photos',
      type: 'array',
      of: [{
        type: 'image',
        options: { hotspot: true },
        fields: [
          { name: 'alt', title: 'Alt Text', type: 'string' }
        ]
      }]
    },
    { name: 'featured', title: 'Featured on Homepage', type: 'boolean' },
    { name: 'order', title: 'Display Order', type: 'number' }
  ]
}
```

## Design Notes
- Keep `studio/` as a subdirectory of the repo — easier to version schemas alongside code
- Use Sanity's free tier (no credit card needed)
- Dataset name: `production`
- Do NOT use Sanity's hosted Studio URL for now — run locally with `sanity dev`

## Related Tickets
- Blocks: KNOCH-023, KNOCH-024, KNOCH-025, KNOCH-026
