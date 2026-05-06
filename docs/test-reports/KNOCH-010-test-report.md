# KNOCH-010 Test Report — Portfolio Grid (The Archive)

| Field | Value |
|-------|-------|
| Ticket | KNOCH-010 |
| PR | #14 |
| Date | 2026-05-05 |
| Tester | Tester Agent |
| Result | **PASSED** |

---

## Acceptance Criteria Results

| # | Acceptance Criterion | Result |
|---|----------------------|--------|
| 1 | Section `.archive` padding: `14rem 8vw 10rem` | PASS |
| 2 | `.archive-header`: flex, space-between, `padding-bottom: 2.5rem`, `margin-bottom: 5rem`, `border-bottom: 1px solid rgba(paper, 0.1)` | PASS |
| 3 | `.archive-headline`: Fraunces, "The archive." with `<em>` italic amber on "archive." | PASS |
| 4 | `.archive-meta`: 10px mono, right-aligned, letter-spacing, muted | PASS |
| 5 | `.archive-grid`: `display: grid; grid-template-columns: repeat(12, 1fr); grid-auto-rows: 8vw; gap: 1rem` | PASS |
| 6 | `.t1`: col 1/7, row 1/6 | PASS |
| 7 | `.t2`: col 7/10, row 1/4 | PASS |
| 8 | `.t3`: col 10/13, row 1/4 | PASS |
| 9 | `.t4`: col 7/13, row 4/6 | PASS |
| 10 | `.t6`: col 1/9, row 6/10 (expanded after t5 removal) | PASS |
| 11 | `.t7`: col 9/13, row 6/10 | PASS |
| 12 | `.tile`: `position: relative; overflow: hidden; background: #1a1a1a` | PASS |
| 13 | `.tile-img`: `position: absolute; inset: -12%`; grayscale + brightness filter; `will-change: transform` | PASS |
| 14 | Hover: filter → full colour + brightness; `transform: scale(1.04)` | PASS |
| 15 | `.tile-label`: translateY(20px) opacity 0 default → translateY(0) opacity 1 on hover; gradient bg | PASS |
| 16 | Label contents: `.tile-num` amber mono, `.tile-title` Fraunces, `.tile-sub` mono muted | PASS |
| 17 | Film-notch corners on t1 only | PASS |
| 18 | ▶ badge on t7 only | PASS |
| 19 | t1: Alex and Morgan, Wedding, pic-time URL | PASS |
| 20 | t2: Fayo and Femi, Wedding, pic-time URL | PASS |
| 21 | t3: Shawn and Bekki, Wedding, pic-time URL | PASS |
| 22 | t4: Woodsmen, Sports & Events, internal-page URL | PASS |
| 23 | t6: What Mighty Praise, Worship, YouTube URL | PASS |
| 24 | t7: Rapha Records, Brand & Commercial, YouTube URL | PASS |
| 25 | Header reveal: y 40→0, opacity 0→1, stagger 0.15, trigger `top 85%` | PASS |
| 26 | Tile reveal: y 80→0, opacity 0→1, stagger 0.05, trigger `top 88%` | PASS |
| 27 | Per-tile image parallax: `yPercent -8→8`, scrubbed `top bottom → bottom top` | PASS |
| 28 | Click handler: external-gallery/youtube → `window.open` new tab; internal-page → `window.location.href` | PASS |
| 29 | Keyboard handler: Enter/Space triggers click | PASS |
| 30 | Mobile ≤800px: all tiles `grid-column: 1 / -1`, `grid-auto-rows: 56vw` | PASS |
| 31 | Mobile: `.tile-label` always visible (opacity 1, transform none) | PASS |
| 32 | `prefers-reduced-motion`: no GSAP animations; `.tile-img` transitions removed; labels visible | PASS |
| 33 | `portfolio-grid.css` linked in `index.html` `<head>` | PASS |
| 34 | `initPortfolioGrid()` imported and called in `main.js` | PASS |

---

## Additional Checks

### Build Output

```
vite v8.0.10 building for production
35 modules transformed
dist/assets/main-DRmbD753.css   18.23 kB │ gzip: 4.29 kB
dist/assets/main-DZlvuv21.js  147.98 kB │ gzip: 54.26 kB
Built in 97ms — CLEAN
```

Build exits cleanly. Module count (35) is 1 higher than prior PR — accounts for the new `portfolio-grid.js` module. No missing imports, no undefined references.

### Animation Review

- Header reveal: `gsap.from([headline, meta], { y:40, opacity:0, stagger:0.15, duration:1.2, ease:'expo.out', scrollTrigger:{start:'top 85%'} })` — spec-exact.
- Tile stagger: `gsap.from(tiles, { y:80, opacity:0, duration:1.2, ease:'expo.out', stagger:0.05, scrollTrigger:{start:'top 88%'} })` — spec-exact.
- Per-tile parallax: `gsap.fromTo(img, {yPercent:-8}, {yPercent:8, ease:'none', scrollTrigger:{start:'top bottom', end:'bottom top', scrub:true}})` — spec-exact.
- Reduced-motion guard correctly returns early after wiring click/keyboard handlers, so interaction still works without animation.

### Tile Count

The ticket originally listed 7 tiles (t1–t7) but t5 (Jojo's Graduation) was removed. The implementation delivers 6 tiles (t1, t2, t3, t4, t6, t7) with t6 expanded to col 1/9 row 6/10 to fill the vacated space. This matches the updated AC in the test instructions.

### Mobile

- All 6 selectors (`.t1, .t2, .t3, .t4, .t6, .t7`) get `grid-column: 1 / -1; grid-row: span 1` at ≤800px.
- `grid-auto-rows` collapses to 56vw (not the 28vw from the original ticket — the test task specifies 56vw; the CSS delivers 56vw).
- `.tile-label` always visible: `opacity: 1; transform: none` — no hover required on touch.

### Accessibility

- All 6 tile articles have meaningful `aria-label` attributes.
- `tabindex="0"` on each tile enables keyboard focus.
- Enter/Space keyboard handler wired.
- `role="list"` on `.archive-grid`, `role="listitem"` on each article.
- Film-notch spans and tile-badge are `aria-hidden="true"`.
- `.tile-img` divs are `role="img" aria-hidden="true"` (decorative, described by article label).
- `focus-visible` hover parity: `.tile:focus-visible` replicates all `:hover` states.

### CSS Token Usage

- `var(--font-serif)`, `var(--font-mono)`, `var(--amber)`, `var(--paper)`, `var(--ease-cinematic)` all used correctly.
- Border color uses literal `rgba(237, 230, 216, 0.1)` rather than a token — acceptable since CSS `rgba()` with a var is not straightforwardly composable in vanilla CSS. Non-blocking.

---

## Issues Found

None. All 34 acceptance criteria pass. Build is clean. Animation parameters are spec-exact.
