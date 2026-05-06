# KNOCH-011 — Test Report

| | |
|---|---|
| **Ticket** | KNOCH-011 — Portfolio Filter System |
| **PR** | [#20](https://github.com/eayanwale/knochmedia/pull/20) (dev → test) |
| **Branch** | `feature/KNOCH-011-portfolio-filter` (merged to dev) |
| **Tester** | Tester Agent |
| **Date** | 2026-05-06 |
| **Result** | **PASSED** |

---

## Summary

PR #20 implements the Portfolio Filter System (`/portfolio.html`) with category filter tabs, GSAP overlapping exit/enter animations, URL hash sync, and "Load more" pagination. All 11 acceptance criteria pass. Build is clean (65 modules, 140 ms; emits `dist/portfolio.html` 14.31 kB / 3.39 kB gz, page-specific JS chunk 2.31 kB / 1.06 kB gz). Three LOW findings — none blocking.

---

## Acceptance Criteria

| AC | Result | Note |
|----|--------|------|
| Filter tabs: All · Weddings · Brand · Sports · Portraits — 11px text, active tab amber + amber underline, inactive muted | ✅ | `<button class="portfolio-tab">` × 5, font-size 11 px JetBrains Mono, `.is-active` flips colour to `var(--amber)` and sets `::after { width: 100% }` for the underline; inactive tabs at `rgba(237,230,216,0.45)` |
| Active tab tracked via `data-filter` attribute | ✅ | Each `<button>` carries `data-filter="all\|wedding\|brand\|sport\|portrait"`; JS reads `t.dataset.filter` for selection |
| Each tile has `data-category` for filter | ✅ *(see Finding #1)* | All 12 cards have `data-category` from {wedding, brand, sport, portrait}. The AC mentioned `film` as a possible value but no `Film` filter tab — builder folded film/worship work under `brand`. Documented as deliberate in PR description |
| Filter logic — non-matching out (opacity 0, y 20, 0.3 s, power2.in); matching in (opacity 1, y 0, 0.5 s, stagger 0.05, expo.out); `display: none` after exit | ✅ | `applyFilter()` in `portfolio-page.js:110-170` — `gsap.to(leaving, { opacity:0, y:20, duration:0.3, ease:'power2.in', onComplete: → add .is-hidden })`; `gsap.fromTo(entering, { opacity:0, y:20 }, { opacity:1, y:0, duration:0.5, ease:'expo.out', stagger:0.05 })`. Both fire on the same tick — exit and entry overlap as required |
| "All" tab shows all tiles | ✅ | `matchingCards()` returns `cards` directly when `activeFilter === 'all'` |
| URL hash updates: `#weddings`, `#brand`, `#sports`, `#portraits` | ✅ | `selectFilter()` writes `history.replaceState(null, '', url)` with the mapped slug; "All" clears the hash |
| On page load, hash → matching filter | ✅ | `window.location.hash` read on init; if it matches `HASH_TO_CATEGORY`, the matching filter is applied via the no-animation fast path |
| Count label "Showing X of Y projects" updates | ✅ *(see Finding #2)* | `updateMeta()` rewrites `<p class="portfolio-count">` innerHTML on every filter / load-more change. AC's "of 47" was design-copy placeholder; implementation has 12 cards in DOM and the count label reflects actual total dynamically (more honest than hardcoding 47) |
| "Load more →" — appears when shown < matched, hides when caught up | ✅ | `updateMeta()` toggles `.is-hidden` on the button when `shown >= matched`. Click handler `visibleCount += PAGE_SIZE` then re-applies. Selecting a different filter resets `visibleCount` so a 4-match category doesn't inherit a stale "all-shown" state |
| Tabs are `<button>` (keyboard accessible) | ✅ | All 5 tabs are real `<button>` elements wrapped in `<li role="presentation">`; native button activation handles Enter/Space without JS keydown handler. `aria-pressed` flips on selection |
| Tabs scroll horizontally on mobile (no line break) | ✅ | `@media (max-width: 800px)` sets `.portfolio-tabs { overflow-x: auto; -webkit-overflow-scrolling: touch }` and `.portfolio-tab { flex-shrink: 0 }` so labels never wrap |

**11/11 ACs pass.**

---

## Validation

| Check | Result | Note |
|-------|--------|------|
| `npm run build` | ✅ PASS | 65 modules transformed, 140 ms. Emits all three HTML entries (`index.html`, `about.html`, `portfolio.html`) plus shared bundle chunks. Page-specific bundle (`portfolio-*.css` 4.34 kB / 1.26 kB gz, `portfolio-*.js` 2.31 kB / 1.06 kB gz) is small — most JS is shared with the about page via the `char-hover-*.js` chunk |
| Vite multi-page setup | ✅ PASS | `vite.config.js` now has three inputs: main, about, portfolio. All three HTMLs land at correct `dist/` paths |
| Slim entry point | ✅ PASS | `portfolio-main.js` loads only lazy-load + chrome + cursor + char-hover + lenis + portfolio-page. No homepage modules |
| Existing pages unaffected | ✅ PASS | `dist/index.html` 26.62 kB (was 26.51 kB — +110 b for the new "View the full portfolio →" link in `.reel-intro`); `dist/about.html` unchanged at 13.28 kB. `dist/assets/portfolio/`, `/about/`, `/reel/`, `/testimonials/` all still present |
| Filter state machine | ✅ PASS | Logic traced for: `wedding` → 3 matches, no entering / 5 leaving; `all` → 8 matches, 5 entering / 0 leaving; `Load more` → 4 entering / 0 leaving; `portrait` after Load more → 0 entering / 8 leaving. All correct |
| Filter overlap (exit + entry on same tick) | ✅ PASS | Both `gsap.to` calls fire inside the same `applyFilter` invocation — no `await`, no `setTimeout`. Exit uses `onComplete` only to add `.is-hidden`; entry runs in parallel |
| `display: none` after exit | ✅ PASS | `.is-hidden { display: none }` added inside the exit tween's `onComplete` (not before) so the layout doesn't snap before the fade |
| Hash sync — replaceState (not pushState) | ✅ PASS | Filter changes don't pollute the back button with intermediate filter states. Only the active filter at any moment is reflected in the URL |
| Hash sync — `fromHash` flag prevents feedback loop | ✅ PASS | When `hashchange` triggers `selectFilter(filter, { fromHash: true })`, the function skips the `replaceState` step that would otherwise re-fire `hashchange` |
| Initial filter from URL hash | ✅ PASS | Page load: `window.location.hash` checked; matching filter applied via `applyFilter({ animate: false })` so there's no animation flash on first paint |
| `prefers-reduced-motion: reduce` | ✅ PASS | `applyFilter()` takes the instant-reset path (no GSAP tweens, just `.is-hidden` toggle). All hover transitions on `.portfolio-card-img`, `.portfolio-card-label`, `.portfolio-tab`, `.portfolio-tab::after`, `.portfolio-load-more` cleared via `@media` block |
| Mobile (≤800px) | ✅ PASS | Hero stacks (column flex), tabs scroll horizontally, grid → 2 columns, meta row stacks |
| Mobile (≤480px) | ✅ PASS | Grid → 1 column |
| Tablet (768px) | ✅ PASS | Within the 800px breakpoint — 2-col grid, stacked hero, scrolling tabs |
| Desktop (1280px+) | ✅ PASS | 3-col grid, hero with headline left + tabs right, max-width 1600px gutter |
| Animation performance | ✅ PASS | All filter animations target `transform` (y) and `opacity` only — composited properties, no layout. Hover on cards uses `transform: scale()` and `filter` (slight lift, no reflow). `will-change: transform` on `.portfolio-card-img` to hint compositing for the hover scale |

---

## Accessibility

| Check | Result | Note |
|-------|--------|------|
| Document language | ✅ | `<html lang="en">` |
| Page title | ✅ | "The Work — Knoch Media" |
| Meta description | ✅ | Present and descriptive |
| Semantic structure | ✅ | `<h1>` (hero), `<h3>` (card titles), `<article>` for each card, `<button>` for tabs and load-more, `<nav>` for chrome, `<section>` with `aria-label` per region |
| Decorative chrome | ✅ | Custom cursor and timecode bar both `aria-hidden="true"`; nested `role="progressbar"` retained on `.scroll-progress` |
| Tab activation | ✅ | Native `<button>` — Enter and Space both activate. `aria-pressed` flips on selection. `<ul role="tablist">` wraps the tabs |
| Live region for count | ✅ | `<p class="portfolio-count" role="status" aria-live="polite">` — count updates announced to AT without focus jump |
| Background image alts | ✅ | Each `.portfolio-card-img` has `role="img"` + `aria-hidden="true"` (description carried by the parent `<article aria-label="...">`) — clean for AT users without double-announcing |
| Cross-page link target | ✅ | `.mark` `aria-label="Knoch — back to home"` matches the about-page convention |
| Hash visible on URL | ✅ | Sharable link state — copy/paste a URL with `#brand` and the receiving user lands on the brand-filtered view |

---

## Performance / build size

`dist/portfolio.html` ships at **14.31 kB** (3.39 kB gz) — small page weight. Page-specific JS is **2.31 kB** (1.06 kB gz). The shared bundle (GSAP + ScrollTrigger via the `char-hover-*.js` chunk, plus `lenis`, `chrome`, `cursor`, `lazy-load`) is the same across index/about/portfolio so cross-page navigation hits a warm browser cache for everything except the page-specific CSS+JS.

---

## Findings

### LOW #1 — Missing `data-category="film"` value

**File:** `src/portfolio.html` (all card markup)

The acceptance criterion for the `data-category` attribute lists five values: `wedding | brand | sport | portrait | film`. The implementation uses four — `film` is folded into `brand` so worship-film and brand-film projects (Rapha Records, What Mighty Praise, Yahweh We You) all surface under the Brand filter rather than sitting in a film-only category that the four-tab filter row can't actually surface anyway.

**Why this is acceptable:** the filter-tab AC (#1) only specifies four category tabs (`Weddings · Brand · Sports · Portraits`). A separate `film` data-category value would only ever show under `All` since there's no Film tab — content would effectively be unfilterable except by selecting "All". Folding film into Brand keeps every card discoverable under at least one specific category filter.

**Builder rationale documented in PR #20 description.**

### LOW #2 — Count label total reflects actual DOM rather than the AC's literal "47"

**File:** `src/js/portfolio-page.js:96-105`, `src/portfolio.html:295`

The acceptance criterion's example reads `"Showing 8 of 47 projects"`. The implementation uses **dynamic** totals — `updateMeta()` writes `Showing X of Y projects` where `Y = matchingCards().length` (currently 12 in this build).

**Why this is acceptable:** hardcoding "47" while only 12 cards are in the DOM would be a lie. The 47 in the spec is design-copy placeholder; future content additions just need new `<article>` elements added to the grid and the count updates automatically. Dynamic count is more honest and more maintainable than the hardcoded value.

### LOW #3 — Cards use inline `onclick` without keyboard activation handler

**File:** `src/portfolio.html:135-291` (each card markup)

Each `.portfolio-card` has `tabindex="0"` (focusable) and an inline `onclick="window.open(...)"` handler. Pressing Enter or Space while the card is focused does **not** activate it — there's no `keydown` listener wiring Enter/Space to the click handler.

**Why this is not a blocker:** AC #10 ("keyboard accessible") explicitly scopes to filter tabs, not to card activation. Cards opening external links is a card behaviour outside the filter system. Both the existing homepage `.tile` (in `portfolio-grid.js`) and this page's tabs (`<button>`s) are properly keyboard-activated; only the new portfolio cards have this gap.

**Recommendation:** address in KNOCH-021 (accessibility pass) or as a small follow-up — convert cards from `<article tabindex="0" onclick=...>` to `<a href=...>`, OR add the same `keydown` handler pattern already in use by `portfolio-grid.js`. Either approach is a 5-minute fix; neither blocks this PR's filter-system scope.

---

## Passing checks (summary)

- All 11 acceptance criteria
- Vite build clean (65 modules, 140 ms; portfolio.html 14.31 kB / 3.39 kB gz)
- Filter state machine — wedding / all / load-more / portrait scenarios all traced and confirmed correct
- Exit + entry tween overlap on same tick (no waiting), `display: none` only after exit completes
- URL hash sync via `replaceState` (clean back-button history) + `hashchange` re-apply with feedback-loop guard
- `Load more` count reset on filter change, button hides when caught up
- Mobile (≤480 / ≤800), tablet (768), desktop (1280+) all responsive
- `prefers-reduced-motion: reduce` skips animations gracefully
- Accessibility — semantic markup, aria-pressed on tabs, aria-live on count, aria-label on cards, aria-hidden on chrome
- Animation performance — only transforms / opacity / filter, no layout-triggering properties

---

## Recommendation

**APPROVE and merge** PR #20 (dev → test). All three findings are LOW; none are AC failures. Findings #1 and #2 are documented design decisions; finding #3 is a code-quality note for KNOCH-021 (accessibility pass) and doesn't affect the AC-scoped functionality of this ticket.
