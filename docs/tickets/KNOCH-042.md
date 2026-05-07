# KNOCH-042 — Wire Portfolio + Archive Grids to Sanity

## Status: TODO
## Priority: P1 (high — closes the loop on CMS workflow)
## Epic: EPIC-007 — Post-Launch CMS Coverage

## Title
Make `galleryCollection` the single source of truth for portfolio + archive tiles, with Sanity → Vercel webhook for auto-deploys

## Description
After Phase 6 launch, the portfolio grid (`portfolio.html`) and homepage archive grid (`index.html`) are still hardcoded HTML tiles backed by `src/js/projects.js` — even though the same data already lives in Sanity's `galleryCollection` collection (powering the homepage reel via KNOCH-025). This means every new client gallery requires a code change + PR + deploy cycle, defeating the purpose of having a CMS.

This ticket migrates both grids to be Sanity-driven at build time, and connects Sanity publishes to Vercel rebuilds via a webhook so the studio workflow becomes:

1. Shoot wedding → upload to Pic-Time
2. Sanity Studio → "Add Gallery Collection" → fill in title, category, cover, Pic-Time URL, slug, order → Publish
3. Sanity webhook fires → Vercel rebuilds → live within ~2 minutes

No code change. No PR. No `projects.js` to maintain.

## Acceptance Criteria

**Build pipeline:**
- [ ] `scripts/render-projects.mjs` (already exists from KNOCH-040) extended to fetch ALL `galleryCollection` entries from Sanity at build time (existing logic only handles per-project static page emission).
- [ ] New helper or separate script generates the portfolio grid tiles into `dist/portfolio.html`. Replaces the hardcoded `<article class="portfolio-card">` block delimited by marker comments (e.g., `<!-- KNOCH-042: portfolio tiles BEGIN -->` / `<!-- KNOCH-042: portfolio tiles END -->`) so the static template stays diff-able.
- [ ] Same pattern for `dist/index.html` archive grid tiles (`#archive` section).
- [ ] Build script handles Sanity-down case: cache the last successful Sanity fetch in a fallback file (e.g., `.sanity-cache.json` in repo root, gitignored); warn but don't fail the build if Sanity is unreachable.

**Tile-router refactor:**
- [ ] `src/js/tile-router.js` reads `data-link-type` + `data-url` directly from the clicked tile's DOM attributes, removing the dependency on `getProject()` from `projects.js`.
- [ ] External-gallery tiles → `window.open(url, '_blank', 'noopener')` (new tab, matching homepage reel behavior).
- [ ] YouTube tiles → `openVideoLightbox(parseYouTubeId(url))`.
- [ ] Internal-page tiles (rare — only entries explicitly marked `linkType: internal-page` in Sanity) → navigate to `/project/<slug>` (KNOCH-040's static pages still serve as fallback).

**Sanity → Vercel webhook:**
- [ ] In Sanity Studio → Manage → API → Webhooks: add a webhook on the `galleryCollection` document type, fired on `create`/`update`/`delete`, pointing at a Vercel deploy hook URL.
- [ ] In Vercel project → Settings → Git → Deploy Hooks: create a deploy hook for the `main` branch (call it "Sanity content update").
- [ ] Document the operator setup in this ticket's notes so future content changes auto-trigger rebuilds.

**Schema:**
- [ ] Confirm `galleryCollection` schema covers the tile rendering needs (it currently does — title, category, slug, coverImage, subtitle, url, linkType, order, featured). No schema changes expected.
- [ ] If any field is needed by the tiles but missing (e.g., a per-tile aspect-ratio variant for asymmetric grid layout), add it to the schema and document the migration.

**Decommission:**
- [ ] Once portfolio + archive grids are verified Sanity-driven, delete `src/js/projects.js` and any imports of `getProject` / `listProjects` (after confirming no other consumers).
- [ ] KNOCH-040's `/project/<slug>` static pages stay (used as fallback for `linkType: internal-page` entries) — script needs to source data from Sanity now, not `projects.js`.
- [ ] Hardcoded portfolio.html tile block + index.html archive tile block removed (replaced with the marker comments the build script substitutes into).

## Verification

- [ ] Add a new `galleryCollection` in Sanity Studio (test entry, can be deleted after) → publish → wait ~2 min → confirm it appears on `https://knoch.media/portfolio.html` and `https://knoch.media/#archive` (the homepage archive grid).
- [ ] Edit an existing entry's `order` field in Sanity → publish → confirm the tile order updates on the live site after rebuild.
- [ ] Delete the test entry in Sanity → publish → confirm the tile disappears from the live site.
- [ ] Block Sanity (DevTools network throttling) before a build → confirm the build still succeeds using cached data, with a console warning.

## Design Notes

The BEGIN/END marker-comment substitution pattern keeps the static HTML files (`src/portfolio.html`, `src/index.html`) committed and diff-able — the build script only rewrites the section between the markers. Anyone reading the source sees a placeholder block ("Tiles populated from Sanity at build time — see scripts/render-projects.mjs") instead of an empty `<section>`.

Cache-on-failure makes Sanity's uptime not a build-time gating dependency. Vercel rebuilds (triggered by Git pushes OR Sanity webhooks) shouldn't fail because Sanity's API is briefly down.

KNOCH-040's `/project/<slug>` pages aren't deprecated — they remain the rendered surface for any project marked `linkType: internal-page` (where the studio wants a self-hosted detail page rather than linking out to Pic-Time / YouTube). The build script just needs a single-source data flow now.

## Tradeoffs Considered

- **Build-time Sanity fetch vs. runtime fetch**: build-time wins for SEO (tiles in initial HTML, crawler-visible) and perceived performance (no loading state). The trade-off is the rebuild dependency on a webhook — covered by the Sanity → Vercel hook.
- **Sanity webhook vs. manual rebuilds**: webhook removes operator friction. Vercel deploy hooks are free, no monthly limit on the hobby tier, and only fire on Sanity publish events.
- **Keep `projects.js` as a backup vs. delete entirely**: deleting forces the migration to be real. Keeping it open invites drift. The cache-on-failure mechanism is the actual backup.

## Related Tickets

- KNOCH-022 (Sanity init), KNOCH-023 (fetch layer), KNOCH-025 (Sanity reel — same `galleryCollection` source)
- KNOCH-040 (per-project static routes — internal-page fallback)
- KNOCH-026 (hero images — deferred; revisit if Sanity-driven hero rotation becomes valuable)

## Operator setup (Enoch)

Before implementation:
1. **Vercel deploy hook**: Project → Settings → Git → Deploy Hooks → "Create hook" → Name: "Sanity content update", Branch: `main`. Copy the URL Vercel shows.
2. **Sanity webhook**: sanity.io/manage → your project → API → Webhooks → "Create new" → URL: paste the Vercel deploy hook from step 1, Document filter: `_type == "galleryCollection"`, Trigger on: Create + Update + Delete. Save.

Hand the Vercel deploy hook URL to me and I'll add it to the ticket as a reference (it's not secret — Vercel's deploy hooks are public-but-unguessable URLs).
