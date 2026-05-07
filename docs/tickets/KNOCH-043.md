# KNOCH-043 — Sanity-driven `/project/<slug>` detail pages

## Status: 🚀 SHIPPED — v0.7.2 (commit pending)
## Priority: P1 (high — completes the CMS-on-the-portfolio loop)
## Epic: EPIC-007 — Post-Launch CMS Coverage

## Title
Restore the cinematic on-site project detail page for Sanity-driven entries

## Description
KNOCH-042 wired the works grid to Sanity but routed `external-gallery` tile clicks straight to Pic-Time, skipping the on-site `/project/<slug>` detail page that KNOCH-040 introduced. Enoch flagged that he liked the cinematic landing experience (chrome → cover → "View full gallery" CTA) and wanted it back for Sanity entries too.

This ticket extends the `/project/<slug>` static-page emission to cover Sanity `galleryCollection` entries (in addition to projects.js entries), routes external-gallery tiles through the on-site page, and adds a Vite dev middleware so the path-based URL works in `npm run dev` as well as production.

## Acceptance Criteria

**Build pipeline:**
- [x] `scripts/render-projects.mjs` extended to fetch all `galleryCollection` from Sanity at build time (Node fetch, no SDK).
- [x] For each Sanity entry whose slug isn't already in projects.js, emit `dist/project/<slug>.html` with per-project canonical / og:image / Article JSON-LD baked in.
- [x] Sitemap regenerated to include both projects.js + Sanity slugs (no duplicates; idempotent across rebuilds; legacy KNOCH-040 marker block stripped).
- [x] Sanity unreachable at build time → continue with just projects.js entries; build doesn't fail.

**Runtime page:**
- [x] `project-page.js` made async. Tries `getProject(slug)` first; falls back to `getGalleryCollectionBySlug(slug)` runtime fetch if the slug isn't in projects.js.
- [x] Sanity entries map to the same project shape the renderer expects (title / category / cover / location / galleryUrl / youtubeId). Description / images[] / frames stay empty — renderer already gracefully skips missing fields.
- [x] "Other works" reel sources from the same place (Sanity → other Sanity entries; projects.js → other projects.js entries) so links never point at non-existent slugs.

**Routing:**
- [x] `tile-router.js`: external-gallery tiles now run the cinematic transition + navigate to `/project/<slug>` instead of opening the URL in a new tab. The detail page hosts the "View the full gallery →" CTA that opens the external URL.
- [x] YouTube tiles unchanged (still open the lightbox directly — cleaner UX than navigating to a thin static page).
- [x] Internal-page tiles unchanged (still navigate to `/project/<slug>`).

**Dev-mode parity:**
- [x] `vite.config.js`: new `projectPathRewritePlugin` middleware. Rewrites `/project/<slug>` → `/project.html?id=<slug>` in dev so the same URL pattern works in dev and production. Browser address bar still shows `/project/<slug>`; Vite resolves the file behind the scenes.
- [x] `vercel.json`'s legacy `/project.html?id=<slug>` redirect (KNOCH-040) still in place for any external link that hasn't migrated.

**Sanity helper:**
- [x] `src/js/sanity.js`: new `getGalleryCollectionBySlug(slug)` that returns the matching doc (or null). Uses the existing `fetchQuery` plumbing with a parameterised GROQ query.
- [x] `fetchQuery` extended to accept a params object (passed as `$name=value` query string) so callers don't have to string-interpolate values into GROQ.

## Verification

- Visit `/project/alex-and-morgan-s-wedding` (or any Sanity slug) on production → branded landing page renders with cover, title, category, subtitle. "View the full gallery →" CTA opens the Pic-Time URL.
- View-source shows per-project canonical / og:image / Article JSON-LD.
- Click any portfolio tile → cinematic transition → on-site detail page (no longer jumps straight to Pic-Time).
- Run Google Rich Results Test on a Sanity-slug URL → Article schema detected.
- `/sitemap.xml` lists both projects.js and Sanity slugs (no duplicates).
- `npm run dev` → click a tile → URL becomes `/project/<slug>` → page renders (Vite middleware rewrite handles the path).

## Tradeoffs Considered

- **Sanity-only static pages vs. richer pages with description + gallery**: Sanity's `galleryCollection` schema doesn't have description or images[] fields. Adding them is a future ticket; for now the on-site page is a "preview + redirect" surface that still feels branded.
- **Build-time fetch vs. runtime hydration**: KNOCH-042 went runtime to fix dev mode. KNOCH-043 goes build-time for the static project pages because per-project og:image needs to be in the static HTML for social-card crawlers (Twitter, Slack, iMessage don't run JS on link previews). The Vite dev middleware bridges the gap so dev still works.

## Related Tickets

- KNOCH-040 — Per-project static routes (projects.js source)
- KNOCH-042 — Portfolio grid Sanity wiring (sibling)
- KNOCH-027 — About page Sanity wiring (same runtime-fetch pattern)
