# KNOCH-019 — Performance Optimization

## Status: TODO
## Priority: P1 (high)
## Epic: EPIC-005 — Polish & Ship

## Title
Performance: Lazy Loading, Animation Budget, and Core Web Vitals

## Description
A premium portfolio site that scores poorly on Lighthouse is a credibility problem. This ticket covers all performance-specific work that isn't already handled by individual component tickets: image lazy loading strategy, animation frame budget enforcement, GSAP scroll-trigger cleanup, font optimization, and Vite build output tuning.

## Acceptance Criteria
**Images:**
- [ ] All images below the fold use `loading="lazy"` on `<img>` elements
- [ ] Hero image is `loading="eager"` with a `<link rel="preload">` in `<head>` — it is the LCP element
- [ ] All portfolio tile and reel card images use `srcset` with at least two breakpoints: `800w` and `1600w`
- [ ] Images converted to WebP format; JPEG fallback via `<picture>` element
- [ ] `src/assets/` images run through Vite's `vite-plugin-imagemin` during build (or manual pre-compression to ≤200KB per image)
- [ ] IntersectionObserver-based lazy loader for background-image tiles (CSS `background-image` doesn't support `loading="lazy"`) — swap `data-bg` to `background-image` on entry

**Animations:**
- [ ] All GSAP ScrollTrigger instances store their return value and are killed on page unload/nav: `trigger.kill()` in a cleanup function
- [ ] `will-change: transform` is applied ONLY to elements actively animating — removed after animation completes via `onComplete: () => el.style.willChange = 'auto'`
- [ ] No scroll listener (`addEventListener('scroll', fn)`) used anywhere — all scroll work goes through ScrollTrigger or Lenis's event system
- [ ] Horizontal reel uses `will-change: transform` on `.reel-track` only (not on each card)
- [ ] GSAP `gsap.ticker.fps(60)` set to cap animation updates at 60fps

**Fonts:**
- [ ] Google Fonts loaded with `font-display: swap` (already in KNOCH-001, verify here)
- [ ] Only the required font weights/styles are loaded: Fraunces `300`, `300 italic`, `400 italic`; JetBrains Mono `400`, `500`; Inter Tight `400`, `500`
- [ ] No unused CSS from Google Fonts (use the `text=` param for icon-only subsets if any)

**Build / delivery:**
- [ ] Vite build output: JS chunks ≤ 150KB gzipped total
- [ ] GSAP imported with tree-shaking: `import { gsap } from 'gsap'` + `import { ScrollTrigger } from 'gsap/ScrollTrigger'` (not the bundle)
- [ ] CSS purged of unused rules via `vite-plugin-purgecss` or manual audit
- [ ] `<link rel="preconnect">` for Google Fonts and any third-party scripts (Behold.so, Calendly)
- [ ] Lighthouse mobile score target: ≥85 Performance, ≥90 Accessibility, 100 Best Practices, ≥90 SEO

**Meta / SEO:**
- [ ] `<meta name="description">` on each page
- [ ] `<meta property="og:image">` pointing to a 1200×630 hero image for social sharing
- [ ] `<title>` unique per page: `Knoch · Wedding & Commercial Photography · Maryland`
- [ ] Structured data: `LocalBusiness` JSON-LD schema on homepage

## Design Notes
The LCP element (Largest Contentful Paint) is the hero background image. Preloading it with `<link rel="preload" as="image">` is the single highest-impact performance win. Without it, the image loads after the JS has parsed and the loader counter has run, causing a flash.

`will-change` is a double-edged sword — it promotes elements to their own compositing layers (good for animation smoothness) but increases GPU memory usage. Use it only during active animation; strip it after.

Animation frame budget: at 60fps, each frame has 16.67ms. GSAP + Lenis together should consume ≤8ms per frame. Monitor with Chrome DevTools Performance tab during the horizontal reel section (most expensive animation).

## Tradeoffs Considered
- Automatic image optimization (plugin) vs. manual pre-compression: A Vite plugin automates this but adds build time. For a portfolio with ≤50 images that rarely change, manual WebP conversion in `src/assets/` is faster to set up and easier to debug.
- GSAP tree-shaking: GSAP's npm package supports tree-shaking if you import plugins individually rather than the bundle. This eliminates DrawSVG, Flip, MotionPath etc. from the bundle — saves ~40KB gzipped.

## Related Tickets
- KNOCH-001 (Vite config for build optimization)
- KNOCH-005 (hero LCP preload)
- KNOCH-007 (reel is the most animation-heavy section)
- KNOCH-016 (Lenis ticker efficiency)
- All image-bearing sections (KNOCH-005, 007, 008, 010, 013, 018)
