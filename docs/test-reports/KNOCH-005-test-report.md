# KNOCH-005 Test Report — Hero Section

| Field | Value |
|-------|-------|
| Ticket | KNOCH-005 |
| PR | #6 (dev → test) |
| Tester | Tester Agent |
| Date | 2026-05-05 |
| Result | **PASSED** |

---

## Passing checks

### Loader

- **AC-1** `#loader` — `position: fixed; inset: 0; z-index: var(--z-loader); background: var(--ink)` — `hero.css:23-34` ✓
- **AC-2** `.loader-counter` — `font-family: var(--font-serif); font-size: 18vw; font-weight: 300` — `hero.css:48-56` ✓
- **AC-3** GSAP proxy tween — `{ val: 0 }` proxy tweened to 36; `Math.ceil(proxy.val).padStart(2,'0')` in `onUpdate` — `hero.js:63-78` ✓
- **AC-4** Amber label — text "Loading roll · 36 exposures"; `font-size: 11px; letter-spacing: 0.3em; color: var(--amber); text-transform: uppercase`; `.loader-label` appears before `.loader-counter` in DOM — `hero.css:37-45`, `index.html:121-122` ✓
- **AC-5** Progress bar — `.loader-progress` 240px wide, 1px height; `.loader-progress-fill` background `var(--amber)`; GSAP animates width 0→100% — `hero.css:59-79`, `hero.js:82-87` ✓
- **AC-6** Loader fade — `gsap.to(loader, { autoAlpha: 0, duration: 1, delay: 2 })` with `loader.style.display = 'none'` in `onComplete` — `hero.js:92-119` ✓
- **AC-7** Triggered on `window` `load` event (not `DOMContentLoaded`) — `hero.js:58` ✓
- **AC-8** `body.loader-active .cursor { opacity: 0 }` in `cursor.css:61-64`; `body.classList.add('loader-active')` in `hero.js:36` ✓

### Hero layout

- **AC-9** `#hero` — `height: 100vh; width: 100vw; overflow: hidden; position: relative; display: flex; align-items: center; justify-content: center` — `hero.css:84-92` ✓
- **AC-10** `.hero-bg` — `position: absolute; inset: 0; background-size: cover; background-position: center; filter: brightness(0.45) grayscale(0.4) contrast(1.15); transform: scale(1.1)` — `hero.css:97-107` ✓
- **AC-11** `.hero-bg` background-image uses `/assets/reel/reel-01.png` (not hero-01.jpg) — `hero.css:100` ✓
- **AC-12** `<link rel="preload" as="image" href="/assets/reel/reel-01.png" fetchpriority="high">` present in `<head>` — `index.html:54` ✓
- **AC-13** `.hero-content` — `position: relative; z-index: 2; text-align: center; padding: 0 5vw` — `hero.css:127-133` ✓
- **AC-14** `.hero-meta` — `font-family: var(--font-mono); font-size: 10px; letter-spacing: 0.3em; color: var(--amber); opacity: 0` — `hero.css:137-145` ✓
- **AC-15** `.hero-headline .line` — `overflow: hidden; display: block` — `hero.css:161-164` ✓
- **AC-16** `.hero-headline .line span` — `transform: translateY(110%)` as initial state — `hero.css:169-173` ✓
- **AC-17** Headline is four lines: "Nothing" / `<em>staged.</em>` / "Everything" / `<em>remembered.</em>` — `index.html:156-159` ✓
- **AC-18** `#hero-sub` — `opacity: 0` initially; copy renders as `CINEMATIC PHOTOGRAPHY & FILM · SCROLL ↓` — `hero.css:183-191`, `index.html:163-165` ✓

### Reveal sequence

- **AC-19** `tl.to(heroBg, { scale: 1, duration: 2.4, ease: 'power3.out' }, 0)` — `hero.js:142-147` ✓
- **AC-20** `tl.to(heroMeta, { opacity: 1, duration: 0.8 }, 0)` — `hero.js:151-155` ✓
- **AC-21** `tl.to(lineSpans, { y: 0, duration: 1.2, ease: 'expo.out', stagger: 0.12 }, 0.2)` — `hero.js:162-168` ✓
- **AC-22** `tl.to(heroSub, { opacity: 1, duration: 0.8 }, 1.4)` — `hero.js:171-176` ✓

### Chrome coordination

- **AC-23** `chrome.style.opacity = '0'` at top of `initHero()`; `gsap.to('#chrome', { opacity: 1, duration: 0.8, delay: 0.2 })` in loader `onComplete` — `hero.js:33`, `hero.js:109-113` ✓

### Lenis timing

- **AC-24** `initLenis()` not called in `main.js` directly; called inside loader `onComplete` in `hero.js` — `main.js` (comment confirms intent), `hero.js:103` ✓
- **AC-25** `main.js` imports `initHero` and calls `initHero()` with no direct `initLenis` call — `main.js:3,20` ✓

### Hero exit

- **AC-26** ScrollTrigger on `.hero-bg` — `yPercent: 25`, `trigger: '#hero'`, `start: 'top top'`, `end: 'bottom top'`, `scrub: true` — `hero.js:202-215` ✓
- **AC-27** ScrollTrigger on `.hero-content` — `yPercent: -40; opacity: 0`, same trigger config — `hero.js:218-225` ✓

### prefers-reduced-motion

- **AC-28** `@media (prefers-reduced-motion: reduce)` sets `.hero-bg { transform: scale(1) }`, `.hero-headline .line span { transform: translateY(0) }`, `.hero-meta, #hero-sub { opacity: 1 }` — `hero.css:211-224` ✓

### Asset

- **AC-29** `src/assets/reel/reel-01.png` exists on disk ✓

### Additional checks

- **Build** — `npm run build` exits cleanly: 18 modules, `dist/assets/reel-01-DiP1D0Pp.png 2,052.71 kB`, CSS 6.74 kB, JS 135.48 kB, 83ms ✓
- **No DOMContentLoaded** — only appears in a comment block, not in executable code ✓
- **`#loader aria-hidden="true"`** — present on `index.html:120` ✓
- **`.hero-bg` is child div of `#hero`** — confirmed; `#hero` has no background-image of its own; `.hero-bg` is a `<div role="img">` child — `index.html:141` ✓

---

## Failing checks

None.

---

## Notes

**LOW (non-blocking):** Two hardcoded `rgba()` values in `hero.css` that ideally should use design tokens:
- `hero.css:62` — `.loader-progress` background: `rgba(237, 230, 216, 0.15)` — this is `--paper` at 15% opacity. The token system has `--paper` but not `--paper-15`. This is a mild inconsistency; the value is correct and cosmetically appropriate.
- `hero.css:189` — `#hero-sub` color: `rgba(237, 230, 216, 0.6)` — `--paper` at 60% opacity. Same situation.

Neither blocks functionality. Consider adding `--paper-dim` or `--paper-muted` alpha tokens in a future pass (KNOCH-002 follow-up or KNOCH-021 a11y sweep).

**LOW:** PR body still references `src/assets/hero/hero-01.jpg` in the "Files changed" table (build output note). The actual implementation correctly uses `src/assets/reel/reel-01.png`. This is a documentation inconsistency in the PR description only — no code impact.
