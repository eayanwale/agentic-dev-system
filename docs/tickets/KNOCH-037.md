# KNOCH-037 — SEO Basics

## Status: IN REVIEW
## Priority: P1 (high)
## Epic: EPIC-006 — Launch & SEO

## Title
Sitemap, Robots, and per-project Article Schema

## Description
KNOCH-019 covered the per-page meta basics (canonical URLs, Open Graph, Twitter cards, LocalBusiness JSON-LD on the homepage). This ticket layers on the second tier of SEO hygiene — sitemap, robots, and structured data on the project detail page — so on launch day the site is fully crawlable and Google has rich-result eligibility for individual films.

## Acceptance Criteria

**Sitemap + robots:**
- [ ] `src/public/sitemap.xml` lists every static entry: `/`, `/about.html`, `/portfolio.html`, `/contact.html`. Project-detail URLs are NOT included here — they're query-string driven (`?id=…`); KNOCH-040 covers static project routes and adds them then.
- [ ] Each entry has `<loc>`, `<lastmod>` (yyyy-mm-dd), and a sensible `<priority>` (homepage 1.0, others 0.8).
- [ ] `src/public/robots.txt` allows all user-agents and references the sitemap URL: `Sitemap: https://knoch.media/sitemap.xml`.
- [ ] Both files served at root (`/sitemap.xml`, `/robots.txt`) — Vite already serves `src/public/` as the static root.

**Article schema on /project.html:**
- [ ] `Article` JSON-LD block injected at runtime by `project-page.js` once the project is resolved from the URL.
- [ ] Required fields: `@type: "Article"`, `headline: project.title`, `image: project.cover` (absolute URL), `datePublished: project.date`, `author: { @type: "Organization", name: "Knoch Media" }`, `publisher: { @type: "Organization", name: "Knoch Media", logo: { @type: "ImageObject", url: ... } }`.
- [ ] Optional but recommended: `description: project.description`, `keywords: project.category`.
- [ ] Inserted via a `<script type="application/ld+json">` element appended to `<head>` once the project is known. No insertion if the URL has no valid `?id=` (project-page.js redirects in that case anyway).

**Heading hierarchy + alt text audit:**
- [ ] One `<h1>` per page; hierarchical h2/h3 below it; no skipped levels.
- [ ] Every `<img>` has a meaningful `alt`. Decorative bg-image divs use `alt=""` + `role="img"` + `aria-hidden="true"` (most already do).
- [ ] Coordinate with KNOCH-021 (a11y) — overlap acceptable, no duplicate fixes.

**Verification:**
- [ ] Validate JSON-LD with Google's Rich Results Test (manual; document URL in PR description).
- [ ] Validate sitemap with `xmllint --noout` or an online validator.
- [ ] Confirm `robots.txt` resolves at `https://<deploy>/robots.txt` and references the sitemap.

## Design Notes

The sitemap is checked in (not generated at build) so it stays diff-able and reviewable. Switch to a build-time generator once KNOCH-040 introduces static project routes — at that point regenerating per-build makes more sense.

JSON-LD goes into the page DOM at runtime (not preloaded) because it's per-project and depends on the URL parameter. Added byte cost is ~500 B once filled in — negligible.

## Tradeoffs Considered

- **Build-time sitemap generator vs. checked-in static**: Static is simpler and works fine for ≤10 URLs. Switch when the URL count grows.
- **Article vs. CreativeWork JSON-LD**: Article fits photography/film projects better and is what Google's docs sample for portfolio-type content.

## Related Tickets

- KNOCH-019 (per-page meta basics — already shipped)
- KNOCH-021 (a11y pass — heading + alt audit overlaps)
- KNOCH-040 (per-project static routes — adds project URLs to sitemap)
