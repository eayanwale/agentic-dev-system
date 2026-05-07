# KNOCH-006 — Interlude / Manifesto Section

## Status: SHIPPED — Phase 2 squash to main
## Priority: P1 (high)
## Epic: EPIC-002 — Homepage

## Title
Interlude: Scroll-Driven Word-by-Word Text Reveal (Manifesto Section)

## Description
The interlude is the transitional "breathing room" section between the hero and the horizontal reel. A single editorial quote fades in word-by-word as the user scrolls, each word going from `opacity: 0.15` to `opacity: 1`. This is the "I don't photograph weddings. I attend them — quietly — and bring back what was beautiful." moment. It establishes the studio's voice before the portfolio.

## Acceptance Criteria
- [ ] Section: `height: 100vh; display: flex; align-items: center; justify-content: center; padding: 0 8vw`
- [ ] Amber category label top-left: `position: absolute; top: 18vh; left: 8vw` — `— A note before we begin`
- [ ] Quote: Fraunces 300 weight, `clamp(28px, 5vw, 64px)`, `line-height: 1.15`, max-width: `22ch`; italic amber `<em>` for emphasis word
- [ ] Signature bottom-right: `— Enoch Knoch · Founder`, 10px mono, muted
- [ ] JS splits quote text into individual word `<span class="word">` elements at init time (preserves `<em>` tags)
- [ ] All `.word` spans start at `opacity: 0.15`
- [ ] GSAP ScrollTrigger: `trigger: '.interlude'`, `start: 'top 70%'`, `end: 'center 40%'`, `scrub: 0.5`, stagger 0.04 — animates each word to `opacity: 1`
- [ ] `<em>` words receive the amber color via CSS (not inline style), so the `span.word` wrapper doesn't break italics
- [ ] Accessible: the original quote text is in the DOM (not replaced with divs), so screen readers read the full sentence

## Design Notes
Quote text: `"I don't photograph weddings. I attend them — quietly — and bring back what was beautiful."`

The word-split JS must handle mixed content (plain text nodes + `<em>` elements). Strategy: iterate `childNodes`, wrap text node words in `<span class="word">`, wrap `<em>` elements themselves in a `<span class="word">` wrapper that contains the em.

The section has no fixed height constraints beyond `100vh` — it's purely scroll-driven. On mobile, the section collapses to `min-height: 100vh` with a simpler fade-in (no per-word scrub, just a single opacity transition via IntersectionObserver fallback for performance).

## Tradeoffs Considered
- Word-by-word scrub vs. single fade: The scrubbed per-word reveal is the signature "reading along" effect from the reference. It requires splitting DOM content, which has accessibility implications — mitigated by keeping the original text flow in the DOM.
- `scrub: 0.5` vs. `scrub: true`: 0.5s lag feels more organic than instant scrub; matches the slow-reveal pacing of reading.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-005 (appears directly after hero)
- KNOCH-007 (leads into horizontal reel)
- KNOCH-020 (mobile simplification of the scrub effect)
