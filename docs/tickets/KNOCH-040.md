# KNOCH-040 — Per-project SEO + Static Project Routes

## Status: 🚀 SHIPPED — v0.7.0 (commit 0560d37)
## Priority: P3 (nice-to-have, post-launch)
## Epic: EPIC-006 — Launch & SEO

## Title
Build-time pre-rendered `/project/<slug>` routes with per-project og:image and Article schema

## Description
`/project.html?id=alex-morgan` works today and Google does index query-string URLs, but a static `/project/alex-morgan` route is meaningfully better SEO: cleaner shareable URLs, per-project `og:image`, per-project canonical URL, easier social-card preview targeting. This ticket adds a build-time render step that emits one HTML file per project entry in `projects.js`, each with its own meta + Article schema baked in.

**Defer until post-launch.** The current setup works. This ticket is an upgrade once there's traffic data and the studio wants to optimise per-project share previews.

## Acceptance Criteria

**Build pipeline:**
- [ ] New `scripts/render-projects.mjs` (or a Vite plugin) — reads `src/js/projects.js`, opens `src/project.html` as a template, and emits `dist/project/<slug>.html` for every project, each with:
  - Per-project `<title>`: `<title>{{project.title}} — Knoch Media</title>`
  - Per-project `<meta name="description">`: first sentence of `project.description` (truncated to ≤155 chars).
  - Per-project canonical: `<link rel="canonical" href="https://knoch.media/project/{slug}">`.
  - Per-project Open Graph: `og:title`, `og:description`, `og:url`, `og:image` (project cover, absolute URL).
  - Per-project Twitter card: same fields.
  - Per-project `Article` JSON-LD (replaces the runtime injection from KNOCH-037 — strictly faster).
- [ ] Build script wired into `package.json`: `"build": "vite build && node scripts/render-projects.mjs"`.
- [ ] Sitemap regenerated to include all `/project/<slug>` URLs alongside the static entries.

**Routing:**
- [ ] `tile-router.js`'s `_navigateToProject(slug)` updated: `/project.html?id=<slug>` → `/project/<slug>`.
- [ ] All other places that build project URLs updated to the new shape — there's currently one in `project-page.js`'s "Other works" reel, plus any portfolio-card hrefs.
- [ ] Old `/project.html?id=…` URLs continue to work (redirect to the static route via `vercel.json`) so any external links Enoch has shared don't 404.

**Runtime page:**
- [ ] `project-page.js` continues to render the page client-side. The static HTML provides only the meta / SEO layer + an empty shell; the JS still populates the gallery, hero, sticky meta column, etc. (No SSR of the body content — the dynamic logic stays as-is.)
- [ ] The static-rendered content includes a fallback `<noscript>` block with the project title + an external link, so search-engine crawlers without JS still see something meaningful.

**Verification:**
- [ ] `dist/project/alex-morgan.html` exists and contains the correct title / meta / Article JSON-LD.
- [ ] Vercel preview: navigating to `/project/alex-morgan` renders the page; share-link preview on Slack / iMessage / Twitter shows the project's cover image.
- [ ] Lighthouse SEO score on a sample project page: ≥95.

## Design Notes

The runtime / build-time split keeps the dynamic gallery + lightbox interactions exactly as they are — no SSR gymnastics. The build step is essentially a string-replace on the existing `project.html` template.

Sanity is intentionally out of scope here: project metadata stays in `projects.js` as the source of truth. If Enoch wants Sanity-driven projects later, this script just changes its data source — same render output.

## Tradeoffs Considered

- **Build-time render vs. SSR / SSG framework**: A 50-line render script does the job; bringing in Astro / Next.js / Eleventy for one small win would be over-engineering.
- **Per-project HTML vs. Vercel rewrite + dynamic meta in client**: client-side meta updates after the page loads don't help social previews (crawlers don't run JS). Static HTML is required for og:image to work.
- **Per-project `og:image` (project cover) vs. generated per-project image cards**: project covers are good enough for v1. Designed share-cards (overlay text, etc.) are a separate ticket if/when needed.

## Related Tickets

- KNOCH-012 (project page runtime — unchanged behaviour, just a static shell upgrade)
- KNOCH-037 (SEO basics — this supersedes its runtime Article-schema injection on /project.html)
- KNOCH-019 (perf — image pipeline already produces the per-project covers and WebP siblings)
