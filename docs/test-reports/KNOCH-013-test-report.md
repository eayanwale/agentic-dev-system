# KNOCH-013 — Test Report

| | |
|---|---|
| **Ticket** | KNOCH-013 — About / Story Section |
| **PR** | [#19](https://github.com/eayanwale/knochmedia/pull/19) (dev → test) |
| **Branch** | `feature/KNOCH-013-about-story-section` (merged to dev) |
| **Tester** | Tester Agent |
| **Date** | 2026-05-06 |
| **Result** | **PASSED** |

---

## Summary

PR #19 implements the About / Story page (`/about.html`) plus the homepage studio teaser block. All 12 acceptance criteria pass. Build is clean (60 modules, 124 ms; emits `dist/about.html` 13.28 kB / 4.08 kB gz, `dist/index.html` 26.51 kB / 7.77 kB gz). Two minor findings — one trivial spec deviation fixed during QA, one design-rationale note documented in the CSS — neither blocking.

---

## Acceptance Criteria

### Homepage studio teaser

| AC | Result | Note |
|----|--------|------|
| Not a full about section — 2-sentence brand statement + "Read the story →" link | ✅ | `<section class="studio-teaser">` in `src/index.html` lines 379–388. Body has 2 sentences, link reads "Read the story →" with amber underline |
| Simple text block, no separate layout component | ✅ | One `<section>` with three children: `.studio-teaser-meta`, `.studio-teaser-body` (with amber `<em>` highlight), `.studio-teaser-link` |

### `about.html` full page

| AC | Result | Note |
|----|--------|------|
| Page entry: lighter reveal, no full film-counter loader | ✅ | No `#loader` element. `about.js` runs a short hero entrance timeline (meta + headline + sub fade up, total ~1.5 s) before idle — lighter than the 2 s+ homepage loader, matching the spirit of the AC |
| Hero: large Fraunces headline `"A working studio."` with subhead | ✅ | `.about-hero-headline` clamp(48px, 9vw, 140px) Fraunces 300 + amber italic `<em>studio.</em>`. Sub: "Cinematic photography & film · Maryland". Year ("EST. 2018") in meta label |
| Split layout: `display: grid; grid-template-columns: 1fr 1fr` on desktop | ✅ | `src/css/about.css` line 118: `.about-split-inner { display: grid; grid-template-columns: 1fr 1fr; gap: 6vw; }` |
| Left col (sticky): `position: sticky; top: 10vh; height: 80vh` | ✅ *(after fix)* | Originally implemented as `top: 12vh` with no `height` set; aligned to spec exactly during QA pass — see Finding #1 below |
| Right col (scrolls): 4–5 full-width images stacked vertically with `aspect-ratio: 3/4`, each with caption | ✅ | 5 `<figure class="about-image">` elements, each with a `.about-image-frame` (aspect-ratio 3/4 in CSS) and `<figcaption>` mono caption |
| Right column triggers chapter highlight | ✅ | `about.js` IntersectionObserver(threshold: 0.5) toggles `.active` on chapter matching the visible image. Chapter 1 set active on init so page never opens with all chapters dimmed |
| "By the numbers": 47 / 8 / 12,884 in Fraunces, reuses KNOCH-008 counter | ✅ | Same proxy-tween pattern: `gsap.to({ val: 0 }, { val: target, ... onUpdate })` inside a `ScrollTrigger.create({ start: 'top 85%', once: true })`. `<em>` wrap preserved across rewrites for amber italic on stats 1 & 3 |
| Process: 3 numbered steps `01 The inquiry`, `02 The shoot`, `03 The delivery` | ✅ | Three `.about-process-step` blocks with mono "01 / 02 / 03" labels, Fraunces titles, sans bodies |
| CTA: `"Ready to make a roll?"` → contact | ✅ | `.about-cta-link` href `/#cta`. Contact page proper is KNOCH-014 — `/#cta` is the closest match available; PR description acknowledges this dependency |
| Nav: same chrome overlay from KNOCH-003 | ✅ | Same `.chrome` markup, same `chrome.css` link, same `chrome.js` loaded via `about-main.js`. `.mark` click handler updated in chrome.js to allow cross-page navigation (see passing checks below) |

**12/12 ACs pass.**

---

## Validation

| Check | Result | Note |
|-------|--------|------|
| `npm run build` | ✅ PASS | Clean — 60 modules transformed, 124 ms. Emits `dist/about.html`, `dist/index.html`, hashed CSS / JS bundles, all `dist/assets/about/about-01..05.jpg` at literal paths |
| Vite multi-page setup | ✅ PASS | `vite.config.js` has both `main` and `about` in `rollupOptions.input`. Both HTML files end up at the correct `dist/` paths |
| `src/public/assets/about/` images | ✅ PASS | 5 jpgs (about-01..05) shipped to `dist/assets/about/` literal-named, matching `<style background-image: url('/assets/about/about-0N.jpg')>` runtime references in HTML |
| Homepage assets unaffected | ✅ PASS | `dist/assets/portfolio/`, `dist/assets/reel/`, `dist/assets/testimonials/` all still in place at literal paths — KNOCH-031 publicDir fix not regressed |
| chrome.js cross-page nav | ✅ PASS | `.mark` handler now compares `mark.href` pathname to `window.location.pathname`; same-page → `e.preventDefault()` + `scrollTo(0)`, cross-page → browser navigates. Homepage `.mark` (href `/`) still smooth-scrolls to top; about-page `.mark` (href `/`) navigates to `/` |
| Slim entry point | ✅ PASS | `about-main.js` loads only lazy-load, chrome, cursor, char-hover, lenis, about — no homepage-specific modules (hero, interlude, frame, reel, portfolio-grid, testimonial, inquiry). About bundle stays small |
| LCP preload | ✅ PASS | `<link rel="preload" as="image" href="/assets/about/about-01.jpg" fetchpriority="high">` in `<head>` |
| Sanity meta tags consistent | ✅ PASS | Same `sanity-project-id` and `sanity-dataset` meta tags on `about.html` as `index.html` for future content wiring (KNOCH-027) |
| IntersectionObserver chapter sync | ✅ PASS | Threshold 0.5 — chapter activates when image is at least half in view. `chapterMap` lookup is O(1). Initial state sets chapter 1 active so the page doesn't open with all chapters dimmed. Image 5 tagged `data-image="4"` (intentional duplicate) so chapter 4 stays lit through the closing image |
| ScrollTrigger counter (`once: true`) | ✅ PASS | Each stat tween fires once on first scroll-in. Proxy `{val: 0}` tweens to `data-count` target with `Math.ceil(proxy.val).toLocaleString()` formatting. `<em>` wrap preserved across `innerHTML` rewrites |
| `prefers-reduced-motion: reduce` | ✅ PASS | Hero entrance timeline gated on `!prefersReduced`; counter tweens snap to final value via `wrap(target)`; `chapter` and `chapter-label` CSS transitions cleared via `@media` block; chapters set to `opacity: 1` (no dim) so the page is fully readable without animation |
| Mobile (≤800px) — split layout collapse | ✅ PASS | `@media (max-width: 800px)` sets `.about-split-inner` to single column, `.about-text-sticky` to `position: static` (drops sticky), `.chapter` to `opacity: 1` (drops dim), `.about-stats-grid` and `.about-process-grid` to single column |
| Tablet (768px) | ✅ PASS | Within mobile breakpoint — collapses to single column |
| Desktop (1280px+) | ✅ PASS | Two-column grid with 6vw gap; sticky engages |
| Animation performance | ✅ PASS | All animations target `transform`, `opacity`, `clip-path`, `filter` — composited properties. No animations on `top`/`left`/`width`/`height` (which trigger layout). `will-change: transform, filter` only used on `.hero-bg` (homepage); about page chapter transitions use only `opacity` and `color` which the browser composites efficiently |

---

## Accessibility

| Check | Result | Note |
|-------|--------|------|
| Document language | ✅ | `<html lang="en">` |
| Page title | ✅ | "The Studio — Knoch Media" |
| Meta description | ✅ | Present and descriptive |
| Semantic structure | ✅ | `<h1>` (hero), `<h2>` (chapters, CTA), `<h3>` (process steps), `<article>`, `<figure>`/`<figcaption>`, `<nav>`, `<section>` with `aria-label` per region |
| Decorative chrome | ✅ | Custom cursor and timecode bar both `aria-hidden="true"`; nested `role="progressbar"` retained on `.scroll-progress` (intentional — meaningful indicator despite parent's aria-hidden) |
| Background image alts | ✅ | Each `.about-image-frame` div has `role="img"` + descriptive `aria-label` so AT users get the same caption AT users would otherwise miss from a `background-image` |
| Stat counter labels | ✅ | `<div class="about-stat-n" data-count="47" aria-label="47 projects">` — final value announced regardless of tween state |
| Active page indicator | ✅ | Studio nav link has `aria-current="page"` |
| Cross-page link target | ✅ | `.mark` `aria-label="Knoch — back to home"` (vs homepage `Knoch — scroll to top`) — accurately describes cross-page action |

---

## Performance / build size

`dist/about.html` ships at **13.28 kB** (4.08 kB gz). The shared bundle (GSAP + ScrollTrigger via `char-hover` chunk, plus `lenis`, `chrome`, `cursor`, `about`) totals ~170 kB / 61 kB gz — same dependencies as homepage, so cross-page nav from index.html → about.html should hit a warm browser cache for everything except the page-specific `about-*.css` (4.91 kB / 1.22 kB gz) and `about-*.js` (1.62 kB / 0.80 kB gz).

---

## Findings

### LOW #1 — Sticky CSS values vs spec  *(fixed during QA)*

**File:** `src/css/about.css:135-137`

The implementation initially shipped `.about-text-sticky { position: sticky; top: 12vh; }` with no `height` set. The acceptance criterion specifies `position: sticky; top: 10vh; height: 80vh`.

**Fix applied:** Aligned to spec exactly — `top: 10vh; height: 80vh`. Comment in the CSS now documents that overflow is intentionally left visible because four chapters at the current sizing total ~880 px on a 900 px viewport, exceeding the 80vh box (~720 px); clipping the 4th chapter would defeat the narrative. The 80vh height still bounds the sticky box for the active-chapter highlight and gives 20vh of breathing room between sticky and viewport bottom.

**Impact:** Cosmetic only — visual outcome on desktop is essentially identical to the original 12vh value; on shorter viewports the chapter content extends slightly past the sticky's "box" but is still fully visible.

### LOW #2 — Image 5 maps to chapter 4 via duplicate `data-image`  *(documented, not a defect)*

**File:** `src/about.html:201` and `src/js/about.js:80-99`

The page has 5 images but only 4 narrative chapters. To keep chapter 4 highlighted while the user reads the closing paragraph and lands on the stats section, image 5 is tagged `data-image="4"` (same value as image 4). Both figures activate chapter 4 via `chapterMap.get('4')`. This is intentional and documented in HTML and JS comments, but worth noting because the duplicate attribute value would otherwise look like a typo to a casual reader.

**Impact:** None — design intent is implemented correctly.

---

## Passing checks (summary)

- All 12 acceptance criteria
- Vite build clean (60 modules, 124 ms; about.html 13.28 kB / 4.08 kB gz)
- Sticky CSS now matches spec exactly (`top: 10vh; height: 80vh`)
- Multi-page Vite setup, slim about-main.js entry, chrome.js cross-page nav fix all working
- 5 about images shipped to `src/public/assets/about/` per the publicDir rule
- Hero entrance timeline + counter ScrollTriggers + IntersectionObserver chapter sync all functioning
- Mobile (≤800px) collapse to single column, drop sticky, drop dim
- `prefers-reduced-motion: reduce` skips all animations gracefully
- Accessibility — semantic markup, aria-label on every region, aria-current on active nav, role=img + aria-label on background-image divs, aria-label values include final stat counts

---

## Recommendation

**APPROVE and merge** PR #19 (dev → test). Both findings above are LOW; #1 is fixed during QA and committed as part of this test pass, #2 is documented design intent.
