# KNOCH-039 — Contact Form: Real Submission via Formspree

## Status: 🚀 SHIPPED — Phase 6 squash to main (v0.6.0)
## Priority: P0 (launch-blocker)
## Epic: EPIC-006 — Launch & SEO

## Title
Wire `/contact.html` and the homepage inquiry CTA to Formspree with a honeypot

## Description
The contact form (KNOCH-014) ships with a placeholder submit handler — pressing the button on the third step does nothing useful. **This is a launch-blocker**: the entire qualified-inquiry flow exists to deliver inquiries to the studio's inbox, and currently it doesn't.

Original scope had a Vercel serverless function + Resend + Cloudflare Turnstile. Re-scoped to Formspree per Enoch's preference for the lightest possible setup — submissions land at `hello@knoch.media` (the public-facing studio address; routes to the same Zoho inbox as `enoch@`) with zero infrastructure on our side. Honeypot stays as the spam guard; Turnstile dropped since Formspree's free tier already filters obvious spam and a honeypot stops 80% of bots on its own.

## Operator setup (Enoch — before implementation)
- [ ] Sign up at [formspree.io](https://formspree.io) (free tier = 50 submissions/month, plenty for this site)
- [ ] Create a new form, configure the destination email as `hello@knoch.media` (the public-facing studio address — Zoho aliases route both `hello@` and `enoch@` to the same inbox)
- [ ] In the form settings: enable autoresponder, set the autoresponder copy in-voice, set the Reply-To header to use the visitor's `email` field so a studio reply hits them directly
- [ ] Note the form's unique ID — looks like `xrgjabcd` — and hand it to me. The endpoint becomes `https://formspree.io/f/xrgjabcd`.

## Acceptance Criteria

**Wiring:**
- [ ] `<form>` `action` attribute on `/contact.html` updated from `/__contact-pending` to `https://formspree.io/f/<ID>`.
- [ ] `method="POST"` (already set), `accept="application/json"` added so Formspree returns JSON instead of redirecting (lets us keep the in-page success state).
- [ ] Same wiring applied to the homepage inquiry CTA's final-step submit (KNOCH-030 / KNOCH-014 — see `src/js/inquiry.js`).

**Honeypot:**
- [ ] Hidden `<input name="_gotcha">` (Formspree's reserved honeypot field name — they auto-discard any submission with it filled) styled `position:absolute; left:-9999px; opacity:0`, `tabindex="-1"`, `aria-hidden="true"`. Bots fill every field; humans never see it.
- [ ] Add to both contact form and homepage inquiry CTA.

**Subject + metadata:**
- [ ] Hidden `<input name="_subject" value="[Knoch · Inquiry] {type}">` so the studio inbox subject line carries the project type. Formspree interpolates `{field}` references against the submitted form data.
- [ ] Hidden `<input name="_format" value="plain">` if the default HTML email becomes annoying — defer until you see a real submission.

**Submit UX (already mostly built — just hook up the real fetch):**
- [ ] `contact-page.js` and `inquiry.js` already have a placeholder submit handler. Replace with a real `fetch(formspreeUrl, { method: 'POST', body: new FormData(form), headers: { 'Accept': 'application/json' } })`.
- [ ] Submit button shows loading state during POST (disabled + "SENDING…" label).
- [ ] On `{ ok: true }` from Formspree: advance to the existing step-3 confirmation panel with the summary list (already built in `contact-page.js`).
- [ ] On non-OK response: in-form error near the submit button — "Couldn't deliver right now. Try again, or email hello@knoch.media directly." Preserves all entered data.
- [ ] Network error / timeout (10 s) → same fallback message.

**Edge cases:**
- [ ] Honeypot field filled (bot) → Formspree silently 200s and discards. No client work needed.
- [ ] Above the 50/month limit → Formspree returns an error. Surface the same fallback message and email address.

## Design Notes

Formspree's "AJAX submission with `Accept: application/json` header" pattern keeps the visitor on the page — no Formspree-branded thank-you redirect. We keep our own step-3 confirmation panel, which the contact form already renders.

The `_gotcha` honeypot is Formspree's convention; using it (rather than a custom name) means Formspree's own server-side filtering catches it before counting against the monthly quota.

If/when we outgrow the free tier, three options: (1) upgrade Formspree to $10/mo, (2) self-host Forms via Formspree-OS, (3) migrate to a Vercel function + Resend at that point. None are urgent — 50 inquiries/month is well above expected volume for a premium portfolio.

## Tradeoffs Considered

- **Formspree vs. Vercel function + Resend**: chose Formspree for launch. Resend would mean writing a function, managing API keys, configuring DNS records for sending domain verification, and owning the email template. Formspree is one HTML attribute change. The lock-in concern (an old worry) is mitigated by Formspree's standard payload format — migrating to a function later is a half-day swap.
- **Turnstile dropped**: not worth the JS weight + setup overhead at this volume. Honeypot + Formspree's built-in filtering handles ~99% of bots. Add Turnstile later if real spam shows up.
- **Synchronous AJAX vs. native form submit + redirect**: AJAX. The form's three-step flow with the confirmation panel is the visible payoff for the visitor — losing it to a Formspree redirect would waste the form-design work.

## Related Tickets

- KNOCH-014 (contact form UX — already shipped, just needs the real submit wire)
- KNOCH-021 (a11y — confirmation panel announcement already wired via `aria-live`)
- KNOCH-037 (SEO — `/contact.html` already in sitemap)
