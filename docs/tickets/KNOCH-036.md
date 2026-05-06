# KNOCH-036 — Phase 3 polish pass

## Status: IN REVIEW
## Priority: P2 (polish)
## Epic: Phase 3 polish
## Branch: feature/KNOCH-036-phase-3-polish (merged to dev)
## PR: bundled into PR #21 (KNOCH-012)

## Title
Cross-ticket polish across nav, homepage, portfolio, testimonial, about, and project page after Enoch's first walk-through of the Phase 3 build.

## Description
Bundle of small fixes and one section redesign, applied as a single polish pass after Enoch reviewed the test preview at the end of Phase 3 ticket #3 (KNOCH-012). Spans four tickets' worth of code but treated as one branch since the changes are tightly coupled (nav structure, category taxonomy, visual consistency).

## Items

### Homepage
- Interlude signature `— Enoch Knoch · Founder` → `— Enoch · Founder`
- Studio teaser section removed; "Read the story →" link relocated into the `#frame` section's `.big` H2 next to the "Eight years…" headline
- `.reel-scroll-hint` and `.reel-intro-portfolio-link` now have proper top padding so they don't visually cluster

### Nav
- `.nav-center a[aria-current="page"]` styled as amber + 100%-width underline so the active page is clearly indicated
- About / portfolio / project pages: `Work` link → `/portfolio.html` (was `/#reel`); `aria-current="page"` set on `Work` for portfolio + project pages, on `Studio` for about
- Homepage chrome stays as in-page anchors so smooth scroll still works there

### Testimonial
- Spotlight reveal mimics `#frame`'s gradient/settings/radius exactly:
  - `SPOT_RADIUS` 250 → 200 px
  - `BG_OPACITY` 0.80 → 1.0
  - `buildSpotMask` 9-stop curve → simple `radial-gradient(circle 200px at X Y, black 0%, transparent 82%)`
- `.testimonial-bg` filter retuned to match `#frame`'s `.bg-reveal`: `grayscale(0.15) brightness(0.92) contrast(1.05)`

### Portfolio
- Filter tabs reduced to `All · Weddings · Brand · Music` (Sports + Portraits removed; Music added)
- Card `data-category` attributes synced: WMP + Yahweh → music; BCF + Woodsmen → brand
- 4 portrait placeholder cards deleted from portfolio.html
- `projects.js` categories aligned; orphaned portrait stubs (senior-portrait, studio-session, family-session) removed
- BCF card image path fixed (was a Windows-path with backslashes)

### About page — chapter narrative redesign
- Replaced `.about-split` (sticky text + scrolling images) with `.about-story` (four full-bleed `.about-chapter` sections)
- Each chapter: full-bleed bg image with vignette overlay, mono label, Fraunces title, sans body
- Per-chapter ScrollTrigger reveal: bg scales from 1.12 → 1, label fades up, title fades + rises, body words split into per-word spans that stagger in (~0.6s cascade)
- Process step blocks (.about-process-step) get a fade-up stagger when section enters view
- `headline-hover` class added to `.about-process-title` elements for the smoke hover effect
- IntersectionObserver chapter-sync removed (unneeded — each chapter is now its own viewport)

### Project page
- Body 2-col grid (gallery left, metadata right sticky) replaced with a single centred metadata column
- New `.project-others` section below body with a horizontal "Other works" reel — same card vocabulary as the homepage Selected Work reel, rendered via CSS overflow-x + scroll-snap
- `project-page.js` now renders `listProjects()` (excluding current, capped at 8) into the existing `.project-gallery` element

## Acceptance criteria
- [ ] Homepage: interlude sig reads "Enoch · Founder"; reel scroll-hint and portfolio link have visible spacing; "Read the story" link sits inside the #frame headline
- [ ] Studio teaser section is gone from homepage; CSS file may remain (only `.studio-teaser-link` rule still referenced by the relocated link)
- [ ] Nav: on every non-homepage page, "Work" navigates to `/portfolio.html`; the active link is amber with a full underline
- [ ] Testimonial spotlight is now a 200px circle with simple two-stop gradient; visually similar to `#frame`'s spotlight
- [ ] Portfolio filter tabs are All / Weddings / Brand / Music; clicking each filters correctly; URL hash sync still works
- [ ] About page: 4 full-bleed chapter sections; each reveals on scroll with bg-scale + label/title/body cascade; process steps fade up in stagger; process titles wear smoke-hover effect
- [ ] Project page: single centred metadata column; below it, "Keep looking." section with a horizontal scrollable reel of other projects; cards link to `/project.html?id=…` with hover lift + filter ramp
- [ ] No console errors on any page; `npm run build` clean

## Out of scope (followups)
- `studio-teaser.css` file still linked but mostly dead — single surviving rule (`.studio-teaser-link`) could be inlined into `frame.css` and the file deleted. Leftover CSS doesn't render anything since the section markup is gone.
- About page's "By the numbers" stat row + CTA could get more text effects — current pass focused on the chapter narrative section.
- The project page's metadata column is centred but visually thin — could grow into a richer hero / body layout once Enoch's actual project copy replaces the placeholder descriptions.

## Related Tickets
- KNOCH-011 (portfolio filter — categories updated here)
- KNOCH-012 (project detail — gallery → reel here, layout retuned)
- KNOCH-013 (about page — chapter section redesigned)
- KNOCH-035 (testimonial reveal — replaced with frame-aligned settings)
