# KNOCH-031 — Hotfix: Vite publicDir for runtime-string assets

## Status: IN PROGRESS
## Priority: P0 (production broken)
## Epic: Hotfix

## Title
Move runtime-string-referenced image assets (`portfolio/`, `reel/`) into `src/public/` so Vercel build emits them.

## Description
The deployed Vercel preview returned 404 for every archive tile image except the one Sanity-CDN-hosted one. Root cause: Vite's build pipeline only emits files it can trace at build time — imports in JS, `url()` in CSS files, inline `<style>` `url()` in HTML, and `<link rel="preload" href>`. It does **not** scan custom attributes such as `data-bg`, which is exactly how the archive grid (`src/index.html`) and `lazy-load.js` reference portfolio covers.

Result: 6 of 7 archive tiles silently 404 in production. The bug never surfaced locally because `npm run dev` serves files from `src/` directly without going through the build step.

The Sanity CMS CORS issue (testimonials blank, only 3 reel cards instead of 5) is a separate concern handled outside this ticket — it requires allowlisting the deployed origins in the Sanity dashboard, not a code change.

## Acceptance Criteria
- [ ] `src/assets/portfolio/` moved to `src/public/assets/portfolio/`
- [ ] `src/assets/reel/` moved to `src/public/assets/reel/` (defensive — currently CSS-only refs work, but future runtime refs would silently break)
- [ ] No HTML/CSS/JS edits required — `/assets/portfolio/...` and `/assets/reel/...` paths still resolve at runtime
- [ ] `npm run build` emits literal `dist/assets/portfolio/cover-*.jpg` (not hashed) — verified
- [ ] Vercel preview deploy renders all 7 archive tiles and the interlude film strip
- [ ] No regression on dev server (`npm run dev`)

## Design Notes
- `src/public/` is Vite's default `publicDir` when `root: 'src'` is set. No `vite.config.js` change needed.
- Other `src/assets/` subdirs (`hero/`, `logo/`, `studio/`, `testimonials/`) intentionally **not** moved: `studio/banner.jpg` is referenced via CSS `url()` (Vite handles it), the rest are currently unused.
- Future asset placement rule documented in memory: runtime-string paths → `src/public/`; CSS `url()` and JS imports → `src/assets/`.

## Tradeoffs Considered
- **Move only `portfolio/` (minimal) vs. move `portfolio/` + `reel/` (defensive)**: chose defensive. `reel/` currently works because `reel-01.png` is referenced via CSS `url()` and `<link rel="preload">`, both of which Vite rewrites. But if anyone later adds a JS string ref to `reel-02..06`, it would break silently the same way. Cheap to prevent, costly to debug twice.
- **Move vs. set `publicDir: 'assets'` in vite.config.js**: rejected — would conflate Vite's own hashed-output dir name (`/assets/...`) with the public dir's purpose, confusing for future readers.
- **Move vs. refactor `data-bg` to JS imports**: rejected as scope creep — the `data-bg` lazy-load pattern is intentional and shared with Sanity-driven tiles.

## Related Tickets
- KNOCH-010 (portfolio archive grid — original `data-bg` consumer)
- KNOCH-007 (horizontal reel — uses Sanity CDN, this fix is defensive)
- KNOCH-030 (phase 2 — added the archive grid that exposed this bug on first real Vercel build)
