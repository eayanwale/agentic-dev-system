# KNOCH-032 — Scroll polish: testimonial bg, reverse type, reel intro, archive float

## Status: IN PROGRESS
## Priority: P1 (polish)
## Epic: Polish

## Title
QA-pass cinematic polish across testimonial, reel, and archive sections.

## Description
Four small motion / content tweaks bundled in one PR after the test preview shipped:

1. **Testimonial backgrounds** — replace the client-name → portfolio-cover lookup with a sequential `testimonial-XX.jpg` map keyed by Sanity's `order asc` index. Files live in `src/public/assets/testimonials/` (moved from `src/assets/`, same Vite publicDir reasoning as KNOCH-031).
2. **Testimonial reverse type** — scroll-up while a testimonial's text is partially or fully revealed now un-reveals the most recent batch of words instead of immediately advancing to the previous testimonial. Mirrors the typing motion in reverse.
3. **"Selected work." intro punch-up** — split the headline into per-character spans and stagger their yPercent/opacity rise. Bumps label/desc/hint y-offsets and durations so the entry reads on a fast scroll-by, where the previous full-clip wipe collapsed into a single flash.
4. **Archive tile float** — subtle infinite yPercent yoyo on each archive tile, with pseudo-random durations and negative delays so phases desync. Range stays at ~3% of tile height (≤ ~6 px on smaller tiles, more readable on t1/t6/t7).

## Acceptance Criteria
- [ ] `src/public/assets/testimonials/testimonial-01..05.jpg` ship in `dist/assets/testimonials/` literal-named (verified)
- [ ] Each Sanity testimonial maps to its index-ordered image (testimonial-01 → order 1, etc.)
- [ ] Scroll-up un-reveals 8 words per gesture; releases scroll only at first testimonial AND fully un-revealed
- [ ] "Selected work." headline reads as a per-character cascade, not a single clip-wipe flash
- [ ] All 7 archive tiles bob independently; magnetic hover and image parallax still work
- [ ] No regression on `prefers-reduced-motion` (float skips, char split skips, reveal/un-reveal stays instant)

## Design Notes
- **Float on `yPercent`, entry on `y` (px)** — different transform components in GSAP, so they compose without overwriting. Same trick used to keep `.tile-img` parallax from fighting magnetic-cursor pan.
- **Per-char split via TreeWalker** — preserves the existing `<em>work.</em>` so the amber italic styling survives the split. Avoids regex-on-innerHTML hazards.
- **Reverse reveal = `unrevealBatch(newIdx, oldIdx)`** — only animates the slice that actually transitioned, so already-hidden words don't re-animate. Stagger reversed (from end → start) so the visual cue moves in the opposite direction of forward typing.

## Tradeoffs Considered
- **Tile float as CSS animation vs GSAP** — GSAP wins because CSS transform composes-by-replacement with GSAP transform on the same element, causing the magnetic hover and entry tweens to fight the float. GSAP's per-property tracking handles them as independent.
- **Reverse reveal vs immediate previous-testimonial on scroll-up** — the existing immediate-jump felt inconsistent with the deliberate scroll-write-down pacing. Mirroring the typing motion makes the section feel like a reversible scrub track rather than a slide deck.
- **Naming testimonial bg files by client** — rejected: testimonials roster shifts in Sanity, alphabetical order would drift, the index-by-order rule keeps content + asset coupled by a single ordinal.

## Related Tickets
- KNOCH-009 / KNOCH-024 (testimonial section + Sanity wiring — modified here)
- KNOCH-007 (reel section — intro animation modified)
- KNOCH-010 (archive grid — tile float added)
- KNOCH-031 (Vite publicDir hotfix — same `src/public/` pattern reused for testimonials)
