# KNOCH-014 — Contact / Inquiry Flow (Multi-Step Qualified Form)

## Status: SHIPPED — Phase 3 squash to main
## Priority: P1 (high)
## Epic: EPIC-003 — Secondary Pages
## Branch: feature/KNOCH-014-contact-form
## PR: #22 (dev → test)

## Title
Contact Page: Multi-Step Qualified Inquiry Form with Calendar Booking Option

## Description
A dedicated `contact.html` page with a 3-step qualified inquiry form. Not a basic contact form — step 1 qualifies the project type and budget, step 2 collects contact info and event details, step 3 is a confirmation. A right-panel sidebar offers a "skip the form" Calendly booking link. This is the page that converts visitors to clients.

## Acceptance Criteria
**Page layout:**
- [ ] Two-column grid: `5fr 4fr` (form left, sidebar right) — desktop
- [ ] Mobile: single column, form first, sidebar collapsed to a sticky bottom CTA

**Step 1 — Event Details:**
- [ ] Header: `"— Step 1 of 3 · 2 minutes"` amber label, `"Tell me about your day."` Fraunces headline
- [ ] Sub-copy: `"Every inquiry gets a personal reply within 24 hours — usually faster. No bots, no auto-responders."`
- [ ] **Service type selector**: 4 tiles — Wedding, Brand / Commercial, Sports / Event, Portrait / Family — clicking selects (amber border + subtle bg tint); keyboard-navigable
- [ ] **Date field**: styled date input or custom date picker (dark-themed, no default browser styling)
- [ ] **Location field**: text input, styled to match dark theme
- [ ] **Budget selector**: 4 tiers — $1–3k · $3–5k · $5–8k · $8k+ — same tile selector pattern
- [ ] **Note field**: `<textarea>` with ghost placeholder `"Outdoor ceremony, candid moments, want it to feel like a film not a slideshow…"` — min 3 rows
- [ ] Continue → button; disabled state until service type is selected

**Step 2 — Contact Info:**
- [ ] Fields: Name, Email, Phone (optional), "How did you find me?" (radio: Instagram, Google, Referral, Other)
- [ ] Conditional: if Wedding selected in step 1, show "Partner's name" field
- [ ] Back ← button

**Step 3 — Confirmation:**
- [ ] "Your inquiry is on its way." Fraunces headline
- [ ] Summary of submitted details
- [ ] Expected reply timeframe: "Within 24 hours — usually same day."
- [ ] Optional: "While you wait, explore the work →" link to portfolio

**Sidebar (right panel):**
- [ ] `"— Or skip the form"` amber label
- [ ] `"Book a 15-min call"` heading + short explanation copy
- [ ] `"Pick a time on my calendar →"` button (Calendly link — to be configured)
- [ ] `"What happens next"` numbered steps: 01 reply within 24hrs, 02 15-min call, 03 custom proposal in 48hrs
- [ ] Direct contact: phone `240.714.6933`, email `enoch@knochmedia.com`

**Form behavior:**
- [ ] Step transitions: current step slides out left (translateX -40px, opacity 0), next step slides in from right — GSAP tween, 0.5s
- [ ] Progress indicator: `"Step X of 3"` updates; amber dot indicator
- [ ] Form submits to a Netlify Forms endpoint (or Formspree) — static hosting compatible, no backend
- [ ] `?type=wedding` URL param pre-selects the service type on page load
- [ ] Client-side validation with inline error messages (styled in amber/rust) before each step proceeds

## Design Notes
The form should feel like a conversation, not a form. Labels use the voice from the reference: `"What are we capturing?"`, `"A short note — what's the feeling you're after?"`.

The scarcity signal appears as an amber banner at the top of the page: `"SIX DATES LEFT · 2026"` — pulls from the same `DATES_LEFT` constant as the homepage.

No CAPTCHA — the multi-step qualification flow inherently filters spam. Rate limiting can be handled at the Netlify/Formspree level.

## Tradeoffs Considered
- Multi-step vs. single-page form: The multi-step approach is a deliberate qualification gate. A client who completes 3 steps is a serious inquiry. Single-page would get more submissions but lower quality. Design intent: quality over quantity.
- Netlify Forms vs. custom backend: Netlify Forms works with no backend and is free for the volume a solo photographer gets. Custom backend adds complexity with zero benefit at this stage.
- Calendly link vs. custom scheduler: Calendly handles timezone, availability, and confirmations. Building a custom scheduler is out of scope.

## Related Tickets
- KNOCH-001 (contact.html as Vite entry point)
- KNOCH-003 (shared chrome nav)
- KNOCH-012 (project tiles link here with `?type=` param)
- KNOCH-021 (accessibility: form labels, error announcements, focus management between steps)
