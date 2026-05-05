# KNOCH-001 — Project Scaffolding & Build Setup

## Status: IN REVIEW
## Priority: P0 (critical)
## Epic: EPIC-001 — Foundation

## Title
Project Scaffolding: File Structure, Build Tool, and Entry Point

## Description
Establish the production repository structure exactly as defined in CLAUDE.md. Set up a minimal Vite build pipeline (no framework) to provide HMR, asset hashing, and an optimized dist output. Wire up the three Google Font families, create `src/index.html` as the single-page entry point, and verify the dev server runs.

## Acceptance Criteria
- [ ] Directory tree matches CLAUDE.md spec: `src/css/`, `src/js/`, `src/assets/`, `src/reference/`, `docs/tickets/`, `dist/`
- [ ] Vite configured with `vanilla` template (no React/Astro)
- [ ] `npm run dev` opens a working dev server on localhost
- [ ] `npm run build` emits a clean `dist/` with hashed asset filenames
- [ ] `src/index.html` is the entry point — includes `<link>` to Google Fonts (Fraunces, Inter Tight, JetBrains Mono) with `font-display: swap`
- [ ] `.gitignore` excludes `node_modules/`, `dist/`, `.env`
- [ ] `package.json` scripts: `dev`, `build`, `preview`
- [ ] Vite config sets `base: '/'` and outputs to `dist/`

## Design Notes
- No framework. Vite vanilla template only.
- GSAP and Lenis will be installed as npm packages (not CDN) for tree-shaking and version locking.
- Font loading order: Fraunces (display serif) → JetBrains Mono (UI mono) → Inter Tight (body). Preconnect both Google Fonts origins.
- Keep `src/reference/` as read-only reference material — never import from it in production code.

## Tradeoffs Considered
- **Vite vs. no bundler**: Vite chosen for HMR speed, native ESM, and asset optimization. The reference files use CDN GSAP; moving to npm package gives us version control and tree-shaking.
- **Single HTML vs. multi-page**: Single `index.html` for now. Portfolio detail views and booking page will be separate HTML files added in later tickets — Vite supports multi-page via `build.rollupOptions.input`.

## Related Tickets
- KNOCH-002 (design tokens depend on this structure)
- All implementation tickets depend on this scaffold
