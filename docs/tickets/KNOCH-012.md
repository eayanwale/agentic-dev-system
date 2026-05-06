# KNOCH-012 — Project Detail View

## Status: IN REVIEW
## Priority: P2 (medium)
## Epic: EPIC-003 — Secondary Pages
## Branch: feature/KNOCH-012-project-detail-lightbox
## PR: #21 (dev → test)

## Title
Project Detail: Full-Screen Lightbox / Dedicated Project Page

## Description
When a user clicks a portfolio tile (or reel card), they need to see the full project — a gallery of images, project description, and a prompt to inquire. For films/video tiles, this opens the YouTube embed in a lightbox. For photo projects, it opens a dedicated `project.html` page (or a full-screen overlay panel) with a scrollable image gallery, project metadata, and a CTA to the inquiry form.

## Acceptance Criteria
- [ ] Clicking a `.tile` or `.reel-card` navigates to `project.html?id=XXXX` (or uses URL hash routing)
- [ ] `project.html` page structure:
  - Back navigation: `← Back to work` (returns to portfolio with filter state preserved via URL hash)
  - Full-bleed hero image with project title overlay
  - Two-column layout: image gallery left (scroll), project metadata right (sticky)
  - Metadata: category tag, project title, location, date, frame count, description paragraph
  - Image gallery: vertical scroll of full-width images with lazy loading
  - CTA: `"Book a similar project →"` links to `contact.html` with `?type=wedding` (or relevant category) pre-filled
- [ ] **Video lightbox** (for film tiles like The Hartleys trailer): clicking the tile opens a full-screen modal with a YouTube embed; `Escape` key or clicking outside closes it; modal traps focus (WCAG)
- [ ] Page transition: the clicked tile expands to fill the viewport (GSAP `fromTo` on clone), then fades to the new page (or the overlay appears from behind the expanded tile)
- [ ] Back button uses browser `history.back()` — does not reload the portfolio page
- [ ] Project data stored in a `src/js/projects.js` module (array of objects) — no CMS for now
- [ ] First 3 projects fully populated; remaining tiles are placeholders

## Design Notes
The "expanding tile" page transition is the "wow" interaction for this ticket. Technique: on tile click, clone the tile element, absolutely position the clone over the original, then animate it to `top: 0; left: 0; width: 100vw; height: 100vh` with GSAP, then fade in the actual project content beneath. This avoids a jarring page navigation cut.

For the video lightbox: YouTube embed with `?autoplay=1&rel=0&modestbranding=1` params. The embed container should be `aspect-ratio: 16/9; width: min(90vw, 1200px)`.

`projects.js` data shape:
```js
{ id: 'maya-james', category: 'wedding', title: 'Maya & James', location: 'Eastern Shore', date: 'October 2025', frames: 312, description: '...', images: ['...'], youtubeId: null }
```

## Tradeoffs Considered
- Dedicated page vs. overlay panel: Dedicated page is better for SEO (each project has its own URL) and social sharing. The expanding transition bridges the visual gap. For video, the lightbox keeps the user in context.
- Static project data vs. CMS: JSON/JS module is the right call until there are 10+ projects requiring frequent updates. At that point, a headless CMS (Contentful, Sanity) can be introduced.

## Related Tickets
- KNOCH-001 (project.html added as Vite entry point)
- KNOCH-010, KNOCH-011 (source of the clickable tiles)
- KNOCH-017 (YouTube embed in lightbox)
- KNOCH-019 (image lazy loading in gallery)
- KNOCH-021 (focus trap in modal, back-nav accessibility)
