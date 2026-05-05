# KNOCH-009 — Testimonial Section

## Status: TODO
## Priority: P2 (medium)
## Epic: EPIC-002 — Homepage

## Title
Testimonial Pull-Quote Section with Scroll-Triggered Stagger Reveal

## Description
A full-width centered testimonial section between the pinned frame and the portfolio grid. Large quotation mark, editorial pull-quote in Fraunces, client attribution. Scroll-triggered stagger reveal. Simple but typographically impactful — it breaks the visual rhythm and adds social proof at exactly the right moment (after studio credentials, before the full archive).

## Acceptance Criteria
- [ ] Section: `padding: 14rem 8vw; text-align: center`
- [ ] Large `"` mark: Fraunces 8rem `line-height: 0.5`, amber color — decorative, `aria-hidden="true"`
- [ ] Quote: Fraunces 300, `clamp(28px, 4vw, 56px)`, `line-height: 1.25`, `letter-spacing: -0.015em`, max-width `22ch`, centered; `<em>` phrase in italic amber
- [ ] Quote text: `"The final product turned out better than we had imagined. We knew we made the right decision."`
- [ ] `<em>` on: `better than we had imagined.`
- [ ] Attribution: `— Marcus T. · Brand Director · 2025`, 10px mono, 0.3em LS, muted paper, `margin-top: 3rem`
- [ ] Scroll reveal: `gsap.from('.testimonial > *', { y: 40, opacity: 0, duration: 1.2, stagger: 0.15, ease: 'expo.out' })` triggering at `top 75%`
- [ ] Section has a subtle top border: `1px solid rgba(paper, 0.08)`

## Design Notes
This testimonial appears in both reference concepts (the cinematic `final demo.html` and the Aperture `knoch_homepage_redesign_mockup.html`). Its presence in both references confirms it's a required content element.

Future expansion: this section could become a testimonial carousel (3–5 testimonials), but for now a single impactful quote is stronger than a carousel that users skip.

The `> *` selector on the reveal targets the three direct children (the quote mark div, the p.quote, and the attribution div) giving them a staggered reveal.

## Tradeoffs Considered
- Single testimonial vs. carousel: one deliberate, fully-styled testimonial reads as more premium than a rotating widget. Client names and photos can be added later; for now, the name + role + year is sufficient.

## Related Tickets
- KNOCH-002 (tokens), KNOCH-008 (follows pinned frame)
- KNOCH-010 (portfolio grid follows)
