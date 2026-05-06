# KNOCH-013 — About / Story Section

## Status: QA PASSED
## Priority: P2 (medium)
## Epic: EPIC-003 — Secondary Pages
## Branch: feature/KNOCH-013-about-story-section
## PR: #19 (dev → test) — merged
## Test report: docs/test-reports/KNOCH-013-test-report.md

## Title
About / Studio Story Section: Split-Layout with Pinned Text and Scrolling Image Column

## Description
The "about" section tells the studio's origin story with a split-screen layout: the left column of text is sticky (stays in view), while the right column scrolls through a sequence of images as the user reads down. This is a dedicated `about.html` page AND a truncated "studio" section on the homepage (the pinned-frame section covers the numbers; this covers the personal narrative).

## Acceptance Criteria
**Homepage studio teaser (appears between pinned-frame and testimonial):**
- [ ] Not a full about section — just a 2-sentence brand statement with a "Read the story →" link
- [ ] No separate layout component needed; can be a simple text block with amber CTA link

**`about.html` full page:**
- [ ] Page entry: same loader or instant reveal (no full film-counter loader — a lighter 0.5s fade-in)
- [ ] Hero: large Fraunces headline `"A working studio."` with subhead location/year
- [ ] Split layout: `display: grid; grid-template-columns: 1fr 1fr` on desktop
- [ ] **Left col (sticky)**: `position: sticky; top: 10vh; height: 80vh` — contains the personal narrative text with multiple paragraphs, broken into chapters with amber section labels
- [ ] **Right col (scrolls)**: 4–5 full-width images stacked vertically with `aspect-ratio: 3/4`, each with a subtle caption
- [ ] Right column images trigger the left column chapter-heading color change (amber highlight) when they scroll into view
- [ ] "By the numbers" stat row: 3 inline stats (47 projects, 8 years, 12k frames) in Fraunces large numerals — reuses same counter animation from KNOCH-008
- [ ] Process section: 3 numbered steps — `01 The inquiry`, `02 The shoot`, `03 The delivery` with short descriptions
- [ ] CTA at bottom: `"Ready to make a roll?"` → links to contact page
- [ ] Nav on about page: same chrome overlay from KNOCH-003

## Design Notes
The studio narrative (placeholder — to be replaced with Enoch's actual copy):
- Chapter 1: How Knoch started (2018, UMD, first wedding)
- Chapter 2: The philosophy (attending vs. photographing)
- Chapter 3: The gear/process (film-first mindset even shooting digital)
- Chapter 4: Who we work with (qualifying criteria)

Images for the scrolling right column should be pulled from `src/assets/about/` — mix of behind-the-scenes, detail shots, and photographer portraits.

The chapter-change interaction: use an IntersectionObserver (not ScrollTrigger) to detect which right-column image is in view — lighter than GSAP for a simple class toggle.

## Tradeoffs Considered
- Sticky left vs. GSAP pinning: `position: sticky` is semantically clean and doesn't require JS. The chapter highlight on scroll is the only JS needed here.
- Separate `about.html` vs. single-page section: separate page keeps `index.html` focused on the story-scroll homepage experience. The `about.html` page has its own scroll narrative.

## Related Tickets
- KNOCH-001 (about.html as Vite entry point)
- KNOCH-003 (shared chrome nav)
- KNOCH-008 (shares counter animation module)
- KNOCH-014 (CTA links to contact page)
