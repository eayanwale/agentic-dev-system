# KNOCH-021 — Accessibility Pass

## Status: IN REVIEW
## Priority: P1 (high)
## Epic: EPIC-005 — Polish & Ship

## Title
Accessibility: WCAG 2.1 AA Compliance Audit and Remediation

## Description
A thorough accessibility pass across all pages and components. Target: WCAG 2.1 AA. This is not a box-checking exercise — a premium studio site that works for all users is part of the brand's professionalism. Key focus areas: keyboard navigation through the scroll-story, screen reader announcements for animated content, focus management in modals, and color contrast on the amber-on-ink palette.

## Acceptance Criteria
**Keyboard navigation:**
- [ ] Tab order follows logical DOM order on all pages
- [ ] All interactive elements (links, buttons, form fields, filter tabs, modal close) reachable via Tab
- [ ] Custom cursor replaced by browser cursor when navigating by keyboard (detect `mousedown` vs `keydown`)
- [ ] Horizontal reel keyboard-navigable: Left/Right arrow keys advance cards when reel section is focused
- [ ] Escape key closes video modal and returns focus to the triggering tile

**Focus states:**
- [ ] `:focus-visible` outline on all interactive elements — `2px solid var(--amber); outline-offset: 3px`
- [ ] No `:focus { outline: none }` without a `:focus-visible` replacement
- [ ] Focus rings visible against both ink and paper backgrounds (amber works on both)

**Screen reader:**
- [ ] All images have `alt` text — portfolio tiles: `alt="[Project title] — [category] photography by Knoch Media"`
- [ ] Decorative elements (film-grain overlay, quote mark `"`, corner decorations) marked `aria-hidden="true"`
- [ ] Loader announces "Loading" to screen readers: `role="status" aria-live="polite"` on the loader
- [ ] The frame counter (animated number) uses `aria-hidden="true"` — screen readers should not read out "01, 02, 03..." during load
- [ ] Nav REC timecode: `aria-hidden="true"` (decorative timecode)
- [ ] Scroll progress bar: `role="progressbar" aria-valuenow="{percent}" aria-valuemin="0" aria-valuemax="100"` — updated as user scrolls
- [ ] Portfolio filter tabs: `role="tablist"` + `role="tab"` + `aria-selected="true/false"` + `aria-controls="{grid-id}"`
- [ ] Animated counters (studio stats): final value read once via `aria-live="polite"` region after count completes

**Color contrast:**
- [ ] Amber (`#e8a23a`) on ink (`#0a0a0a`): ratio ~7.5:1 ✓ (AAA)
- [ ] Paper (`#ede6d8`) on ink (`#0a0a0a`): ratio ~16:1 ✓ (AAA)
- [ ] Muted paper (`rgba(237,230,216,0.45)`) on ink: ratio ~4.2:1 — passes AA for normal text ✓
- [ ] Amber on paper (used in some sections): ratio ~3.8:1 — FAILS AA for small text — use only for large text (≥18px) or decorative in those cases
- [ ] Run automated contrast check with axe DevTools on every page

**Forms (contact page):**
- [ ] All form fields have visible `<label>` elements (not just `placeholder`)
- [ ] Error messages announced via `aria-live="assertive"` region
- [ ] Required fields marked with `aria-required="true"` (not just `*` visually)
- [ ] Step transition: `aria-live="polite"` region announces `"Step 2 of 3"` when step changes
- [ ] Budget/service tile selectors: implemented as `<fieldset>` + `<legend>` + styled radio inputs (not divs)

**Modals:**
- [ ] Video lightbox: `role="dialog" aria-modal="true" aria-label="Film reel player"`
- [ ] Focus trapped inside modal while open (Tab cycles within modal only)
- [ ] `aria-hidden="true"` added to `<body>` content while modal is open
- [ ] Focus returns to triggering element on modal close

**Motion:**
- [ ] `@media (prefers-reduced-motion: reduce)`: all GSAP animations disabled or replaced with instant `opacity` transitions
- [ ] Lenis smooth scroll disabled under `prefers-reduced-motion`
- [ ] Loader counter animation replaced with a static `"Loading…"` text under reduced motion
- [ ] Horizontal reel pin behavior replaced with a simple `display: flex; overflow-x: auto` under reduced motion

**Semantic HTML:**
- [ ] Correct heading hierarchy: one `<h1>` per page, logical `<h2>`/`<h3>` for sections
- [ ] Nav landmarks: `<nav aria-label="Primary navigation">` for chrome nav
- [ ] `<main>`, `<header>`, `<footer>`, `<section aria-label="...">` used correctly
- [ ] Skip-to-content link as first focusable element: `<a href="#main-content" class="skip-link">Skip to content</a>`

## Design Notes
The `prefers-reduced-motion` implementation is the most impactful accessibility improvement for this animation-heavy site. Users with vestibular disorders can be made nauseous by parallax and pinned scroll effects. The pattern is:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

Combined with a JS check: `if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) { /* skip GSAP inits */ }`

The skip-to-content link should be visually hidden by default but become visible on `:focus`:
```css
.skip-link { position: absolute; top: -100%; left: 0; ... }
.skip-link:focus { top: 0; }
```

## Tradeoffs Considered
- Full WCAG AA vs. AAA: AA is the legal standard and appropriate target. AAA on contrast would require changing the amber muted-text uses, which would break the design. Focus on AA for text, AAA for interactive elements where possible.
- Tile selectors as `<div>` vs. `<input type="radio">`: The reference shows styled divs. Replacing them with radio inputs (visually hidden, custom-styled label) gives correct screen reader behavior and keyboard navigation for free. This is the right call even though it requires more CSS.

## Related Tickets
- All tickets — this is a final pass that touches every component
- KNOCH-003 (nav landmark, keyboard nav)
- KNOCH-005 (loader ARIA)
- KNOCH-007 (reel keyboard nav, reduced motion)
- KNOCH-012 (modal focus trap)
- KNOCH-014 (form accessibility)
- KNOCH-016 (Lenis disabled under reduced motion)
- KNOCH-020 (mobile tap target sizes)
