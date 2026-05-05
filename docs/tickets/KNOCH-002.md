# KNOCH-002 — Design Tokens & CSS Custom Properties

## Status: IN REVIEW
## Priority: P0 (critical)
## Epic: EPIC-001 — Foundation

## Title
Design System Foundation: CSS Custom Properties and Global Styles

## Description
Create `src/css/tokens.css` and `src/css/global.css` that encode the entire Knoch design system as CSS custom properties. This is the single source of truth for color, typography, spacing, easing, and z-index layers. No component should hardcode a value that belongs in tokens.

## Acceptance Criteria
- [ ] `src/css/tokens.css` defines all custom properties on `:root`
- [ ] Color tokens: `--ink`, `--paper`, `--amber`, `--rust`, `--muted` (exact values from reference)
- [ ] Additional semantic tokens: `--color-surface`, `--color-border`, `--color-text-dim`, `--color-text-faint`
- [ ] Typography tokens: `--font-serif`, `--font-mono`, `--font-sans` (Inter Tight)
- [ ] Size scale tokens: `--space-xs` through `--space-2xl` (4px base, power-of-2 scale)
- [ ] Easing tokens: `--ease-out-expo`, `--ease-cinematic`, `--ease-spring` (cubic-bezier values mirroring GSAP curves)
- [ ] Z-index tokens: `--z-loader`, `--z-chrome`, `--z-cursor` (10000, 50, 9999)
- [ ] Radius tokens: `--radius-sm`, `--radius-md`
- [ ] `src/css/global.css` imports `tokens.css` and sets: box-sizing reset, `html/body` background/color, `cursor: none` on desktop, `font-family`, `-webkit-font-smoothing`, `img` block display, `a` color inherit
- [ ] A `@media (prefers-color-scheme: light)` stub is present but empty — dark mode is primary
- [ ] `src/css/global.css` is imported in `src/index.html` before any component CSS

## Design Notes
Colors from reference:
- `--ink: #0a0a0a` (near-black background)
- `--paper: #ede6d8` (warm off-white text)
- `--amber: #e8a23a` (accent — gold/amber)
- `--rust: #7a2418` (secondary accent, used sparingly)
- `--muted: rgba(237, 230, 216, 0.45)` (dimmed paper)

Typography from reference:
- `--font-serif: 'Fraunces', Georgia, serif`
- `--font-mono: 'JetBrains Mono', ui-monospace, monospace`
- `--font-sans: 'Inter Tight', system-ui, sans-serif`

Easing values matching GSAP defaults:
- `--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1)`
- `--ease-cinematic: cubic-bezier(0.22, 1, 0.36, 1)`
- `--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1)`

## Tradeoffs Considered
- CSS custom properties over Sass variables: no build step needed for tokens, runtime theming possible, better DevTools visibility.
- Numeric z-index tokens over named layers: `@layer` CSS cascade layers would be cleaner but have less browser support for the mix-blend-mode patterns used in the design.

## Related Tickets
- KNOCH-001 (depends on scaffold)
- All component tickets depend on these tokens
