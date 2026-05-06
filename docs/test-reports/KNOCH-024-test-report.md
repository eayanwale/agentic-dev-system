# KNOCH-024 Test Report — Wire Testimonials to Sanity CMS

| Field | Value |
|-------|-------|
| Ticket | KNOCH-024 |
| PR | #13 (dev → test) |
| Tester | Tester Agent |
| Date | 2026-05-05 |
| Result | **PASSED** |

---

## Acceptance Criteria Results

| AC | Description | Result | Evidence |
|----|-------------|--------|----------|
| AC-1 | Testimonials section fetches from Sanity on page load via `getTestimonials()` | PASS | `testimonial.js` line 35 imports `getTestimonials` from `./sanity.js`; line 104 `const testimonials = await getTestimonials()` inside `initTestimonial()` which is called (fire-and-forget) in `main.js` line 37 |
| AC-2 | All 5 real testimonials render correctly — `buildItem()` templates all fields | PASS | `buildItem()` (lines 47–73) creates `.testimonial-mark`, `.testimonial-quote` (textContent = `t.quote`), and `.testimonial-attr` with parts array covering `t.clientName`, optional `t.role`, optional `t.year`. All 5 testimonials in Sanity (Denise Bard, Rapha Records, Zach Albright, Mont Alto Woodsmen, Joseph Williams) will be mapped through this function. |
| AC-3 | Display order matches `order` field from Sanity Studio | PASS | `sanity.js` line 28: GROQ query is `*[_type == "testimonial"] \| order(order asc)` — `order asc` is present with no hardcoded limit; results returned in ascending order |
| AC-4 | Section renders a loading skeleton while fetch is in flight | PASS | `testimonial.js` lines 92–101: skeleton `.testimonial-item` div (with `aria-hidden="true"` and opacity:0.3 "Loading…" text) is built and appended to `list`, then `list` appended to `section` BEFORE the `await getTestimonials()` call on line 104. Skeleton is correctly in-flight. |
| AC-5 | Fetch failure falls back gracefully — no broken layout | PASS | `testimonial.js` lines 139–143: `catch` block calls `list.remove()` which removes the list element from the DOM entirely. Section collapses cleanly; a `console.warn` is also logged for dev debugging. |
| AC-6 | GSAP scroll animations trigger on dynamically rendered cards — `gsap.from` is called AFTER DOM nodes exist | PASS | `testimonial.js` lines 119–132: `gsap.from('.testimonial-item', {...})` is inside the `try` block, AFTER the `testimonials.forEach(t => list.appendChild(buildItem(t)))` call on line 113. DOM nodes exist before GSAP registration. |
| AC-7 | `ScrollTrigger.refresh()` called after content renders | PASS | `testimonial.js` line 137: `ScrollTrigger.refresh()` is called after `gsap.from()` registration (inside `try`, after forEach and GSAP block). |
| AC-8 | Adding/reordering in Sanity reflects on site after refresh — no hardcoded limit in GROQ | PASS | `sanity.js` line 28: `*[_type == "testimonial"] \| order(order asc)` fetches ALL testimonials (no `[0...N]` slice), ordered by the `order` field. Reordering in Studio and refreshing the page will reflect the new order. |

---

## Additional Checks

| Check | Result | Notes |
|-------|--------|-------|
| `testimonial.css` linked in `<head>` | PASS | `index.html` line 58: `<link rel="stylesheet" href="/css/testimonial.css" />` — present (carried from KNOCH-009) |
| `initTestimonial()` imported in `main.js` | PASS | `main.js` line 7: `import { initTestimonial } from './testimonial.js'` |
| `initTestimonial()` called in `main.js` (fire-and-forget, no await) | PASS | `main.js` line 37: `initTestimonial()` — no `await`, no `.then()` — correct fire-and-forget pattern |
| `.testimonial-list` CSS class exists | PASS | `testimonial.css` lines 42–47: `.testimonial-list { display: flex; flex-direction: column; align-items: center; width: 100%; }` |
| `.testimonial-item` CSS class exists | PASS | `testimonial.css` lines 55–60: `.testimonial-item { width: 100%; max-width: 680px; text-align: center; padding: 4rem 0; }` |
| `.testimonial-item + .testimonial-item` divider rule exists | PASS | `testimonial.css` lines 69–71: `.testimonial-item + .testimonial-item { border-top: 1px solid rgba(237, 230, 216, 0.08); }` |
| Hardcoded `.testimonial-mark`, `.testimonial-quote`, `.testimonial-attr` removed from `index.html` | PASS | `index.html` lines 291–294: section contains only a comment; no hardcoded `.testimonial-item` children. Section shell is `<section id="testimonial" class="testimonial" aria-label="Client testimonials">` + comment + closing tag only. |
| Section shell preserved: `<section id="testimonial" class="testimonial">` | PASS | `index.html` line 292: `<section id="testimonial" class="testimonial" aria-label="Client testimonials">` — shell present with correct id and class |
| `prefers-reduced-motion` guard prevents GSAP registration | PASS | `testimonial.js` lines 86, 119: `const prefersReduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches` checked at top of function; `if (!prefersReduced) { gsap.from(...) }` — GSAP block skipped entirely when reduced motion is requested. Content still renders. |
| Attribution format handles role-present case (`— Name · Role · Year`) | PASS | `testimonial.js` lines 65–69: `parts` array starts with `['—', t.clientName]`; if `t.role` is truthy, pushed; if `t.year` is truthy, pushed; joined with ` · ` → e.g., `— Denise Bard · Bride · 2024` |
| Attribution format handles company-only case (`— Name · Year`) | PASS | Same logic: when `t.role` is null/falsy (Rapha Records, Mont Alto Woodsmen), it is skipped → `— Rapha Records · 2023` |
| DOM built with `textContent` not `innerHTML` (XSS safety) | PASS | `testimonial.js` lines 60, 69: `quote.textContent = t.quote ?? ''` and `attr.textContent = parts.join(' · ')...` — both use textContent. The only `innerHTML` use is `mark.innerHTML = '&ldquo;'` (line 55) which is a literal HTML entity, not CMS data — safe. Skeleton uses innerHTML with literal string constants (line 96–99), no CMS data interpolated. |
| No layout thrashing: GSAP only animates `y` (transform) and `opacity` | PASS | `testimonial.js` line 120: `gsap.from('.testimonial-item', { y: 40, opacity: 0, ... })` — only `y` and `opacity` animated. GSAP translates `y` to CSS `transform: translateY()` internally. No layout-affecting properties (width, height, padding, margin, top, left) animated. |
| Mobile ≤800px: list items stack correctly | PASS | `.testimonial-list` uses `flex-direction: column` (default stacking); `.testimonial-item` has `width: 100%` and `max-width: 680px`. At ≤800px, `testimonial.css` lines 145–153 reduce section padding to `8rem 6vw` and `.testimonial-mark` font-size to `6rem`. Column flex already stacks items vertically on all viewports. |
| `getTestimonials()` exists in `src/js/sanity.js` (BLOCKER check) | PASS | `sanity.js` lines 27–29: `export function getTestimonials() { return fetchQuery(...) }` — exported named function confirmed present. |
| GROQ query uses `order(order asc)` (BLOCKER check) | PASS | `sanity.js` line 28: `*[_type == "testimonial"] \| order(order asc)` — exact ordering operator present. |
| Skeleton appears BEFORE `await getTestimonials()` (BLOCKER check) | PASS | `testimonial.js`: skeleton appended at lines 100–101 (`list.appendChild(skeleton)` then `section.appendChild(list)`) which is physically before line 104 (`await getTestimonials()`). Skeleton is visible during the entire fetch round-trip. |
| `catch` block calls `list.remove()` not just logging (BLOCKER check) | PASS | `testimonial.js` lines 141–142: `console.warn(...)` AND `list.remove()` — both present. The list (including any skeleton) is removed from the DOM on error. |
| `gsap.from` is INSIDE `try` block AFTER `forEach` (BLOCKER check) | PASS | `testimonial.js`: `forEach` is line 113, `gsap.from` starts line 120, both inside the `try` block (lines 103–143). Ordering confirmed correct. |

---

## Build Verification

| Step | Result | Detail |
|------|--------|--------|
| `npm run build` | PASS | Vite v8.0.10 — 27 modules transformed, no errors. `dist/assets/main-CBzSeLXV.js` 143.23 kB (53.05 kB gz), `dist/assets/main-CgAosyF9.css` 14.43 kB (3.59 kB gz). Built in 87ms. |

---

## Issues Found

None. All acceptance criteria and additional checks pass.

**Non-blocking observations (no action required):**
- The skeleton uses `innerHTML` with literal string constants (not CMS data), so there is no XSS risk. The comment in `buildItem()` correctly documents the `textContent`-vs-`innerHTML` distinction.
- `mark.innerHTML = '&ldquo;'` is a safe HTML entity literal; not a security concern.
- The `list.innerHTML = ''` clear (line 107) is correct: it replaces the skeleton efficiently before appending real items.
