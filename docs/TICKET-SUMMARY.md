# Knoch Media — Ticket Summary

> **Living document.** Updated whenever tickets are created, modified, split, or closed.  
> Last updated: 2026-05-06 | Total tickets: 30 | Open: 5 | In progress: 0 | In review: 1 | Done: 20 | Deferred: 4 | Phases 1–3 shipped to main 🚀

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
| [KNOCH-001](tickets/KNOCH-001.md) | Project Scaffolding & Build Setup (Vite) | `🚀` | `feature/KNOCH-001-project-scaffold` | Shipped — Phase 1 squash to main |
| [KNOCH-002](tickets/KNOCH-002.md) | Design Tokens & CSS Custom Properties | `🚀` | `feature/KNOCH-002-design-tokens` | Shipped — Phase 1 squash to main |
| [KNOCH-016](tickets/KNOCH-016.md) | Smooth Scrolling — Lenis + ScrollTrigger Sync | `🚀` | `feature/KNOCH-016-lenis-smooth-scroll` | Shipped — Phase 1 squash to main |
| [KNOCH-003](tickets/KNOCH-003.md) | Cinematic Chrome Navigation + Timecode Bar | `🚀` | `feature/KNOCH-003-chrome-navigation` | Shipped — Phase 1 squash to main |
| [KNOCH-004](tickets/KNOCH-004.md) | Custom Cursor & Film-Grain Overlay | `🚀` | `feature/KNOCH-004-cursor-film-grain` | Shipped — Phase 1 squash to main |

---

## CMS — Content Layer (Sanity)

KNOCH-022 and KNOCH-023 are infrastructure — implement these first. The wiring tickets (024–026) slot in immediately after their dependent section is built. Do not start 024 before 009, or 025 before 007. KNOCH-026 can start now (005 is done).

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-022](tickets/KNOCH-022.md) | Sanity Project Init + Schema Definitions | `✅` | `feature/KNOCH-022-023-sanity-cms-setup` | Studio scaffolded; testimonial, galleryCollection, service schemas deployed; all 5 testimonials + 7 collections entered |
| [KNOCH-023](tickets/KNOCH-023.md) | JS Content-Fetch Layer | `✅` | `feature/KNOCH-022-023-sanity-cms-setup` | `src/js/sanity.js` built with hardened fetch, logging, and `imageUrl()` helper; meta tags in index.html |
| [KNOCH-024](tickets/KNOCH-024.md) | Wire Testimonials to Sanity | `✅` | `feature/KNOCH-024-wire-testimonials-sanity` | QA PASSED — merged to test |
| [KNOCH-025](tickets/KNOCH-025.md) | Wire Gallery Reel to Sanity | `✅` | `feature/KNOCH-007-horizontal-reel` | `main.js` fetches `getFeaturedCollections()` → `initReel()`; 3 featured collections with Sanity CDN images; `subtitle` field added to schema |
| [KNOCH-026](tickets/KNOCH-026.md) | Migrate Hero Images to Sanity CDN | `⏸` | — | Deferred — hero is LCP-critical and design-tied; static files are the correct approach |
| [KNOCH-027](tickets/KNOCH-027.md) | Wire About Page to Sanity | `⬜` | — | After KNOCH-013 (about page) is built |
| [KNOCH-028](tickets/KNOCH-028.md) | Wire Services Page to Sanity | `⬜` | — | After services page is built |
| [KNOCH-029](tickets/KNOCH-029.md) | Blog Listing Page | `⏸` | — | Deferred — blog schema needs redesign (dynamic related posts, YouTube + Instagram content types) |
| [KNOCH-030](tickets/KNOCH-030.md) | Blog Post Detail Page | `⏸` | — | Deferred — blocked by KNOCH-029 redesign |

---

## Phase 2 — Homepage Sections

Build top-to-bottom in scroll order. Wire each section to Sanity immediately after it is built (see CMS section above). Test the complete homepage scroll experience before moving to Phase 3.

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-005](tickets/KNOCH-005.md) | Hero — Film Counter Loader + Reveal Sequence | `🚀` | `feature/KNOCH-005-hero-section` | Shipped — Phase 2 squash to main (hero images stay static) |
| [KNOCH-006](tickets/KNOCH-006.md) | Interlude — Word-by-Word Scroll Reveal | `🚀` | `feature/KNOCH-006-interlude-manifesto` | Shipped — Phase 2 squash to main |
| [KNOCH-007](tickets/KNOCH-007.md) | Horizontal Reel — Pinned Scroll Carousel | `🚀` | `feature/KNOCH-007-horizontal-reel` | Shipped — Phase 2 squash to main (Sanity-driven, KNOCH-025 included) |
| [KNOCH-008](tickets/KNOCH-008.md) | Pinned Frame — Parallax + Animated Counters | `🚀` | `feature/KNOCH-008-pinned-frame` | Shipped — Phase 2 squash to main |
| [KNOCH-009](tickets/KNOCH-009.md) | Testimonial Pull-Quote Section | `🚀` | `feature/KNOCH-009-testimonial-section` | Shipped — Phase 2 squash to main |
| [KNOCH-010](tickets/KNOCH-010.md) | Portfolio Grid — Asymmetric 12-Col Archive | `🚀` | `feature/KNOCH-010-portfolio-grid` | Shipped — Phase 2 squash to main |

---

## Phase 3 — Secondary Pages

Build in this order: About → Portfolio page → Project detail → Contact → Footer.

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-013](tickets/KNOCH-013.md) | About / Story Section (about.html) | `🚀` | `feature/KNOCH-013-about-story-section` | Shipped — Phase 3 squash to main |
| [KNOCH-011](tickets/KNOCH-011.md) | Portfolio Filter System (portfolio.html) | `🚀` | `feature/KNOCH-011-portfolio-filter` | Shipped — Phase 3 squash to main |
| [KNOCH-012](tickets/KNOCH-012.md) | Project Detail View + Video Lightbox | `🚀` | `feature/KNOCH-012-project-detail-lightbox` | Shipped — Phase 3 squash to main (KNOCH-036 polish included) |
| [KNOCH-014](tickets/KNOCH-014.md) | Contact — Multi-Step Qualified Inquiry Form | `🚀` | `feature/KNOCH-014-contact-form` | Shipped — Phase 3 squash to main |
| [KNOCH-015](tickets/KNOCH-015.md) | Footer — Credits Bar + Sitemap Variant | `🚀` | `feature/KNOCH-015-footer` | Shipped — Phase 3 squash to main (PR #23 + release squash) |

---

## Phase 4 — Integrations

| ID | Title | Status | Branch | Notes |
|----|-------|--------|--------|-------|
| [KNOCH-017](tickets/KNOCH-017.md) | YouTube Integration — Lightbox + Showreel | `🔵` | `feature/KNOCH-017-youtube-integration` | Lazy iframe inject on click |
| [KNOCH-018](tickets/KNOCH-018.md) | Instagram Feed Integration | `⏸` | — | Deferred — footer icon link covers it |

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
     │   └─ 007 (reel) ──────────────────────────┐
     ├─ 005 (hero) ✅                              │
     │   └─ 006 (interlude)                       │
     │       └─ [007] ──── 008 ── 009 ────────────┤
     │                                            │
     │                               010 (grid)   │
     │                               011          │
     │                               012 ─────────┤
     │                               013          │
     │                               014          │
     │                               015          │
     │                                            │
     ├─ 017 (YouTube) ── links into 012 ──────────┘
     ├─ 018 (Instagram)
     │
     └─ 019 (perf) → 020 (mobile) → 021 (a11y)

CMS layer (cuts across phases — wire each section after it is built):

022 (Sanity init) ✅
 └─ 023 (fetch layer) ✅
     ├─ 024 (wire testimonials) ── after 009
     ├─ 025 (wire reel) ✅       ── done with 007
     ├─ 026 (hero images) ⏸     ── deferred (hero stays static)
     ├─ 027 (wire about) ──       after 013
     └─ 028 (wire services)     ── after services page built
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
| Blog listing | `src/blog.html` | 029 |
| Blog post detail | `src/blog-post.html` | 030 |
| Internal gallery | `src/gallery/[name].html` | 012 (Jojo, Woodsmen) |

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

### 2026-05-06 — KNOCH-017 hero PLAY REEL CTA descoped (still IN REVIEW)

**Action:** Hero CTA portion of KNOCH-017 dropped post-review; lightbox + reel-routing portions kept
**Tickets affected:** KNOCH-017
**Reason:** Enoch reviewed the freshly-pushed hero CTA in the browser, screenshotted it, and rejected immediately: "remove that youtube section. it is ruining the hero." The dark `maxresdefault.jpg` thumbnail competed with the moody hero backdrop and broke the headline → SCROLL-prompt cinematic pacing. The other two KNOCH-017 deliverables — `&color=white` on the lightbox iframe URL, and reel cards routing through the lightbox instead of opening in a new tab — stand. The lightbox itself is still reachable from every video tile, reel card, and project-page CTA, so removing the hero entry point doesn't strand any video; visitors still have multiple cinematic paths to play any film.
**Changes:**
- src/index.html: `<button class="hero-reel">` markup + `<link rel="stylesheet" href="/css/hero-reel.css">` removed; hero composition restored to its KNOCH-005 shape (meta → headline → SCROLL prompt).
- src/js/main.js: `initHeroReel` import + call removed.
- src/js/hero.js: opacity tween for `.hero-reel` removed from the reveal timeline.
- src/css/hero-reel.css: deleted (was new in this PR — never reached test).
- src/js/hero-reel.js: deleted (same).
- docs/tickets/KNOCH-017.md: Showreel-section AC items struck through with a 2026-05-06 DESCOPED note; rationale + reference to feedback memory recorded.
- New auto-memory: `feedback_hero_no_extra_ctas.md` — never insert CTAs / video chips / chrome between the hero headline and the SCROLL prompt; the cinematic pacing is the whole composition.
- PR #24 auto-updates from dev — no PR re-open needed.
**Requested by:** Enoch (live screenshot review)

---

### 2026-05-06 — KNOCH-017 PR opened — IN REVIEW

**Action:** PR #24 opened dev → test
**Tickets affected:** KNOCH-017
**Reason:** Builder closed the three remaining KNOCH-017 acceptance gaps left by KNOCH-012's lightbox base. (1) `&color=white` added to the YouTube iframe URL so the player progress bar renders white instead of red. (2) Reel cards with `linkType: 'youtube'` now route through `openVideoLightbox()` instead of opening in a new tab — new shared parser `src/js/youtube-id.js` extracts the 11-char ID from any youtu.be / watch?v= / embed / shorts URL. (3) New cinematic PLAY REEL CTA in the hero — bordered card overlaid on the showreel's YouTube `maxresdefault.jpg`, with the chrome details lifted from the cinema-vérité reference (`▢ 16:9 · 24FPS` chip top-left, `TC 00:01:42:11` top-right, 40 px play ring centre, bordered `▶ PLAY REEL` chip bottom-right). Default video is Hs25JK7WcZQ (Rapha Records — the studio's brand introduction film); swap by editing `data-youtube-id` on the `.hero-reel` button.
**Changes:**
- KNOCH-017: Status IN PROGRESS → IN REVIEW; PR line added to the ticket file.
- PR #24 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/24
- Files delivered:
  - `src/js/youtube-id.js` — new ~65-line parser; bare-ID short-circuit + URL-shape branching
  - `src/js/hero-reel.js` — new ~55-line module; thumbnail probe + click handler
  - `src/css/hero-reel.css` — new ~165-line stylesheet; button + chrome overlays + reduced-motion + ≤800px mobile
  - `src/js/video-lightbox.js` — `&color=white` added to iframe URL + comment block documenting all params
  - `src/js/reel.js` — `handleCardClick` rewritten as a clean three-branch router (youtube → lightbox, external-gallery → new tab, default → same-tab)
  - `src/js/hero.js` — opacity tween for `.hero-reel` at offset 1.4 in the reveal timeline (same beat as `#hero-sub`)
  - `src/js/main.js` — `initHeroReel()` import + call after `initHero()`
  - `src/index.html` — `<link rel="stylesheet" href="/css/hero-reel.css">` + `<button class="hero-reel" data-youtube-id="Hs25JK7WcZQ">…</button>` with chrome spans
- Build: 87 modules → emit; main 32.59 kB / 10.12 kB gz; main.css 27.38 kB; 166 ms.
- Header counts updated: In progress 1→0, In review 0→1.
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-017 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-017
**Reason:** First ticket of Phase 4. The lightbox modal itself was built under KNOCH-012 and is already wired through tile-router.js for archive tiles, portfolio cards, and the project-page CTA — KNOCH-017 closes the remaining gaps: (1) honour the `color=white` AC param on the iframe URL, (2) route reel cards with `linkType: 'youtube'` through the lightbox instead of opening a new tab, (3) add the "PLAY REEL" hero CTA with cinematic chrome (`▢ 16:9 · 24FPS` + `TC` overlay + maxresdefault thumbnail) per the cinema-vérité reference.
**Changes:**
- Branch `feature/KNOCH-017-youtube-integration` cut from dev (a6dabb4).
- KNOCH-017: Status TODO → IN PROGRESS, Branch line added to ticket file.
- TICKET-SUMMARY row swapped to 🔵 with branch column filled.
- Header counts updated: Open 6→5, In progress 0→1.
**Requested by:** /implement-ticket auto-detect (Enoch)

---

### 2026-05-06 — Phase 1 + Phase 2 status backfilled to SHIPPED 🚀

**Action:** Bookkeeping flip — 11 ticket files + table rows + dashboard badges
**Tickets affected:** Phase 1 — KNOCH-001, KNOCH-002, KNOCH-003, KNOCH-004, KNOCH-016. Phase 2 — KNOCH-005, KNOCH-006, KNOCH-007, KNOCH-008, KNOCH-009, KNOCH-010.
**Reason:** Phase 1 (squashed to main as `676f2bd` / `e2a0549`) and Phase 2 (squashed to main as `f14c0c1`) both reached production weeks ago, but their trackers were still showing `✅ QA Passed` instead of the `🚀 Shipped` state introduced when Phase 3 closed. This entry brings the trackers in line with reality so all three completed phases read consistently.
**Changes:**
- 11 ticket files: Status `QA PASSED` (or `DONE — merged to test` for KNOCH-007) → `SHIPPED — Phase 1 squash to main` / `SHIPPED — Phase 2 squash to main`.
- TICKET-SUMMARY.md Phase 1 + Phase 2 table rows: `✅` → `🚀` with shipped notes; ticket-flavour details preserved (KNOCH-005 hero static, KNOCH-007 Sanity + KNOCH-025).
- Header line: `Phase 3 shipped to main 🚀` → `Phases 1–3 shipped to main 🚀`.
- dashboard.html: 11 row badges flipped `QA Passed` → `Shipped` (badge-pass → badge-shipped).
- Counts unchanged: 🚀 already counts under "Done" so totals (Done: 20) stay the same.

**Requested by:** Enoch

---

### 2026-05-06 — Phase 3 SHIPPED — squash-merged to main 🚀

**Action:** test → main squash merge (commit `30da046`)
**Tickets affected:** KNOCH-011, KNOCH-012, KNOCH-013, KNOCH-014, KNOCH-015 (and the bundled KNOCH-036 polish pass)
**Reason:** Phase 3 closed at 5/5. The five secondary-page tickets plus the polish bundle landed on test in sequence and all passed live review on dev — the squash to main is the phase-milestone commit that takes the site from "homepage only" to "full multi-page studio site."
**Changes:**
- Single squash commit on main covering 50+ test commits.
- All five Phase 3 ticket statuses flipped ✅ → 🚀 in their ticket files and in the table above.
- Header counts updated: In review 1→0, Done 19→20. Phase 3 shipped marker added to header line.
- Branch lineage: main `30da046` ← squash of test `5037e92` ← merge of dev (PR #23 + earlier).
- Live site (knochmedia.xyz on Vercel) will redeploy from main automatically.

**What's on main now:**
- /index.html — homepage with hero, interlude, reel, frame, testimonials, archive, inquiry CTA, footer (minimal)
- /about.html — pinned chapter narrative, How-We-Work, footer (expanded)
- /portfolio.html — filter tabs (Weddings / Brand / Music), URL-hash sync, Load-more, footer (expanded)
- /project.html — runtime URL-param renderer, sticky metadata, "Keep looking" reel, footer (expanded)
- /contact.html — 3-step inquiry form, scarcity banner, ?type= pre-fill, Calendly sidebar, footer (expanded)

**Next phase:** KNOCH-017 (YouTube integration) is the only un-deferred Phase 4 ticket. Then Phase 5 polish trio (KNOCH-019 perf → KNOCH-020 mobile → KNOCH-021 a11y) before launch.

**Requested by:** Enoch

---

### 2026-05-06 — KNOCH-015 PR opened — IN REVIEW

**Action:** PR #23 opened dev → test
**Tickets affected:** KNOCH-015
**Reason:** Footer module shipped — minimal credits bar (homepage) and expanded sitemap (about / portfolio / project / contact) sharing one `<footer id="site-footer">` host populated by `initFooter(variant)`. Editable constants block in footer.js means roll number / version / contact / social can be bumped in a one-line edit without engineer help.
**Changes:**
- KNOCH-015: Status IN PROGRESS → IN REVIEW.
- PR #23 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/23
- Files delivered:
  - `src/js/footer.js` — constants, SVG icons (Instagram + YouTube, 18×18, currentColor), `initFooter('minimal'|'expanded')`
  - `src/css/footer.css` — base + minimal/expanded/mobile variants; 10 px mono / 0.2em LS / uppercase / paper @ 40 % typography matching chrome
  - `src/index.html`, `src/about.html`, `src/portfolio.html`, `src/project.html`, `src/contact.html` — `<link rel="stylesheet" href="/css/footer.css">` + `<footer id="site-footer">`
  - `src/js/main.js`, `about-main.js`, `portfolio-main.js`, `project-main.js`, `contact-main.js` — initFooter wiring
  - `src/about.html` — `id="process"` on the How-We-Work section so the footer Studio column's Process link deep-links
- Build: 5 HTML entries → emit; footer.css 7.50 kB / 2.37 kB gz; 144 ms.
- Header counts updated: In progress 1→0, In review 0→1.
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-015 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-015
**Reason:** Last open Phase 3 ticket. Footer is shared chrome that lands on every page and unblocks the Phase 3 → main milestone squash.
**Changes:**
- Branch `feature/KNOCH-015-footer` cut from dev (1c581f6).
- KNOCH-015: Status TODO → IN PROGRESS, Branch line added to ticket file.
- TICKET-SUMMARY row swapped to 🔵 with branch column filled.
- Header counts updated: Open 7→6, In progress 0→1.
**Requested by:** /implement-ticket auto-detect (Enoch)

---

### 2026-05-06 — KNOCH-014 QA PASSED — merged to test

**Action:** PR #22 merged dev → test (regular merge, no squash)
**Tickets affected:** KNOCH-014
**Reason:** Contact form page passed live review on dev — banner clears chrome (5rem padding), 3-step transitions smooth, ?type= URL pre-fill works from project CTAs, partner field shows only for wedding inquiries. Banner drop-in animation lands cleanly on load.
**Changes:**
- KNOCH-014: Status 🔵 In review → ✅ Done.
- PR #22 merged; dev and test now aligned at 1c581f6.
- Header counts updated: In review 1→0, Done 18→19.
**Requested by:** Enoch

---

### 2026-05-06 — KNOCH-018 deferred — Instagram embed dropped from scope

**Action:** KNOCH-018 status changed ⬜ Open → ⏸ Deferred
**Tickets affected:** KNOCH-018
**Reason:** Enoch reviewed the Phase 4 ticket list and decided a live Instagram feed adds complexity (paid embed service like Behold.so, or Meta Graph API setup, or manual screenshot updates) without meaningful UX gain on a portfolio site. The plain footer icon link to @knochmedia_ — already in KNOCH-015's scope — is the cleaner solution. Ticket file annotated with the rationale + revisit path (Behold.so) if a feed surfaces in a future iteration.
**Changes:**
- KNOCH-018: Status ⬜ Open → ⏸ Deferred. Header reason added to ticket file.
- TICKET-SUMMARY table row swapped to ⏸ + "Deferred — footer icon link covers it" note.
- Header counts updated: Open 8→7, Deferred 3→4.
**Requested by:** Enoch

---

### 2026-05-06 — KNOCH-014 PR opened — IN REVIEW

**Action:** PR #22 opened dev → test
**Tickets affected:** KNOCH-014
**Reason:** Builder completed Contact / Inquiry form page — /contact.html with 3-step qualified inquiry, Calendly sidebar, ?type= URL pre-fill, and cross-page Inquire link routing.
**Changes:**
- KNOCH-014: Status changed 🔵 In progress → 🔵 In review
- PR #22 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/22
- Files delivered:
  - `src/contact.html` — full page (banner + hero + 2-col body w/ form + sidebar)
  - `src/css/contact-page.css` — page styles (scarcity banner, hero, form fields, tile selectors, step panels w/ slide states, sidebar blocks, mobile collapse)
  - `src/js/contact-page.js` — tile selector + ?type= pre-fill + conditional partner + GSAP step transitions + per-step validation + submit handler (placeholder action)
  - `src/js/contact-main.js` — slim entry
  - `src/index.html` + `src/about.html` + `src/portfolio.html` + `src/project.html` — chrome Inquire links + page CTAs all repointed to /contact.html
  - `src/js/project-page.js` — project CTA href stamped at runtime with ?type=<category>
  - `vite.config.js` — contact.html added as 5th `rollupOptions.input`
- Build: 5 HTML entries → all emit; contact bundle 8.29 kB CSS / 1.91 kB gz, 4.22 kB JS / 1.81 kB gz; 158 ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-014 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-014
**Reason:** Builder agent starting Contact / Inquiry Flow — dedicated /contact.html with 3-step qualified inquiry form, Calendly sidebar, scarcity banner, ?type= URL param pre-fill, and slide-out/in step transitions.
**Changes:**
- KNOCH-014: Status changed ⬜ Open → 🔵 In progress
- Branch `feature/KNOCH-014-contact-form` created from dev
- Header counts updated: Open 9→8, In progress 0→1, In review 1→0, Done 17→18 (KNOCH-012 status reflected as ✅ now that PR #21 is merged; KNOCH-036 polish landed in same merge)
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-012 + KNOCH-036 merged to test (PR #21 closed)

**Action:** PR #21 merged dev → test by Enoch (no tester pass — Enoch reviewed live on the test preview).
**Tickets affected:** KNOCH-012, KNOCH-036
**Reason:** Project Detail View + Video Lightbox (KNOCH-012) and the cross-ticket Phase 3 polish (KNOCH-036) shipped together as the third Phase 3 increment.
**Changes:**
- KNOCH-012: Status 🔵 In review → ✅ Done.
- KNOCH-036: Status 🔵 In review → ✅ Done (bundled in PR #21's merge).
- Test branch advanced to 1ac75fb. dev re-aligned to the same commit per the post-merge ritual so the next branch cuts cleanly.
- main untouched at a629573 — Phase 3 still in progress, no shipping to production this cycle.
**Completed by:** Enoch (manual merge in GitHub)

---

### 2026-05-06 — KNOCH-036 polish folded into PR #21

**Action:** Phase 3 polish bundle merged into dev (cebdc46), folded into PR #21
**Tickets affected:** KNOCH-036 (new ticket, in review), KNOCH-011 / KNOCH-012 / KNOCH-013 / KNOCH-035 (cross-ticket polish)
**Reason:** Enoch reviewed the Phase 3 test preview after PR #21 opened and flagged a bundle of issues spanning multiple tickets — nav structure inconsistencies, homepage trims, testimonial reveal style, portfolio category taxonomy change (Sanity-side rename), about-page split feeling static, project page gallery showing irrelevant placeholder images. Fixes are tightly coupled across the four tickets so they ship as one branch (`feature/KNOCH-036-phase-3-polish`) folded into the still-open PR #21 rather than as a separate PR cycle.
**Changes:**
- KNOCH-036: Created. Status ⬜ Open → 🔵 In review (folded into PR #21).
- Branch `feature/KNOCH-036-phase-3-polish` created from dev, six logical commits, merged into dev as fast-forward
- PR #21 title updated to "feat(KNOCH-012 + KNOCH-036)"; description expanded with the polish item list and additional Test plan checkboxes
- Files touched: src/index.html, src/about.html, src/portfolio.html, src/project.html, src/css/{about,chrome,project-page,reel,testimonial}.css, src/js/{about,portfolio-page,project-page,projects,testimonial}.js, docs/tickets/KNOCH-036.md
- Build: 65 modules → 65, dist HTML totals: index 25.96 / 7.58 gz, about 12.13 / 3.82 gz, portfolio 11.84 / 3.13 gz, project 6.34 / 2.29 gz; 172 ms
**Requested by:** Enoch (reviewed live preview, listed issues in chat)

---

### 2026-05-06 — KNOCH-012 PR opened — IN REVIEW

**Action:** PR #21 opened dev → test
**Tickets affected:** KNOCH-012
**Reason:** Builder completed Project Detail View + Video Lightbox; PR open for tester / code review
**Changes:**
- KNOCH-012: Status changed 🔵 In progress → 🔵 In review
- PR #21 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/21
- Files delivered:
  - `src/js/projects.js` — 12-entry project data module (3 fully populated photo + 4 video + 5 placeholder)
  - `src/css/video-lightbox.css` + `src/js/video-lightbox.js` — full-screen YouTube modal with focus trap, ESC, click-out
  - `src/js/tile-router.js` — central click router (data-project-id → lightbox or expanding-tile transition + project.html navigate)
  - `src/project.html` + `src/css/project-page.css` + `src/js/project-page.js` + `src/js/project-main.js` — standalone project page with hero, sticky metadata, image gallery, CTA
  - `src/portfolio.html` — all 12 cards: data-project-id replaces inline onclick; role="button" added; video-lightbox.css linked
  - `src/index.html` — archive tiles t1–t7 swap data-link-type/data-url for data-project-id; video-lightbox.css linked
  - `src/js/main.js` + `src/js/portfolio-main.js` — initVideoLightbox added
  - `src/js/portfolio-grid.js` + `src/js/portfolio-page.js` — click handlers delegate to handleTileActivate
  - `vite.config.js` — project.html added as fourth `rollupOptions.input`
- Build: 60 → 70 modules; dist/project.html 5.30 kB / 1.94 kB gz; new shared chunks tile-router-*.js 1.69 kB / 0.88 kB gz, video-lightbox-*.js 7.29 kB / 2.56 kB gz; 145 ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-012 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-012
**Reason:** Builder agent starting Project Detail View + Video Lightbox — projects.js data module, video-lightbox component, project.html standalone page with expanding-tile transition, click-handler routing across portfolio.html and homepage archive
**Changes:**
- KNOCH-012: Status changed ⬜ Open → 🔵 In progress
- Branch `feature/KNOCH-012-project-detail-lightbox` created from dev
- Header counts updated: Open 10→9, In progress 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-011 QA PASSED — merged to test

**Action:** PR #20 merged (dev → test) — KNOCH-011 QA PASSED
**Tickets affected:** KNOCH-011
**Reason:** Tester agent verified all 11 acceptance criteria; build clean at 140 ms / 65 modules. Filter state machine traced for `wedding`, `all`, `Load more`, and `portrait` scenarios — exit / entry overlap, `display: none` after exit, count + button toggle all confirmed correct. Hash sync via `replaceState` + `hashchange` with feedback-loop guard works. Three LOW findings, none blocking: (1) `data-category="film"` not implemented — folded into `brand` since there's no Film tab in the AC anyway; (2) count label uses dynamic total instead of hardcoded "47" — more honest given DOM has 12 cards; (3) cards have `tabindex="0"` + inline `onclick` but no keydown handler — outside AC scope (filter tabs are properly keyboard-accessible), flagged for KNOCH-021.
**Changes:**
- KNOCH-011: Status changed IN REVIEW → ✅ QA PASSED
- PR #20 merged with full merge commit (no squash, per workflow)
- All 13 Test plan checkboxes ticked in PR body
- Test report written: `docs/test-reports/KNOCH-011-test-report.md`
- Header counts updated: In review 1→0, Done 16→17
**Completed by:** Tester Agent

---

### 2026-05-06 — KNOCH-011 PR opened — IN REVIEW

**Action:** PR #20 opened dev → test
**Tickets affected:** KNOCH-011
**Reason:** Builder completed Portfolio Filter System (portfolio.html); PR open for tester / code review
**Changes:**
- KNOCH-011: Status changed 🔵 In progress → 🔵 In review
- PR #20 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/20
- Files delivered:
  - `src/portfolio.html` — full page (hero with filter tabs, 12-card grid, count + Load more meta row)
  - `src/css/portfolio-page.css` — page styles (hero, tabs with animated underline, 3-col grid w/ aspect-ratio 4/5 cards, hover-reveal label, meta row, mobile + reduced-motion)
  - `src/js/portfolio-page.js` — filter state machine (PAGE_SIZE=8), GSAP overlapping exit/enter tweens, URL hash sync via replaceState + hashchange, Load more
  - `src/js/portfolio-main.js` — slim entry point (no homepage modules)
  - `src/index.html` — "View the full portfolio →" link in `.reel-intro` panel for discoverability
  - `src/css/reel.css` — `.reel-intro-portfolio-link` styles matching the existing amber-underline mono link vocabulary
  - `vite.config.js` — portfolio.html added as third `rollupOptions.input`
- Build: 60 modules → 65, dist/portfolio.html 14.31 kB / 3.39 kB gz, dist/index.html 26.62 kB / 7.79 kB gz, 132 ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-011 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-011
**Reason:** Builder agent starting Portfolio Filter System (portfolio.html) — category filter tabs, GSAP grid re-layout animation, URL hash sync, "Load more" pagination
**Changes:**
- KNOCH-011: Status changed ⬜ Open → 🔵 In progress
- Branch `feature/KNOCH-011-portfolio-filter` created from dev
- Header counts updated: Open 11→10, In progress 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-013 QA PASSED — merged to test

**Action:** PR #19 merged (dev → test) — KNOCH-013 QA PASSED
**Tickets affected:** KNOCH-013
**Reason:** Tester agent verified all 12 acceptance criteria; build clean at 124 ms / 60 modules. Two findings, both LOW: sticky CSS values aligned to spec exactly during QA (originally `top: 12vh` no `height` → now `top: 10vh; height: 80vh`); image 5's duplicate `data-image="4"` attribute is intentional design (keeps chapter 4 lit through the closing image) and documented in HTML / JS comments.
**Changes:**
- KNOCH-013: Status changed IN REVIEW → ✅ QA PASSED
- PR #19 merged with full merge commit (no squash, per workflow); test branch advanced to include the About page + homepage teaser
- Sticky CSS fix committed on dev as part of QA pass — `src/css/about.css:135-137` now matches AC values
- All 12 Test plan checkboxes ticked in PR body
- Test report written: `docs/test-reports/KNOCH-013-test-report.md`
- Header counts updated: In review 1→0, Done 15→16
**Completed by:** Tester Agent

---

### 2026-05-06 — KNOCH-013 PR opened — IN REVIEW

**Action:** PR #19 opened dev → test
**Tickets affected:** KNOCH-013
**Reason:** Builder completed About / Story page implementation; PR open for tester / code review
**Changes:**
- KNOCH-013: Status changed 🔵 In progress → 🔵 In review
- PR #19 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/19
- Files delivered:
  - `src/about.html` — full page (hero, intro, split layout w/ 4 chapters + 5 images, by-the-numbers, process, CTA)
  - `src/css/about.css` — page styles (sticky split, chapter dim/active, aspect-ratio image frames, mobile collapse, reduced-motion)
  - `src/css/studio-teaser.css` — homepage teaser block styles
  - `src/js/about.js` — hero entrance timeline, IntersectionObserver chapter sync, ScrollTrigger counter tweens
  - `src/js/about-main.js` — slim entry point for the About page (no homepage modules loaded)
  - `src/public/assets/about/about-01..05.jpg` — 5 narrative images (copied from reference homepage cache)
  - `src/index.html` — homepage studio teaser block inserted between #frame and #testimonial; studio-teaser.css linked
  - `src/js/chrome.js` — `.mark` click handler now distinguishes same-page (smooth scroll) from cross-page (browser navigation), fixing back-to-home from /about.html
  - `vite.config.js` — about.html added as second `rollupOptions.input`
- Build: 60 modules, dist/about.html 12.99 kB / 4.06 kB gz, dist/index.html 26.51 kB / 7.77 kB gz, 178 ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-06 — KNOCH-013 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-013
**Reason:** Builder agent starting About / Story page (about.html) — split-layout sticky narrative with scrolling image column, plus homepage studio teaser block
**Changes:**
- KNOCH-013: Status changed ⬜ Open → 🔵 In progress
- Branch `feature/KNOCH-013-about-story-section` created from dev
- Header counts updated: Open 12→11, In progress 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-010 QA PASSED — merged to test

**Action:** PR #14 merged (dev → test) — KNOCH-010 QA PASSED
**Tickets affected:** KNOCH-010
**Reason:** Tester agent verified all 34 acceptance criteria; build clean at 97ms, 35 modules. Asymmetric 12-column archive grid fully implemented: 6 tiles, hover colour-reveal, slide-up labels, film notches on t1, video badge on t7, scroll stagger reveal, per-tile inner parallax, mobile collapse, reduced-motion guards.
**Changes:**
- KNOCH-010: Status changed 🔵 In Review → ✅ QA PASSED
- PR #14 merged with full merge commit; branch `feature/KNOCH-010-portfolio-grid` merged into test
- All test plan checkboxes ticked in PR body
- Test report written: `docs/test-reports/KNOCH-010-test-report.md`
- Header counts updated: In review 1→0, Done 14→15
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-010 PR opened — IN REVIEW

**Action:** PR #14 opened dev → test
**Tickets affected:** KNOCH-010
**Reason:** Builder completed the portfolio archive grid; all 7 tiles built, animations wired, PR open for QA
**Changes:**
- KNOCH-010: Status changed ⬜ Open → 🔵 In Review
- Branch `feature/KNOCH-010-portfolio-grid` created, implemented, merged into `dev`
- PR #14 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/14
- Files delivered:
  - `src/css/portfolio-grid.css` — 12-col grid, 7 tile spans, hover colour reveal, slide-up label, film notches on t1, badge on t7, mobile collapse, reduced-motion
  - `src/js/portfolio-grid.js` — header reveal, tile stagger reveal, per-tile inner parallax
  - `src/index.html` — archive section with 7 tile articles; CSS linked in head
  - `src/js/main.js` — initPortfolioGrid() import + call
- Build: 36 modules, 18.25 kB CSS / 4.30 kB gz, 147.64 kB JS / 54.21 kB gz, 102ms
- Header counts updated: Open 13→12, In review 0→1
**Requested by:** Builder

---

### 2026-05-05 — KNOCH-024 QA PASSED — merged to test

**Action:** PR #13 merged (dev → test) — KNOCH-024 QA PASSED
**Tickets affected:** KNOCH-024
**Reason:** Tester agent verified all 8 acceptance criteria and all 16 additional checks; build clean at 87ms, 27 modules. All blockers clear: `getTestimonials()` confirmed exported, GROQ uses `order(order asc)`, skeleton appended before `await`, `list.remove()` called in catch, `gsap.from` inside try after forEach.
**Changes:**
- KNOCH-024: Status changed IN REVIEW → QA PASSED
- PR #13 merged with full merge commit; branch `feature/KNOCH-024-wire-testimonials-sanity` deleted
- All test plan and AC checkboxes ticked in PR body
- Test report written: `docs/test-reports/KNOCH-024-test-report.md`
- Header counts updated: In review 1→0, Done 13→14
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-024 PR opened — IN REVIEW

**Action:** PR #13 opened dev → test
**Tickets affected:** KNOCH-024
**Reason:** Builder agent completed wiring of testimonial section to Sanity CMS; PR open for tester/code review
**Changes:**
- KNOCH-024: Status changed MERGED TO DEV → IN REVIEW
- PR #13 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/13
- Files delivered:
  - `src/js/testimonial.js` — async initTestimonial(); skeleton → fetch → buildItem() render → GSAP stagger; ScrollTrigger.refresh(); failure collapse
  - `src/css/testimonial.css` — .testimonial-list (flex column), .testimonial-item (max-width 680px, padding 4rem 0), inter-item divider, updated reduced-motion guard
  - `src/index.html` — hardcoded mark/quote/attr removed; section shell only, JS-populated
  - `src/js/main.js` — comment updated for async fire-and-forget pattern
- Build: 27 modules, 14.43 kB CSS / 3.59 kB gz, 143.23 kB JS / 53.05 kB gz, 88ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-024 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-024
**Reason:** Builder agent wiring testimonial section to Sanity CMS; replaces hardcoded single quote with dynamic fetch of all 5 testimonials
**Changes:**
- KNOCH-024: Status changed ⬜ Open → 🔵 In progress
- Branch `feature/KNOCH-024-wire-testimonials-sanity` created from dev
- Header counts updated: Open 14→13, In progress 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-009 QA PASSED — merged to test

**Action:** PR #12 merged (dev → test) — KNOCH-009 QA PASSED
**Tickets affected:** KNOCH-009
**Reason:** Tester agent verified all 8 acceptance criteria and all additional checks; build clean at 87ms, 27 modules
**Changes:**
- KNOCH-009: Status changed IN REVIEW → QA PASSED
- PR #12 merged with full merge commit; branch `feature/KNOCH-009-testimonial-section` deleted
- All test plan checkboxes ticked in ticket file
- Test report written: `docs/test-reports/KNOCH-009-test-report.md`
- Header counts updated: In review 1→0, Done 12→13
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-009 PR opened — IN REVIEW

**Action:** PR #12 opened dev → test
**Tickets affected:** KNOCH-009
**Reason:** Builder agent completed testimonial pull-quote section; PR open for tester/code review
**Changes:**
- KNOCH-009: Status changed MERGED TO DEV → IN REVIEW
- PR #12 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/12
- Files delivered:
  - `src/css/testimonial.css` — section layout (14rem 8vw padding, 1px border-top), quote mark (8rem Fraunces amber), pull-quote (clamp 28–56px, 22ch max-width, em amber italic), attribution (10px mono, 0.3em LS, muted), mobile ≤800px, reduced-motion overrides
  - `src/js/testimonial.js` — GSAP stagger from y:40 opacity:0, duration:1.2, stagger:0.15, expo.out, ScrollTrigger top 75%, once:true, prefers-reduced-motion guard
  - `src/index.html` — testimonial section inserted between #frame and #cta; testimonial.css linked in head
  - `src/js/main.js` — initTestimonial() import and call after initFrame()
- Build: 27 modules, 14.18 kB CSS / 3.54 kB gz, 142.07 kB JS / 52.77 kB gz, 89ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-009 implementation started — IN PROGRESS

**Action:** Feature branch created; implementation in progress
**Tickets affected:** KNOCH-009
**Reason:** Builder agent starting testimonial pull-quote section implementation
**Changes:**
- KNOCH-009: Status changed TODO → IN PROGRESS
- Branch `feature/KNOCH-009-testimonial-section` created from dev
- Header counts updated: Open 15→14, In progress 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-008 PR opened — IN REVIEW

**Action:** PR #11 opened dev → test
**Tickets affected:** KNOCH-008
**Reason:** Builder agent completed implementation of pinned parallax + animated studio stats section
**Changes:**
- KNOCH-008: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-008-pinned-frame` created, implemented, merged into `dev`
- PR #11 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/11
- Files delivered:
  - `src/css/frame.css` — section layout, parallax bg, stat typography, grain overlay, mobile stack, reduced-motion overrides
  - `src/js/frame.js` — GSAP parallax tween, .big headline reveal, three animated counters with toLocaleString and once:true
  - `src/index.html` — complete #frame section markup with ARIA labels, frame.css linked in head
  - `src/js/main.js` — initFrame() import and call after initInterlude()
- Build: 25 modules, 13.33 kB CSS / 3.39 kB gz, 141.77 kB JS / 52.71 kB gz, 96ms
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-008 QA PASSED — merged to test

**Action:** PR #11 merged (dev → test) — KNOCH-008 QA PASSED
**Tickets affected:** KNOCH-008
**Reason:** Tester agent verified all 13 acceptance criteria and all additional checks; build clean at 98ms
**Changes:**
- KNOCH-008: Status changed IN REVIEW → QA PASSED
- PR #11 merged with full merge commit; branch `feature/KNOCH-008-pinned-frame` deleted
- All test plan checkboxes ticked in PR body
- Test report written: `docs/test-reports/KNOCH-008-test-report.md`
- Header counts updated: In review 1→0, Done 11→12
- Non-blocking note: headline/counter tweens not behind `prefers-reduced-motion` guard (parallax-only guard meets AC; full motion guard is KNOCH-021 scope)
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-007 + KNOCH-022/023/025 DONE — reel + CMS layer complete

**Action:** KNOCH-007 implemented and merged to test; KNOCH-022/023/025 completed in same session
**Tickets affected:** KNOCH-007, KNOCH-022, KNOCH-023, KNOCH-025, KNOCH-026 (deferred)
**Changes:**
- KNOCH-007: ⬜ → ✅ Done. Branch `feature/KNOCH-007-horizontal-reel` merged to test.
  - Horizontal pinned carousel, GSAP ScrollTrigger scrub:0.8, inner parallax via containerAnimation
  - Full cinematic greyscale filter (grayscale(1)) at rest; colour reveals on hover
  - Film-notch L-bracket corners, FRAME label, meta slide-up on hover
  - Mobile: CSS snap scroll replaces GSAP pin at ≤800px
  - Studio files from CMS branch cherry-picked onto this branch and committed
- KNOCH-022: 🔵 → ✅ Done. Sanity Studio running locally and deployed; all schemas live; 5 testimonials, 7 gallery collections, services entered.
- KNOCH-023: 🔵 → ✅ Done. `src/js/sanity.js` hardened with optional chaining, null guards, console logging for debug; `imageUrl()` helper; meta tags in index.html.
- KNOCH-025: ⬜ → ✅ Done (bundled with KNOCH-007). `main.js` fetches `getFeaturedCollections()` → maps to card shape → `initReel()`. Static CARDS fallback uses Sanity CDN URLs directly. `subtitle` field added to `galleryCollection` schema (Studio: fill in "Maryland · 2024" style labels per collection).
- KNOCH-026: ⬜ → ⏸ Deferred. Hero images are LCP-critical and design-tied — static files are correct. Sanity CDN adds latency with no benefit here.
- KNOCH-027: Added to CMS table (was missing). Open — blocked until KNOCH-013.
- Stat counts: In progress 2→0, Done 7→11, Open 19→16, Deferred 2→3
**Requested by:** Enoch / Builder agent

---

### 2026-05-05 — KNOCH-006 QA PASSED — re-test after fix

**Action:** Re-test by Tester Agent. All 15 ACs pass. PR #10 merged to test.
**Tickets affected:** KNOCH-006
**Reason:** Builder applied fix `fix(interlude): retarget word-splitter from blockquote to inner <p> (KNOCH-006)`. The blocker from the first QA run is resolved — `const para = quote.querySelector('p') ?? quote` now correctly targets the `<p>` inside the blockquote, and all `appendChild`/`querySelectorAll` calls operate on `para`. Word-split produces a non-empty NodeList; GSAP ScrollTrigger animates all words.
**Changes:**
- KNOCH-006: Status changed NEEDS FIXES → QA PASSED
- PR #10: Merged dev → test
- Test report: Result updated to PASSED; Re-test section appended
- Dashboard badge: Needs Fixes → QA Passed (green); PR #10 removed from Open PRs
- Stat counts: Needs fixes 1→0, Done 6→7
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-006 QA FAILED — NEEDS FIXES

**Action:** QA run by Tester Agent. 1 BLOCKER found. PR #10 blocked.
**Tickets affected:** KNOCH-006
**Reason:** Word-splitter JS targets `<blockquote>` childNodes instead of inner `<p>` childNodes — zero `.word` spans are created, the scroll reveal does not function.
**Changes:**
- KNOCH-006: Status changed IN REVIEW → NEEDS FIXES
- PR #10: Changes requested (comment posted — GitHub API prevented formal review on own PR)
- Dashboard badge: In Review → Needs Fixes
- Stat counts: In review 1→0, Needs fixes 0→1
**Blockers:**
1. **BLOCKER** — `src/js/interlude.js` line 37: `Array.from(quote.childNodes)` iterates the `<blockquote>` (whose only child is `<p>`). The `<p>` hits the `else { cloneNode(true) }` branch and is copied back unchanged. No `.word` spans are produced. Fix: use `quote.querySelector('p') ?? quote` as the split target.
**Non-blocking issues:**
- Redundant `w.style.opacity = '1'` in reduced-motion JS path (CSS already handles via `opacity: 1 !important`)
**Passing checks:** CSS layout/label/blockquote/signature/word/mobile/reduced-motion all correct. GSAP parameters correct. `interlude.css` linked in head. `initInterlude()` imported+called in main.js. `.grain` class applied. Token names correct. Build clean (20 modules, 88ms).
**Full report:** `docs/test-reports/KNOCH-006-test-report.md`
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-022 through KNOCH-026 created — CMS integration planned

**Action:** 5 new CMS tickets created; KNOCH-022 and KNOCH-023 implemented in session  
**Tickets affected:** KNOCH-022, KNOCH-023, KNOCH-024, KNOCH-025, KNOCH-026  
**Reason:** Adding Sanity CMS so content (testimonials, galleries, hero images) can be managed via Studio UI without code changes  
**Changes:**
- KNOCH-022: Created + 🔵 In progress — Sanity Studio scaffolded, all schemas deployed, 5 testimonials entered
- KNOCH-023: Created + 🔵 In progress — `src/js/sanity.js` built, Sanity meta tags added to `index.html`
- KNOCH-024: Created + ⬜ Open — blocked until KNOCH-009 is built
- KNOCH-025: Created + ⬜ Open — blocked until KNOCH-007 is built
- KNOCH-026: Created + ⬜ Open — can start now (KNOCH-005 ✅)
- New CMS phase section added to summary; dependency graph updated
- Header counts updated: Total 21→26, Open 13→17, In progress 0→2  
**Requested by:** Enoch

---

### 2026-05-05 — KNOCH-006 PR opened — IN REVIEW

**Action:** PR #10 opened dev → test
**Tickets affected:** KNOCH-006
**Reason:** Builder agent completed implementation; PR open for tester/code review
**Changes:**
- KNOCH-006: Status changed MERGED TO DEV → IN REVIEW
- PR #10 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/10
- Header counts updated: In progress 1→0, In review 0→1
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-006 implementation complete — MERGED TO DEV

**Action:** Implemented and merged to dev; PR to be opened dev → test
**Tickets affected:** KNOCH-006
**Reason:** Builder agent completed the interlude manifesto section with word-by-word scroll reveal
**Changes:**
- KNOCH-006: Status changed TODO → IN PROGRESS → MERGED TO DEV
- Branch `feature/KNOCH-006-interlude-manifesto` created, implemented, merged into `dev`
- Files delivered:
  - `src/css/interlude.css` — section layout, label, blockquote typography, .word spans, signature, mobile + reduced-motion overrides
  - `src/js/interlude.js` — childNodes word-splitter, GSAP ScrollTrigger desktop scrub, IntersectionObserver mobile fallback, prefers-reduced-motion guard
  - `src/index.html` — interlude section markup between hero and reel, interlude.css link in head
  - `src/js/main.js` — initInterlude() import and call after initHero()
- Build: 20 modules, 7.95kB CSS / 2.41kB gz, 136.66kB JS / 50.92kB gz, 87ms
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-005 QA PASSED

**Action:** QA run by Tester Agent. All 29 acceptance criteria pass.
**Tickets affected:** KNOCH-005
**Reason:** Hero section loader, reveal sequence, scroll exit, and asset verified against ticket spec and production bundle.
**Changes:**
- KNOCH-005: Status changed IN REVIEW → QA PASSED
- PR #6 merged: dev → test
- Dashboard badge updated: In Review → QA Passed (green)
- Stat counts updated: In review 1→0, Done 5→6, Open 15→14
**Passing checks:** All 29 ACs — #loader fixed/inset/z-index/ink bg, .loader-counter 18vw/Fraunces/300, GSAP proxy tween {val:0}→36/Math.ceil/padStart, amber label above counter (11px/0.3em/uppercase), .loader-progress 240px×1px/.loader-progress-fill amber, loader autoAlpha fade delay:2 + display:none onComplete, window.load trigger, body.loader-active .cursor{opacity:0}, #hero 100vh×100vw/overflow:hidden/flex center, .hero-bg absolute/inset0/cover/brightness(0.45)/grayscale(0.4)/contrast(1.15)/scale(1.1), reel-01.png background-image, preload link in head, .hero-content relative/z-index:2/text-center/padding:0 5vw, .hero-meta font-mono/10px/0.3em/amber/opacity:0, .line overflow:hidden/display:block, .line span translateY(110%), four headline lines with italic em, #hero-sub opacity:0/correct copy, reveal tl at t=0 scale1.1→1/2.4s/power3.out, meta opacity 0→1 at t=0, lineSpans y:0/1.2s/expo.out/stagger:0.12 at t=0.2, heroSub opacity at t=1.4, chrome opacity:0 pre-loader/faded in onComplete, initLenis not in main.js/called in onComplete, main.js imports+calls initHero, ScrollTrigger yPercent:25 on heroBg (trigger#hero/top-top/bottom-top/scrub), ScrollTrigger yPercent:-40+opacity:0 on heroContent, prefers-reduced-motion overrides, reel-01.png exists on disk, npm run build clean.
**LOW notes:** Two rgba(237,230,216,…) alpha values in hero.css not using named tokens (non-blocking); PR description references old hero-01.jpg filename (doc only, no code impact).
**Full report:** `docs/test-reports/KNOCH-005-test-report.md`
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-005 implementation complete — MERGED TO DEV

**Action:** Implemented and merged to dev; PR to be opened dev → test
**Tickets affected:** KNOCH-005
**Reason:** Builder agent completed the hero section with film-counter loader, reveal sequence, and scroll exit
**Changes:**
- KNOCH-005: Status changed TODO → IN REVIEW
- Branch `feature/KNOCH-005-hero-section` created, implemented, merged into `dev`
- Files delivered:
  - `src/css/hero.css` — loader overlay styles, hero layout, clip-reveal, vignette, reduced-motion overrides
  - `src/js/hero.js` — film-counter GSAP tween, progress bar, loader fade-out, reveal timeline, ScrollTrigger exits
  - `src/index.html` — loader HTML, full hero section markup, hero-01.jpg preload, hero.css link
  - `src/js/main.js` — removed direct initLenis() call; added initHero() import+call
  - `src/css/cursor.css` — added body.loader-active .cursor rule
  - `src/assets/hero/hero-01.jpg` — hero image copied from reference
- Build: 18 modules, 270kB hero image, 6.74 kB CSS / 2.19 kB gz, 135.48 kB JS / 50.56 kB gz, 84ms
- Header counts updated: Open 16→15
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-004 QA PASSED

**Action:** QA run by Tester Agent. All 8 acceptance criteria pass.
**Tickets affected:** KNOCH-004
**Reason:** Custom cursor and film-grain overlay verified against ticket spec and production bundle.
**Changes:**
- KNOCH-004: Status changed IN REVIEW → QA PASSED
- PR #5 merged: dev → test
- Dashboard badge updated: In Review → QA Passed (green)
- Stat counts updated: In review 1→0, Done 4→5
**Passing checks:** All 8 ACs — .cursor fixed properties (position/24px/amber border/border-radius/pointer-events/z-index/mix-blend-mode), 2px amber ::before dot, GSAP quickTo x+y (0.35s/power3.out) with GSAP auto-detecting CSS translate(-50%,-50%) for correct centering, .cursor.grow (80px/rgba amber fill/event delegation on a+button+.reel-card+.tile+.cta .button), mobile hidden (width<=800px + pointer:coarse), feTurbulence grain (baseFrequency:0.9/numOctaves:2/opacity:0.17/static data URI), no canvas/rAF in grain, cursor:none scoped to @media(pointer:fine) for progressive enhancement.
**Full report:** `docs/test-reports/KNOCH-004-test-report.md`
**Completed by:** Tester Agent

---

### 2026-05-05 — KNOCH-004 implementation complete — PR #5 open

**Action:** Implemented and PR opened dev → test
**Tickets affected:** KNOCH-004
**Reason:** Builder agent completed the custom cursor + film-grain overlay layer
**Changes:**
- KNOCH-004: Status changed TODO → IN PROGRESS → MERGED TO DEV → IN REVIEW
- Branch `feature/KNOCH-004-cursor-film-grain` created, implemented, merged into `dev`
- PR #5 opened: dev → test at https://github.com/eayanwale/knochmedia/pull/5
- Files delivered: `src/css/cursor.css` (cursor ring + grow state + .grain::after film-grain overlay), `src/js/cursor.js` (GSAP quickTo tracking + grow/shrink), `src/index.html` (cursor element + section stubs with .grain class), `src/js/main.js` (initCursor call)
- Build: 16 modules, 4.53 kB CSS / 1.67 kB gz, 133.78 kB JS / 50.23 kB gz, 79ms
**Requested by:** Builder agent

---

### 2026-05-05 — KNOCH-003 QA PASSED

**Action:** QA run by Tester Agent. All 10 acceptance criteria pass.
**Tickets affected:** KNOCH-003
**Reason:** Chrome navigation overlay verified against ticket spec, reference design, and production bundle.
**Changes:**
- KNOCH-003: Status changed IN REVIEW → QA PASSED
- PR #4 merged: dev → test
- Dashboard badge updated: In Review → QA Passed (green)
- Stat counts updated: In review 1→0, Done 3→4
**Passing checks:** All 10 ACs — .chrome fixed properties (position/inset/pointer-events/z-index/mix-blend-mode), 3-col grid, Fraunces wordmark with amber dot, 11px mono nav links with ease-cinematic underline-slide, setInterval timecode, bottom timecode bar with K/M·2026·MARYLAND, 800px breakpoint hides nav-center, focus-visible outlines, frame counter Math.ceil formula 1–36. LOW note: role="progressbar" inside aria-hidden parent (KNOCH-021 scope).
**Full report:** `docs/test-reports/KNOCH-003-test-report.md`
**Completed by:** Tester Agent

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
