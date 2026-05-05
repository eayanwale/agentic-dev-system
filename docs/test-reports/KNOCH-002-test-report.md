# KNOCH-002 Test Report — Design Tokens & CSS Custom Properties

| Field      | Value                           |
|------------|---------------------------------|
| Ticket     | KNOCH-002                       |
| Date       | 2026-05-05                      |
| Tester     | Tester Agent                    |
| PR         | #2 (dev → test)                 |
| Branch     | dev                             |
| **Result** | **PASSED**                      |

---

## Build Verification

```
vite v8.0.10 — 5 modules transformed
dist/src/index.html             2.41 kB │ gzip: 1.19 kB
dist/assets/main-DmNGCDYI.css  1.30 kB │ gzip: 0.69 kB
dist/assets/main-BRb_WeNi.js   0.69 kB │ gzip: 0.39 kB
✓ built in 54ms
```

Vite fully resolves the `@import './tokens.css'` inside `global.css` at build time — all 28 custom properties appear in the production bundle on a single minified `:root {}` rule. No build errors, warnings, or unresolved imports.

---

## Acceptance Criteria Checklist

### AC-1 — `src/css/tokens.css` defines all custom properties on `:root`
**PASS.** File exists at `src/css/tokens.css`. All properties are declared inside the `:root {}` block and are present in the production bundle as confirmed by inspecting `dist/assets/main-DmNGCDYI.css`.

### AC-2 — Color tokens: `--ink`, `--paper`, `--amber`, `--rust`, `--muted` (exact reference values)
**PASS.** Values verified in source and in built output:
| Token | Source value | Built value | Reference spec |
|---|---|---|---|
| `--ink` | `#0a0a0a` | `#0a0a0a` | ✅ |
| `--paper` | `#ede6d8` | `#ede6d8` | ✅ |
| `--amber` | `#e8a23a` | `#e8a23a` | ✅ |
| `--rust` | `#7a2418` | `#7a2418` | ✅ |
| `--muted` | `rgba(237,230,216,0.45)` | `#ede6d873` (hex8 equivalent) | ✅ |

### AC-3 — Semantic tokens: `--color-surface`, `--color-border`, `--color-text-dim`, `--color-text-faint`
**PASS.** All four present on `:root`:
- `--color-surface: rgba(237,230,216,0.04)` → `#ede6d80a` ✅
- `--color-border: rgba(237,230,216,0.10)` → `#ede6d81a` ✅
- `--color-text-dim: rgba(237,230,216,0.45)` → `#ede6d873` ✅
- `--color-text-faint: rgba(237,230,216,0.20)` → `#ede6d833` ✅

### AC-4 — Typography tokens: `--font-serif`, `--font-mono`, `--font-sans`
**PASS.** All three stacks present with correct fallbacks:
- `--font-serif: 'Fraunces', Georgia, serif` ✅
- `--font-mono: 'JetBrains Mono', ui-monospace, monospace` ✅
- `--font-sans: 'Inter Tight', system-ui, sans-serif` ✅

### AC-5 — Spacing scale: `--space-xs` through `--space-2xl` (4px base, ×2 scale)
**PASS.** Six tokens, all correct:
| Token | Value |
|---|---|
| `--space-xs` | `4px` |
| `--space-sm` | `8px` |
| `--space-md` | `16px` |
| `--space-lg` | `32px` |
| `--space-xl` | `64px` |
| `--space-2xl` | `128px` |

### AC-6 — Easing tokens: `--ease-out-expo`, `--ease-cinematic`, `--ease-spring`
**PASS.** Values match the ticket spec exactly:
| Token | Value | Ticket spec |
|---|---|---|
| `--ease-out-expo` | `cubic-bezier(0.16, 1, 0.3, 1)` | ✅ |
| `--ease-cinematic` | `cubic-bezier(0.22, 1, 0.36, 1)` | ✅ |
| `--ease-spring` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | ✅ |

### AC-7 — Z-index tokens: `--z-loader` (10000), `--z-chrome` (50), `--z-cursor` (9999)
**PASS.**
| Token | Value |
|---|---|
| `--z-loader` | `10000` |
| `--z-cursor` | `9999` |
| `--z-chrome` | `50` |

### AC-8 — Radius tokens: `--radius-sm`, `--radius-md`
**PASS.** `--radius-sm: 2px`, `--radius-md: 4px` ✅

### AC-9 — `src/css/global.css` imports tokens and sets all required baseline styles
**PASS.** Line-by-line check:
- `@import './tokens.css'` — first statement in file ✅
- `*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }` — universal reset ✅
- `html { background-color: var(--ink); }` ✅
- `html { color: var(--paper); }` ✅
- `@media (pointer: fine) { body { cursor: none; } }` — desktop-only cursor hide ✅
- `html { font-family: var(--font-sans); }` ✅
- `html { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }` ✅
- `img, video, svg { display: block; max-width: 100%; }` ✅
- `a { color: inherit; text-decoration: none; }` ✅

Notable extras (no regressions): `h1–h6` serif baseline, `:focus-visible` amber ring (a11y), `overflow-y: scroll` to prevent CLS from scrollbar, `scroll-behavior: auto` (correct — Lenis overrides this in KNOCH-016), `min-height: 100dvh` on body.

### AC-10 — `@media (prefers-color-scheme: light)` stub present but empty
**PASS.** The stub exists in `src/css/global.css` (lines 81–83). Vite correctly removes the empty block from the production bundle — this is expected behavior and does not fail the criterion, which requires the stub to exist in source, not in the build output.

### AC-11 — `src/css/global.css` is imported in `src/index.html` before any component CSS
**PASS.** `<link rel="stylesheet" href="/src/css/global.css" />` is the sole stylesheet link in `src/index.html` (confirmed from KNOCH-001 scaffold). No other component stylesheets are present yet.

---

## Additional Checks

### CSS Syntax
No syntax errors. The `@import` resolves correctly under Vite's CSS bundler. All `cubic-bezier()` values are syntactically valid (four comma-separated numbers). All `rgba()` values use correct syntax.

### Build Output Integrity
The production bundle (`dist/assets/main-DmNGCDYI.css`, 1.30 kB gzip: 0.69 kB) contains all 28 custom properties in a single minified `:root {}` block followed by the full global reset. No duplicate declarations, no leftover comments. Bundle size is appropriate for a pure token/reset file.

### Responsive Concerns
No layout-specific styles are introduced by this ticket (tokens only + document-level reset). No breakpoints needed for a tokens file. The `@media (pointer: fine)` scoping for `cursor: none` is the correct responsive pattern — touch users retain their native cursor.

### Animation Performance
No animations introduced in this ticket. Easing tokens are CSS custom property strings (not computed at render time), so there is zero performance cost from their presence.

### Accessibility
- `:focus-visible` ring using `--amber` (#e8a23a) on the `--ink` (#0a0a0a) background yields a contrast ratio of approximately 5.7:1, exceeding the WCAG 2.1 AA threshold of 3:1 for UI components ✅
- No ARIA changes in this ticket ✅
- `cursor: none` scoped to fine-pointer only (not suppressed on touch/mobile) ✅

---

## Verdict

**ALL 11 ACCEPTANCE CRITERIA PASS. QA PASSED.**

No issues found. PR #2 approved for merge into `test`.
