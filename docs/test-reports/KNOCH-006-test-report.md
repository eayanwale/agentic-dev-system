# KNOCH-006 Test Report — Interlude Manifesto Section

| Field | Value |
|-------|-------|
| Ticket | KNOCH-006 |
| PR | #10 (dev → test) |
| Tested | 2026-05-05 |
| Result | **PASSED** *(re-test 2026-05-05)* |
| Tester | Tester Agent |

---

## Acceptance Criteria Results

| # | Criterion | Result | Notes |
|---|-----------|--------|-------|
| 1 | Section: `height: 100vh; display: flex; align-items: center; justify-content: center; padding: 0 8vw` | PASS | All five properties confirmed in `interlude.css` lines 11–15 |
| 2 | Amber label top-left: `position: absolute; top: 18vh; left: 8vw`, text `— A note before we begin`, 11px mono, `letter-spacing: 0.3em`, uppercase, amber | PASS | All properties match in `.interlude-label` rule; text matches in HTML |
| 3 | Quote: Fraunces 300, `clamp(28px, 5vw, 64px)`, `line-height: 1.15`, `max-width: 22ch`; italic amber `<em>` | PASS | All typography properties confirmed in `.interlude-quote`; `<em>` gets `color: var(--amber); font-style: italic` via CSS |
| 4 | Signature `— Enoch Knoch · Founder`, 10px mono, muted (--paper at ~0.4 opacity) | PASS | `.interlude-sig`: 10px, `var(--font-mono)`, `color: var(--paper)`, `opacity: 0.4` |
| 5 | JS splits quote text into `.word` spans at init, preserving `<em>` | **FAIL — BLOCKER** | See Issue #1 below |
| 6 | All `.word` spans start at `opacity: 0.15` | FAIL (cascades from #5) | No `.word` spans are created due to the word-split bug; CSS rule exists but is unreachable |
| 7 | GSAP ScrollTrigger: `trigger: '.interlude'`, `start: 'top 70%'`, `end: 'center 40%'`, `scrub: 0.5`, `stagger: 0.04`, animates to `opacity: 1` | FAIL (cascades from #5) | GSAP call is correct on lines 114–128 of `interlude.js` but operates on an empty NodeList — no words animate |
| 8 | `<em>` color via CSS `.interlude-quote em { color: var(--amber) }`, not inline style | PASS | Rule at `interlude.css` line 62–64 is correct |
| 9 | Accessible: original text in DOM; screen readers read full sentence | PASS | Despite the bug, the `<p>` is cloned back intact into the blockquote (`else` branch, line 65 of `interlude.js`); AT reads the full sentence |
| 10 | `interlude.css` linked in `<head>` of `index.html` | PASS | Line 52 of `src/index.html` |
| 11 | `initInterlude()` imported and called in `main.js` | PASS | Line 4 (import) and line 24 (call) of `main.js` |
| 12 | Mobile ≤800px: `min-height: 100vh`; single IntersectionObserver fade-in fallback | PASS | CSS: `height: auto; min-height: 100vh` at line 95–99; JS IO fallback at lines 89–107 of `interlude.js` |
| 13 | `prefers-reduced-motion`: all words shown at `opacity: 1` immediately | PASS (CSS layer) / NOTE (JS layer) | CSS `@media (prefers-reduced-motion: reduce)` sets `.word { opacity: 1 !important }` correctly. JS also sets `w.style.opacity = '1'` inline for each word — redundant with CSS but not harmful. Because of bug #5, JS iterates an empty list, so only CSS handles it. |
| 14 | `.interlude` has `.grain` class | PASS | `class="interlude grain"` in `src/index.html` line 187 |
| 15 | No layout thrashing: word-split uses spans (inline), not block-level elements | PASS (design) | `.word { display: inline }` in CSS; logic is correct in intent |

---

## Code Quality Checks

| Check | Result | Notes |
|-------|--------|-------|
| No inline styles for color/opacity beyond GSAP | PASS | No hardcoded color inline styles; only `w.style.opacity = '1'` in reduced-motion JS path (minor, acceptable as guard) |
| No hardcoded hex values — uses CSS custom properties | PASS | All color references use `var(--amber)`, `var(--paper)`, `var(--ink)` |
| Word-split wraps `<em>` in `span.word > em` (not the reverse) | FAIL (cascades from Issue #1) | The `node.nodeName === 'EM'` branch (line 56) is correct in approach but is never reached because the childNodes iterator operates on the `<blockquote>`, not the `<p>` |
| Whitespace between words preserved so sentence wraps naturally | FAIL (cascades from Issue #1) | Whitespace preservation logic exists and is correct in design, but no splitting occurs so no words or whitespace spans are produced |
| Token variable names correct (`var(--amber)` not `var(--accent)`, etc.) | PASS | All token names match `tokens.css`: `--amber`, `--paper`, `--ink`, `--font-mono`, `--font-serif`, `--ease-cinematic` |

---

## Build

```
> knoch-portfolio@0.1.0 build
> vite build

vite v8.0.10 building client environment for production...
✓ 20 modules transformed.

dist/index.html                      10.29 kB │ gzip:  4.02 kB
dist/assets/reel-01-DiP1D0Pp.png  2,052.71 kB
dist/assets/main-gj9BnGGQ.css         7.95 kB │ gzip:  2.41 kB
dist/assets/main-BWMr6IFu.js        136.66 kB │ gzip: 50.92 kB

✓ built in 88ms
```

Build: **PASSED** — clean exit, no errors, 20 modules.

---

## Issues Found

### Issue #1 — BLOCKER: Word-split targets `<blockquote>` childNodes instead of `<p>` childNodes

**Severity:** BLOCKER  
**File:** `src/js/interlude.js`, line 37  
**Impact:** Zero `.word` spans are created. GSAP ScrollTrigger animates an empty NodeList. The signature "reading along" word-by-word reveal effect does not work at all.

**Root cause:**

The HTML structure is:
```html
<blockquote class="interlude-quote">
  <p>I don't photograph weddings. I attend them — <em>quietly</em> — and bring back what was beautiful.</p>
</blockquote>
```

The JS selects `.interlude-quote` (the `<blockquote>`) and iterates its `childNodes`:
```js
const quote = section.querySelector('.interlude-quote')  // <blockquote>
const nodes = Array.from(quote.childNodes)               // [ <p>...</p> ] plus whitespace text nodes
```

The `<blockquote>`'s only meaningful child is the `<p>` element. Its `nodeType` is `Node.ELEMENT_NODE` (1), not `Node.TEXT_NODE` (3), and its `nodeName` is `'P'`, not `'EM'`. It therefore falls into the `else` branch:
```js
} else {
  // Any other node type (e.g. nested inline elements) — preserve as-is
  quote.appendChild(node.cloneNode(true))
}
```

The `<p>` is cloned back into the `<blockquote>` unchanged. No `.word` spans are created. `quote.querySelectorAll('.word')` returns an empty NodeList. GSAP runs `gsap.fromTo([], ...)` — a no-op.

**Fix:** The word-splitter must target the `<p>` inside the blockquote, not the blockquote itself. Replace:
```js
const quote = section.querySelector('.interlude-quote')
if (!quote) return

const nodes = Array.from(quote.childNodes)
quote.innerHTML = ''
```
with:
```js
const quote = section.querySelector('.interlude-quote')
if (!quote) return

const para = quote.querySelector('p') ?? quote   // target inner <p>; fall back to blockquote if no <p>
const nodes = Array.from(para.childNodes)
para.innerHTML = ''
```

Then replace all `quote.appendChild(...)` inside the `nodes.forEach` loop with `para.appendChild(...)`, and change `quote.querySelectorAll('.word')` to `para.querySelectorAll('.word')` (or keep `quote.querySelectorAll('.word')` — it will find descendants regardless).

Alternatively the HTML can be restructured to place text directly in the `<blockquote>` without a `<p>` wrapper, but the `<p>` wrapper is semantically more correct for a blockquote containing a paragraph.

---

## Notes / Observations

1. **Accessibility holds despite the bug.** The `<p>` is cloned back intact, so AT reads the full quote text. When the bug is fixed, `<span class="word">` (inline, no ARIA role) will still not interrupt screen-reader traversal.

2. **GSAP call parameters are correct.** Once the word-split is fixed, the GSAP animation will work as specified: `trigger: '.interlude'`, `start: 'top 70%'`, `end: 'center 40%'`, `scrub: 0.5`, `stagger: 0.04`.

3. **Mobile IO and reduced-motion CSS paths are correctly designed.** They will work once `.word` spans exist in the DOM.

4. **Minor: JS reduced-motion path sets `w.style.opacity = '1'` as inline style.** The CSS `@media (prefers-reduced-motion: reduce)` block already handles this via `opacity: 1 !important`. The inline JS is redundant but not a violation per the AC. Consider removing for cleanliness in a follow-up.

5. **For KNOCH-020 (mobile ticket):** The IO threshold of `0.3` is reasonable. The `quote.classList.add('is-visible')` + `section.classList.add('is-visible')` pattern for the mobile fade-in is clean.

6. **`.grain` reuse is correct.** `cursor.css` defines `.grain::after` with the SVG feTurbulence overlay; applying `.grain` to `.interlude` correctly inherits the film-grain without duplicating the CSS rule.

---

## Re-test — 2026-05-05

**Tester:** Tester Agent  
**Trigger:** Builder applied fix commit `fix(interlude): retarget word-splitter from blockquote to inner <p> (KNOCH-006)`

### Fix Verified

The one blocker from the original test run has been correctly resolved. The following changes were confirmed present in `src/js/interlude.js`:

- Line 40: `const para = quote.querySelector('p') ?? quote` — present; resolves to the `<p>` when it exists, falls back to `quote` if not.
- Line 41: `Array.from(para.childNodes)` — iterates the `<p>`'s childNodes, not the blockquote's.
- Line 42: `para.innerHTML = ''` — clears the `<p>` before repopulating.
- Lines 54, 57, 65, 69: all `appendChild()` calls target `para`, not `quote`.
- Line 73: `para.querySelectorAll('.word')` — queries from the `<p>` downward.

### Logic Trace — Actual HTML Content

```html
<blockquote class="interlude-quote">
  <p>I don't photograph weddings. I attend them &mdash; <em>quietly</em> &mdash; and bring back what was beautiful.</p>
</blockquote>
```

`para` = the `<p>` element. Its `childNodes` are:
1. Text node: `"I don't photograph weddings. I attend them — "` → split by whitespace → multiple `<span class="word">` elements plus preserved whitespace text nodes.
2. Element node `<em>quietly</em>` → `nodeName === 'EM'` branch → wrapped in `<span class="word"><em>quietly</em></span>`.
3. Text node: `" — and bring back what was beautiful."` → split into further `<span class="word">` elements and whitespace text nodes.

Result: all words become `.word` spans; `<em>` is preserved intact inside its wrapper. `para.querySelectorAll('.word')` returns a non-empty NodeList. GSAP `fromTo(words, ...)` animates all of them.

### All Acceptance Criteria Re-checked

| # | Criterion | Result |
|---|-----------|--------|
| 1 | Section layout: `height: 100vh; display: flex; align-items: center; justify-content: center; padding: 0 8vw` | PASS |
| 2 | Amber label top-left: `position: absolute; top: 18vh; left: 8vw`, text `— A note before we begin`, 11px mono, `letter-spacing: 0.3em`, uppercase, amber | PASS |
| 3 | Quote: Fraunces 300, `clamp(28px, 5vw, 64px)`, `line-height: 1.15`, `max-width: 22ch`; italic amber `<em>` | PASS |
| 4 | Signature `— Enoch Knoch · Founder`, 10px mono, muted | PASS |
| 5 | JS splits quote text into `.word` spans, preserving `<em>` | **PASS** (fix confirmed — blocker resolved) |
| 6 | All `.word` spans start at `opacity: 0.15` | PASS (CSS rule + words now created) |
| 7 | GSAP ScrollTrigger: `trigger: '.interlude'`, `start: 'top 70%'`, `end: 'center 40%'`, `scrub: 0.5`, `stagger: 0.04` | PASS (operates on non-empty NodeList) |
| 8 | `<em>` color via CSS `.interlude-quote em { color: var(--amber) }`, not inline style | PASS |
| 9 | Accessible: original text preserved in DOM for screen readers | PASS |
| 10 | `interlude.css` linked in `<head>` | PASS |
| 11 | `initInterlude()` imported and called in `main.js` | PASS |
| 12 | Mobile ≤800px: `min-height: 100vh`; IntersectionObserver fallback | PASS |
| 13 | `prefers-reduced-motion`: all words at `opacity: 1` immediately | PASS |
| 14 | `.interlude` has `.grain` class | PASS |
| 15 | No layout thrashing: word-split uses inline `<span>` elements | PASS |

### Build (Re-test)

```
> knoch-portfolio@0.1.0 build
> vite build

vite v8.0.10 building client environment for production...
✓ 20 modules transformed.

dist/index.html                      10.29 kB │ gzip:  4.02 kB
dist/assets/reel-01-DiP1D0Pp.png  2,052.71 kB
dist/assets/main-gj9BnGGQ.css         7.95 kB │ gzip:  2.41 kB
dist/assets/main-C-9_YHLB.js        136.69 kB │ gzip: 50.93 kB

✓ built in 89ms
```

Build: **PASSED** — clean exit, no errors, 20 modules.

### Outcome

**All 15 acceptance criteria PASS.** PR #10 approved and merged to `test`. KNOCH-006 status set to QA PASSED.
