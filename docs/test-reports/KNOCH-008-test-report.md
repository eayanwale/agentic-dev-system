# KNOCH-008 Test Report — Pinned Frame: Parallax + Animated Studio Stats

| Field | Value |
|-------|-------|
| Ticket | KNOCH-008 |
| PR | #11 (dev → test) |
| Tester | Tester Agent |
| Date | 2026-05-05 |
| Result | **PASSED** |

---

## Build

```
npm run build
vite v8.0.10 — 25 modules transformed
dist/assets/main-DlR19fTc.css   13.33 kB │ gzip: 3.39 kB
dist/assets/main-DhpK7ylb.js  141.77 kB │ gzip: 52.71 kB
✓ built in 98ms — no errors, no warnings
```

---

## Acceptance Criteria Results

| AC | Description | Result | Evidence |
|----|-------------|--------|----------|
| AC-1 | `.pinned-frame`: `height: 200vh; position: relative` | **PASS** | `frame.css` lines 5–7: `position: relative; height: 200vh;` |
| AC-2 | `.sticky`: `position: sticky; top: 0; height: 100vh; overflow: hidden` | **PASS** | `frame.css` lines 13–17: `position: sticky; top: 0; height: 100vh; overflow: hidden;` |
| AC-3 | `.bg`: `position: absolute; inset: -10%; background-size: cover; will-change: transform` | **PASS** | `frame.css` lines 22–30: `position: absolute; inset: -10%; background-size: cover; will-change: transform;` |
| AC-4 | Parallax: `gsap.to('.bg', { yPercent: -15, scale: 1.05 })` scrubbed `top bottom → bottom top` | **PASS** | `frame.js` lines 20–31: `gsap.to('.pinned-frame .bg', { yPercent: -15, scale: 1.05, ease: 'none', scrollTrigger: { start: 'top bottom', end: 'bottom top', scrub: 1 } })` (selector is more specific, functionally correct) |
| AC-5 | Film-grain overlay on `.overlay` (`feTurbulence` SVG, same as hero/interlude) | **PASS** | `frame.css` lines 33–39: `.overlay` background includes `feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2'` data URI |
| AC-6 | Two-column content grid: `grid-template-columns: 1fr 1fr; padding: 0 8vw; align-items: center` | **PASS** | `frame.css` lines 42–50: `grid-template-columns: 1fr 1fr; align-items: center; padding: 0 8vw;` |
| AC-7 | Left col: amber mono meta tag `— THE STUDIO · BY THE NUMBERS` + Fraunces headline | **PASS** | `index.html` line 250: `<p class="meta-tag">— THE STUDIO &middot; BY THE NUMBERS</p>`; line 251: `<h2 class="big">Eight years. <em>Twelve&nbsp;thousand</em> frames. One pair of hands.</h2>` |
| AC-8 | Right col: three stat blocks, right-aligned (`justify-self: end; text-align: right`) | **PASS** | `frame.css` lines 79–84: `.pinned-frame .stats { justify-self: end; text-align: right; }` |
| AC-9 | Counter animation: `.stat .n[data-count]` tweens from 0 → `data-count` over 2.2s `power3.out`, `once: true` | **PASS** | `frame.js` lines 54–69: `ScrollTrigger.create({ once: true, onEnter() { gsap.to(obj, { v: target, duration: 2.2, ease: 'power3.out', ... }) } })` |
| AC-10 | Stats: `47` Projects 2018–2026, `12884` Frames developed, `6` Dates left 2026 | **PASS** | `index.html` lines 264, 269, 274: `data-count="47"`, `data-count="12884"`, `data-count="6"` with correct labels |
| AC-11 | Italic amber `<em>` on first (47) and third (6) stat numbers | **PASS** | `index.html` line 264: `<em>47</em>`; line 274: `<em>6</em>`; stat 2 (`12884`) is plain text — correct |
| AC-12 | `.big` headline reveal: `y: 60 → 0, opacity: 0 → 1`, 1.4s `expo.out` | **PASS** | `frame.js` lines 34–43: `gsap.from('.pinned-frame .big', { y: 60, opacity: 0, duration: 1.4, ease: 'expo.out', scrollTrigger: { trigger: '.pinned-frame .big', start: 'top 80%' } })` |
| AC-13 | Number formatting: values > 999 use `toLocaleString()` (e.g., `12,884`) | **PASS** | `frame.js` line 64: `const formatted = Math.round(obj.v).toLocaleString();` — 12884 renders as `12,884` |

---

## Additional Checks

| Check | Result | Evidence |
|-------|--------|----------|
| `frame.css` linked in `<head>` of `index.html` | **PASS** | `index.html` lines 55–56: `<link rel="stylesheet" href="/css/frame.css" />` |
| `initFrame()` imported in `main.js` | **PASS** | `main.js` line 6: `import { initFrame } from './frame.js';` |
| `initFrame()` called in `main.js` | **PASS** | `main.js` line 30: `initFrame();` — called after `initInterlude()` |
| Mobile ≤ 800px: single-column grid | **PASS** | `frame.css` lines 111–116: `@media (max-width: 800px) { .pinned-frame .content { grid-template-columns: 1fr; } }` |
| Mobile ≤ 800px: stats left-aligned | **PASS** | `frame.css` lines 118–121: `@media (max-width: 800px) { .pinned-frame .stats { justify-self: start; text-align: left; } }` |
| `prefers-reduced-motion`: parallax tween skipped | **PASS** | `frame.js` lines 15–31: `const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches; if (!prefersReduced) { gsap.to(...) }` |
| `prefers-reduced-motion` CSS: `will-change: auto` on `.bg` | **PASS** | `frame.css` lines 124–129: `@media (prefers-reduced-motion: reduce) { .pinned-frame .bg { will-change: auto; } }` |
| No layout thrashing — animations use transform/opacity only | **PASS** | All GSAP tweens use `yPercent`, `scale`, `y`, `opacity` — no `width`, `height`, `top`, or `left` mutations |
| ARIA: section `aria-label` | **PASS** | `index.html` line 240: `aria-label="Studio — by the numbers"` |
| ARIA: `.bg` role + label | **PASS** | `index.html` line 243: `role="img" aria-label="Studio photograph by Knoch Media"` |
| ARIA: stat numbers have accessible labels | **PASS** | `index.html` lines 264, 269, 274: each `.n` has `aria-label` with full readable text; `.l` labels are `aria-hidden="true"` |
| Build exits cleanly | **PASS** | `✓ built in 98ms` — 0 errors, 0 warnings |

---

## Issues Found

None. All 13 acceptance criteria pass and all additional checks pass.

---

## Notes (non-blocking)

- **LOW — Reduced-motion: headline/counter tweens not guarded.** The `prefers-reduced-motion` JS guard in `frame.js` covers only the parallax tween. The `.big` headline `gsap.from()` (lines 34–43) and the counter `gsap.to()` inside `onEnter` (lines 59–68) are not behind the `prefersReduced` flag. This is not an AC requirement (the ticket spec only says "parallax tween is skipped") and the counters fall back gracefully to their static HTML values, so this is a non-blocking observation. Consider addressing in a future accessibility pass (KNOCH-021).
- **INFO — `initFrame()` called before `initReel()`.** The call order in `main.js` places `initFrame()` before the async `getFeaturedCollections()` reel init. This is correct and intentional per the PR description.
