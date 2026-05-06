# KNOCH-029 — Blog Listing Page (blog.html)

## Description
Build the blog listing page with category filter tabs, search bar, and post grid. Content fetched from Sanity. Posts render with thumbnail (image or YouTube preview), title, category, and date.

## Acceptance Criteria
- [ ] `src/blog.html` created with chrome nav + footer
- [ ] Page intro (title + description) fetched from Sanity `blogSettings` singleton
- [ ] Category tabs: All + all 9 categories — filters posts client-side without page reload
- [ ] Search bar filters posts by title and excerpt in real time
- [ ] Post cards render: thumbnail, title, category label, date, excerpt
- [ ] YouTube thumbnails: extract video ID from URL and use `https://img.youtube.com/vi/{id}/maxresdefault.jpg` as thumbnail when no image is set
- [ ] Clicking a post routes to `/blog/{slug}/`
- [ ] Empty state shown if no posts match filter/search
- [ ] GSAP entrance animation on post cards (staggered fade-up on load)
- [ ] Mobile responsive — single column below 800px

## Categories
All · Client Projects · Contracts · Creative Space · Documentary · Events · Presets · Sports · Visualizer · Wedding

## Page Intro Copy (pre-fill in Sanity Studio)
**Title:** Welcome to the Blog
**Intro:** This is where the Knoch Media team share their expertise, insights, and behind-the-scenes stories. Whether it's photography and videography tips, free resources, or updates on personal and team projects, you'll find a variety of content that inspires and informs.

## Design Notes
- Category tabs styled like the portfolio filter (KNOCH-011) — consistent UI
- Search bar: amber focus ring, mono font, minimal styling
- Post grid: masonry or uniform card grid — to be decided at implementation

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023
- Blocks: KNOCH-030
