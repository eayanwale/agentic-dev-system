# KNOCH-018 — Instagram Embed Integration

## Status: DEFERRED
## Priority: P3 (nice-to-have)
## Epic: EPIC-004 — Integrations
## Deferred: 2026-05-06 — Enoch decided a footer Instagram icon link to @knochmedia_ is enough; live feed adds complexity (paid embed service, FB Graph API, or manual quarterly screenshot updates) without meaningful UX gain on a portfolio site. KNOCH-015 (footer) covers the link. Revisit if Enoch ever wants live posts surfaced on-site — Behold.so is the lightest path.

## Title
Instagram Integration: Feed Display and Profile Link

## Description
The current knochmedia.xyz site has Instagram integration. The new site should surface Instagram content in a "Latest from Instagram" section (likely on the about or homepage), showing the most recent posts as a grid of image cards that link to Instagram. Due to Meta's API restrictions, the implementation uses a lightweight third-party service or the basic embed approach rather than a raw API integration.

## Acceptance Criteria
**Instagram feed section:**
- [ ] Section heading: `"— Follow the work"` amber label, `"@knochmedia"` Fraunces headline linking to Instagram profile
- [ ] Grid: 6 images in a 3×2 or 6×1 horizontal scroll layout, `aspect-ratio: 1/1`, with hover reveal showing post caption truncated to 80 characters
- [ ] Images: fetched via a feed aggregator service (Behold.so, SnapWidget, or similar) that doesn't require OAuth per visitor — OR use static screenshots updated manually quarterly
- [ ] Fallback: if feed service fails or images don't load, section shows `"See our latest work on Instagram →"` plain text link
- [ ] "See all on Instagram →" CTA link at bottom
- [ ] Instagram profile link: `https://instagram.com/knochmedia` (or the correct handle — verify from knochmedia.xyz)

**Footer Instagram link:**
- [ ] Instagram handle and link already covered in KNOCH-015 — this ticket is only for the feed section

**Implementation strategy:**
- [ ] Research and choose between: (a) Behold.so free tier (token-based, no OAuth per user), (b) EmbedSocial, (c) Manual static image grid updated by Enoch
- [ ] If using a third-party embed: load their script lazily (IntersectionObserver on the section container — only load when section enters viewport)
- [ ] If using static images: document the update process in `src/assets/instagram/README.md`

## Design Notes
Meta's Instagram Graph API requires OAuth per-user and is not suitable for a public portfolio feed without a backend. Third-party aggregators solve this by storing a long-lived access token on their server. Behold.so has a generous free tier (up to 25 images, updates every 24hrs) that suits a solo photographer's needs.

The hover reveal should show: post date, truncated caption, and a faint Instagram icon — all in the same dark design language as the portfolio tiles.

If a third-party service adds a watermark or branding on the free tier, evaluate whether the premium tier ($X/mo) is worth it vs. a static manual grid.

## Tradeoffs Considered
- Third-party aggregator vs. Meta API: Meta's API requires app review, OAuth flow, and a backend — entirely disproportionate for showing 6 recent photos. A third-party service trades a minor vendor dependency for massive complexity reduction.
- Live feed vs. static grid: A live feed keeps content fresh automatically. A static grid is zero-dependency but requires manual updates. Recommend live feed via Behold.so; fall back to static if the service proves unreliable.

## Related Tickets
- KNOCH-013 (Instagram feed likely appears on about page or homepage)
- KNOCH-015 (Instagram link in footer)
- KNOCH-019 (feed images must be lazy-loaded)
