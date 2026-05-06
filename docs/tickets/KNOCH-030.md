# KNOCH-030 — Blog Post Detail Page (blog-post.html)

## Description
Build the individual blog post page. Renders Portable Text body including YouTube embeds, Instagram embeds, and images. Shows related posts at the bottom. Category tabs + search bar persist at top for navigation.

## Acceptance Criteria
- [ ] `src/blog-post.html` created — reads `?slug=` query param to fetch the correct post
- [ ] Fetches post via `getBlogPost(slug)` — includes resolved related posts
- [ ] Renders: title, category label, date, full body (Portable Text)
- [ ] Portable Text block rendering handles:
  - Standard text (paragraphs, headings, bold, italic, lists)
  - Images with optional captions
  - YouTube embeds: lazy `<iframe>` injected on click (same pattern as KNOCH-017)
  - Instagram embeds: renders `<blockquote class="instagram-media">` + loads Instagram embed script
- [ ] Related posts section shows up to 3 cards at bottom (thumbnail, title, date)
- [ ] Category tabs + search bar at top link back to `/blog/#{category}`
- [ ] Scroll progress indicator (thin amber line at top of viewport)
- [ ] GSAP: title reveal on load, body fades in paragraph by paragraph on scroll

## Design Notes
- YouTube iframe: lazy inject — show poster image first, swap on click (no autoplay)
- Instagram embed: use Instagram's oEmbed script — loads only on this page, not globally
- Portable Text renderer is a plain JS function, not a framework component

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023, KNOCH-029
- Related: KNOCH-017 (YouTube lazy embed pattern)
