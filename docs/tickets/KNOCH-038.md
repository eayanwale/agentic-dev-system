# KNOCH-038 — Custom 404 Page

## Status: IN REVIEW (bundled into PR #32 with KNOCH-037)
## Priority: P2 (medium)
## Epic: EPIC-006 — Launch & SEO

## Title
Cinematic 404: a branded "no frame found" page on Vercel

## Description
Vercel currently serves its generic 404 for unknown routes. A custom 404 in the brand voice is a small win that signals craft on a portfolio site — one of those "if you hit a dead end the studio still feels like the studio" details. Half-day ticket, no infrastructure lift.

## Acceptance Criteria

**Page:**
- [ ] New `src/404.html` — same chrome (mark + nav-overlay + timecode bar) as the rest of the site so the visitor never feels they've left the studio.
- [ ] Headline in Fraunces 300 with the same italic-amber accent treatment used elsewhere. Suggested copy:
  > "ROLL ENDED."
  > "*No frame found.*"
- [ ] Sub-text in mono, paper @ 50%: short, in-voice. e.g. "The exposure may have been removed, retired, or never developed in the first place. Try the [archive] or come back to the [studio]."
- [ ] Primary CTA: `← Back to studio` linking to `/`.
- [ ] Secondary link: `View the archive →` to `/portfolio.html`.
- [ ] Background: subtle film-grain overlay (reuse `.grain` class) over `--ink`. No hero image — empty space reinforces the "no frame" idea.
- [ ] Page loads the existing chrome / footer modules (footer variant `expanded`).

**Vercel routing:**
- [ ] `vercel.json` at repo root (or extend any existing one) with a rewrite/error handler so unknown routes serve `/404.html`. Vercel auto-honours `404.html` at the dist root for static deploys, but verify — add an explicit `routes` config if the auto-detect doesn't fire.
- [ ] `src/404.html` added to `vite.config.js` `rollupOptions.input` so the build emits it.
- [ ] Confirm via the Vercel preview that visiting a nonsense path (e.g. `/xyz`) renders the custom page with all chrome intact, and that the response status is `404` (not `200` — preserves crawler signal).

**Optional 500 page (defer if pinch on time):**
- [ ] Same shape, copy varies. "ROLL JAMMED." / "Something broke in the darkroom."

**A11y / responsive:**
- [ ] Inherits all the chrome accessibility from the existing pages.
- [ ] Mobile: chrome hamburger works, headline scales via `clamp()`.

## Design Notes

Reuse the existing CSS bundle — no new stylesheet needed. The page is deliberately sparse; the chrome carries the brand and the headline carries the message. Adding a hero image would compete with the visual punch line of an empty frame.

The `404` HTTP status matters for SEO — Google should see this page as a 404 (not a 200 page that says 404), otherwise it indexes the dead link. Vercel's static-deploy default behaviour does the right thing as long as the file is named `404.html` at the dist root.

## Tradeoffs Considered

- **Custom page vs. Vercel default**: A premium portfolio site shouldn't show a generic Vercel branded fallback. Half-day ticket; obvious win.
- **Static HTML vs. dynamic / animated**: keep it static. The visitor is here briefly; an animation would feel like the site is mocking them. Dignified silence.
- **Include vs. omit a 500 page**: 99% of error traffic is 404. 500 is nice-to-have; defer if scope is tight.

## Related Tickets

- KNOCH-003 (chrome navigation — reused as-is)
- KNOCH-015 (footer module — reused, expanded variant)
- KNOCH-021 (a11y pass — applies to this page too)
- KNOCH-037 (SEO — sitemap obviously excludes 404)
