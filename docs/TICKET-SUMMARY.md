# Knoch Media — Ticket Summary

> **Living document.** Updated whenever tickets are created, modified, split, or closed.  
> Last updated: 2026-05-05 | Total tickets: 21 | Open: 17 | In progress: 0 | In review: 1 | Done: 3

---

## Status Legend

| Symbol | Meaning |
|--------|---------|
| `⬜` | Open — not started |
| `🔵` | In progress |
| `✅` | Done — merged to `test` |
| `🚀` | Shipped — squash-merged to `main` |
| `🔁` | Needs revision (tester/review feedback) |
| `❌` | Blocked |

---

## Phase 1 — Foundation

These must be completed before any other ticket can be built. No component should be started until KNOCH-001 and KNOCH-002 are done. KNOCH-016 (Lenis) must be done before KNOCH-007 (reel).

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-001](tickets/KNOCH-001.md) | Project Scaffolding & Build Setup (Vite) | `✅` | `feature/KNOCH-001-project-scaffold` | Entry point for all other work |
| [KNOCH-002](tickets/KNOCH-002.md) | Design Tokens & CSS Custom Properties | `✅` | `feature/KNOCH-002-design-tokens` | QA PASSED — merged to test |
| [KNOCH-016](tickets/KNOCH-016.md) | Smooth Scrolling — Lenis + ScrollTrigger Sync | `✅` | `feature/KNOCH-016-lenis-smooth-scroll` | QA PASSED — merged to test |
| [KNOCH-003](tickets/KNOCH-003.md) | Cinematic Chrome Navigation + Timecode Bar | `🔵` | `feature/KNOCH-003-chrome-navigation` | PR #4 open — dev → test |
| [KNOCH-004](tickets/KNOCH-004.md) | Custom Cursor & Film-Grain Overlay | `⬜` | — | Desktop only |

---

## Phase 2 — Homepage Sections

Build top-to-bottom in scroll order. Test the complete homepage scroll experience before moving to Phase 3.

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-005](tickets/KNOCH-005.md) | Hero — Film Counter Loader + Reveal Sequence | `⬜` | — | LCP element — preload hero image |
| [KNOCH-006](tickets/KNOCH-006.md) | Interlude — Word-by-Word Scroll Reveal | `⬜` | — | Manifesto quote section |
| [KNOCH-007](tickets/KNOCH-007.md) | Horizontal Reel — Pinned Scroll Carousel | `⬜` | — | Most complex interaction; needs KNOCH-016 |
| [KNOCH-008](tickets/KNOCH-008.md) | Pinned Frame — Parallax + Animated Counters | `⬜` | — | Studio stats section |
| [KNOCH-009](tickets/KNOCH-009.md) | Testimonial Pull-Quote Section | `⬜` | — | Scroll-stagger reveal |
| [KNOCH-010](tickets/KNOCH-010.md) | Portfolio Grid — Asymmetric 12-Col Archive | `⬜` | — | 7 tiles, contact-sheet layout |

---

## Phase 3 — Secondary Pages

Build in this order: About → Portfolio page → Project detail → Contact → Footer.

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-013](tickets/KNOCH-013.md) | About / Story Section (about.html) | `⬜` | — | Sticky split layout |
| [KNOCH-011](tickets/KNOCH-011.md) | Portfolio Filter System (portfolio.html) | `⬜` | — | Category tabs, URL hash state |
| [KNOCH-012](tickets/KNOCH-012.md) | Project Detail View + Video Lightbox | `⬜` | — | Expanding tile transition |
| [KNOCH-014](tickets/KNOCH-014.md) | Contact — Multi-Step Qualified Inquiry Form | `⬜` | — | 3-step form + Calendly sidebar |
| [KNOCH-015](tickets/KNOCH-015.md) | Footer — Credits Bar + Sitemap Variant | `⬜` | — | Two variants: minimal + expanded |

---

## Phase 4 — Integrations

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-017](tickets/KNOCH-017.md) | YouTube Integration — Lightbox + Showreel | `⬜` | — | Lazy iframe inject on click |
| [KNOCH-018](tickets/KNOCH-018.md) | Instagram Feed Integration | `⬜` | — | Behold.so or static fallback |

---

## Phase 5 — Polish & Ship

Run in this exact order: perf first (changes markup), then mobile (tests perf changes on device), then a11y (audits finished components).

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-019](tickets/KNOCH-019.md) | Performance Optimization — Images, Build, CWV | `⬜` | — | Lighthouse ≥85 mobile target |
| [KNOCH-020](tickets/KNOCH-020.md) | Responsive / Mobile Adaptations | `⬜` | — | 800px breakpoint; reel → CSS snap |
| [KNOCH-021](tickets/KNOCH-021.md) | Accessibility Pass — WCAG 2.1 AA | `⬜` | — | Reduced motion, focus, ARIA |

---

## Dependency Graph

```
001 (scaffold)
 └─ 002 (tokens)
     ├─ 003 (nav)
     ├─ 004 (cursor)
     ├─ 016 (Lenis)
     │   └─ 007 (reel) ─────────────────┐
     ├─ 005 (hero)                       │
     │   └─ 006 (interlude)              │
     │       └─ [007] ──── 008 ── 009 ──┤
     │                                   │
     │                          010 (grid)┤
     │                          011      │
     │                          012 ─────┤
     │                          013      │
     │                          014      │
     │                          015      │
     │                                   │
     ├─ 017 (YouTube) ── links into 012 ─┘
     ├─ 018 (Instagram)
     │
     └─ 019 (perf) → 020 (mobile) → 021 (a11y)
```

---

## Pages & Entry Points

| Page | File | Key Tickets |
|------|------|-------------|
| Homepage | `src/index.html` | 003–010, 015, 017 |
| Portfolio | `src/portfolio.html` | 011, 012 |
| Project detail | `src/project.html` | 012, 017 |
| About | `src/about.html` | 013, 018 |
| Contact / Booking | `src/contact.html` | 014 |

---

## Design System Reference

| Token | Value | Used in |
|-------|-------|---------|
| `--ink` | `#0a0a0a` | All backgrounds |
| `--paper` | `#ede6d8` | All text |
| `--amber` | `#e8a23a` | Accents, active states |
| `--rust` | `#7a2418` | Sparingly (error states) |
| `--font-serif` | Fraunces 300/400 | Headlines |
| `--font-mono` | JetBrains Mono 400/500 | UI chrome, labels |
| `--font-sans` | Inter Tight 400/500 | Body copy |
| `--z-loader` | `10000` | Film counter loader |
| `--z-cursor` | `9999` | Custom cursor |
| `--z-chrome` | `50` | Fixed nav overlay |

---

## Changelog

All modifications to this document and ticket files are logged here. Tester agent and code review feedback should be recorded as entries.

---

### 2026-05-05 — KNOCH-003 implementation complete — PR #4 open

**Action:** Implemented and PR opened dev → test
**Tickets affected:** KNOCH-003
**Reason:** Builder agent completed the chrome navigation overlay
**Changes:**
- KNOCH-003: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-003-chrome-navigation` created, implemented, merged into `dev`
- PR #4 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/4
- Files delivered: `src/css/chrome.css`, `src/js/chrome.js`, `src/index.html` updated, `src/js/main.js` updated
- Build: 14 modules, 3.29 kB CSS / 1.27 kB gz, 133.02 kB JS / 50.04 kB gz, 79ms
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-016 QA PASSED

**Action:** QA run by Tester Agent. All 9 acceptance criteria pass.
**Tickets affected:** KNOCH-016
**Reason:** Lenis module verified against ticket spec, Lenis v1.3.23 API, and production bundle.
**Changes:**
- KNOCH-016: Status changed IN REVIEW → QA PASSED
- PR #3 merged: dev → test
- Dashboard badge updated: In Review → QA Passed (green)
- Stat counts updated: In review 1→0, Done 2→3
**Passing checks:** All 9 ACs — lenis in package.json, constructor options (duration/easing/orientation/smoothWheel/autoRaf), GSAP ticker add + lagSmoothing(0), ScrollTrigger proxy with scrollTop getter/setter + getBoundingClientRect, lenis.on('scroll') sync, pointer:coarse touch guard, stopLenis/startLenis exports, 5 named exports from lenis.js, scrollTo() with duration 1.5 + touch fallback, main.js bootstrap, npm run build clean.
**Full report:** `docs/test-reports/KNOCH-016-test-report.md`
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-016 implementation complete — PR #3 open

**Action:** Implemented and PR opened dev → test
**Tickets affected:** KNOCH-016
**Reason:** Builder agent completed Lenis smooth scroll layer
**Changes:**
- KNOCH-016: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-016-lenis-smooth-scroll` created, implemented, merged into `dev`
- PR #3 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/3
- Files delivered: `src/js/lenis.js` (Lenis module + GSAP sync), `src/js/main.js` (updated bootstrap)
- Build verified: vite v8.0.10, 12 modules, 131.73 kB JS / 49.59 kB gzip (expected — GSAP+Lenis bundled)
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-002 QA PASSED

**Action:** QA run by Tester Agent. All 11 acceptance criteria pass.
**Tickets affected:** KNOCH-002
**Reason:** Design token layer verified against ticket spec, reference values, and production build output.
**Changes:**
- KNOCH-002: Status changed IN REVIEW → QA PASSED
- PR #2 merged: dev → test
- Dashboard badge updated: In Review → QA Passed (green)
- Stat counts updated: In review 1→0, Done 1→2
**Passing checks:** All 11 ACs — tokens.css on :root, exact color values (#0a0a0a/#ede6d8/#e8a23a/#7a2418/rgba), 4 semantic tokens, 3 font stacks, 6-step spacing scale (4–128px), 3 easing cubic-beziers, z-index layers (10000/9999/50), 2 radius tokens, global.css reset + imports, pointer:fine cursor scoping, light-mode stub, global.css linked in index.html.
**Full report:** `docs/test-reports/KNOCH-002-test-report.md`
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-002 implementation complete — PR #2 open

**Action:** Implemented and PR opened dev → test  
**Tickets affected:** KNOCH-002  
**Reason:** Builder agent completed the full design token layer  
**Changes:**
- KNOCH-002: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-002-design-tokens` created, implemented, and merged into `dev`
- PR #2 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/2
- Files delivered: `src/css/tokens.css` (all design tokens on `:root`), `src/css/global.css` (imports tokens + baseline reset)
- Build verified: `vite v8.0.10`, 5 modules transformed, `dist/` emitted cleanly in 110ms
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-001 QA PASSED on re-test

**Action:** Re-test by Tester Agent. All 8 acceptance criteria now pass.
**Tickets affected:** KNOCH-001
**Reason:** Builder applied the two fixes identified in the initial QA run: `src/js/main.js` and `src/css/global.css` were created as empty stub files. `npm run build` now exits cleanly (`vite v8.0.10`, 5 modules transformed, `dist/` emitted with hashed assets in 33ms).
**Changes:**
- KNOCH-001: Status changed NEEDS FIXES → QA PASSED
- Dashboard badge updated: Needs Fixes → QA Passed (green)
- Stat counts updated: In progress 1→0, Done 0→1
**Passing checks:** All 8 ACs — directory tree, vanilla Vite, dev script, npm run build (now PASS), Google Fonts in index.html, .gitignore, package scripts, vite.config base + outDir.
**Full report:** `docs/test-reports/KNOCH-001-test-report.md` (Re-test section appended)
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-001 QA FAILED — Needs Fixes

**Action:** QA run by Tester Agent. Build pipeline blocked by missing stub files.
**Tickets affected:** KNOCH-001
**Reason:** `npm run build` exits with a hard error — `src/js/main.js` does not exist (only `.gitkeep` present). `src/index.html` references this file as a `<script type="module">` entry, so Vite cannot resolve it and emits no `dist/` output. `src/css/global.css` is similarly absent but does not block the build on its own.
**Changes:**
- KNOCH-001: Status changed IN REVIEW → NEEDS FIXES
**Failing checks:**
1. **BLOCKER — AC-4 (`npm run build`):** `Error: Failed to resolve /src/js/main.js from src/index.html`. Fix: create `src/js/main.js` as a minimal stub ES module.
2. **LOW — `src/css/global.css` missing:** Browser will 404 on the stylesheet link. Fix: create `src/css/global.css` as an empty stub (content will be added in KNOCH-002).
**Passing checks:** AC-1 (directory tree), AC-2 (vanilla Vite), AC-5 (Google Fonts in index.html), AC-6 (.gitignore), AC-7 (package scripts), AC-8 (vite.config base + outDir), GSAP/Lenis as npm deps.
**Full report:** `docs/test-reports/KNOCH-001-test-report.md`
**Requested by:** Tester Agent

---

### 2026-05-05 — KNOCH-001 completed and PR open

**Action:** Implemented, merged to dev, PR open dev → test  
**Tickets affected:** KNOCH-001  
**Reason:** Builder agent completed full scaffold implementation  
**Changes:**
- KNOCH-001: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-001-project-scaffold` created, implemented, and merged into `dev`
- PR #1 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/1
- `test` branch created from `main` and pushed to remote
- Files delivered: `package.json`, `vite.config.js`, `src/index.html`, `src/css/`, `src/js/`, `src/assets/`, `.gitignore` updated, all reference files organised into `src/reference/` and `src/design/`
**Requested by:** Builder agent

---

### 2026-05-05 — Initial ticket creation

**Action:** Created all 21 KNOCH tickets from reference analysis.  
**Source:** `src/reference/final demo.html`, `knoch_homepage_redesign_mockup.html`, `concept_02_cinema_verite demo.html`, `knoch_booking_page_redesign.html`, `knoch_portfolio_page_redesign.html` + `src/design/Knoch-Brand-Design-System.pdf`.  
**Tickets created:** KNOCH-001 through KNOCH-021  
**Created by:** Planner agent

---

<!-- 
TEMPLATE — copy this block when logging a new changelog entry:

### YYYY-MM-DD — [Entry title]

**Action:** [Created / Modified / Closed / Split / Blocked]  
**Tickets affected:** KNOCH-XXX, KNOCH-XXX  
**Reason:** [Why the change was made — tester feedback, design revision, scope change]  
**Changes:** 
- KNOCH-XXX: [what changed in the acceptance criteria or design notes]
- KNOCH-XXX: [new ticket added to cover X]  
**Requested by:** [Tester agent / Enoch / Code review]

-->
