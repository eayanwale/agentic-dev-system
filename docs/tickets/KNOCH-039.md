# KNOCH-039 — Contact Form: Real Submission + Anti-spam

## Status: TODO
## Priority: P0 (launch-blocker)
## Epic: EPIC-006 — Launch & SEO

## Title
Wire /contact.html to a real backend with honeypot + Turnstile

## Description
The contact form (KNOCH-014) ships with a placeholder submit handler — pressing the button on the third step does nothing useful. **This is a launch-blocker**: the entire qualified-inquiry flow exists to deliver inquiries to the studio's inbox, and currently it doesn't.

Two pieces:
1. **Submission backend** — pick a transport, wire it up.
2. **Anti-spam** — honeypot field (free, blocks 80% of bots) + Cloudflare Turnstile (free, privacy-friendly, no Google account required).

## Acceptance Criteria

**Backend (decision required):**
- [ ] Pick a transport — recommended default: **Vercel serverless function** at `api/inquire.js` posting to Resend or Postmark.
  - Pros: lives in this repo, full control over the email template, no third-party form vendor lock-in, Resend free tier covers expected volume (3000/mo).
  - Alternatives: Formspree (cheaper to set up, but lock-in + their branding on the email); Netlify Forms (we're not on Netlify, ruled out).
- [ ] Function reads form data, validates fields, posts a formatted HTML email to the studio inbox via the chosen provider's API.
- [ ] `RESEND_API_KEY` (or equivalent) added to Vercel env vars (production + preview separately).
- [ ] `STUDIO_INBOX` env var holds the destination email — keeps the address out of the repo.
- [ ] Return a JSON `{ ok: true }` on success, `{ ok: false, error: "..." }` on failure with appropriate HTTP status.

**Server-side validation:**
- [ ] Required fields present: `name`, `email`, `type`, `eventDate` (or `dateRange`), `details`.
- [ ] Email syntax check.
- [ ] `details` length ≤ 4000 chars (DoS guard).
- [ ] If `type === 'wedding'`, `partner` field accepted (optional, not required).
- [ ] Reject if Turnstile token is missing or fails verification.
- [ ] Reject if honeypot field is non-empty.

**Anti-spam stack:**
- [ ] **Honeypot**: hidden `<input name="website">` (or similar) styled `position:absolute; left:-9999px`, `tabindex="-1"`, `aria-hidden="true"`. Bots fill every field; humans never see it. Server rejects any submission where this field is non-empty.
- [ ] **Cloudflare Turnstile**: site key + secret key in Cloudflare dashboard; site key embedded in the form via the official `<script>` widget; secret key in Vercel env vars (`TURNSTILE_SECRET`). Server-side verification call to `https://challenges.cloudflare.com/turnstile/v0/siteverify`.
- [ ] Optional rate-limit (Upstash Redis, ~10 inquiries / hour / IP) — defer unless real spam shows up.

**Form UX:**
- [ ] Submit button shows loading state during POST (disabled + spinner / "SENDING…" label).
- [ ] On success: replace the form with a confirmation panel — "Roll received. We'll be in touch within 24 hours." + a link back to `/` and to `/portfolio.html`.
- [ ] On failure: in-form error message (paper @ 70%, mono, near submit button), preserves all entered data.
- [ ] Network error / timeout (10 s) → friendly fallback: "Couldn't deliver right now — email us directly at <inbox>". Inbox address surfaced via a build-time replacement OR fetched from `/api/contact-info` (lighter coupling).

**Email template:**
- [ ] Recipient: studio inbox.
- [ ] Subject: `[Knoch · Inquiry] <type> — <name>` (e.g. `[Knoch · Inquiry] Wedding — Alex Smith`).
- [ ] Reply-To: visitor's email so the studio can hit reply.
- [ ] Body sections: Name, Partner (if wedding), Email, Phone (if provided), Event date / range, Project type, Details, Submitted from (page URL + timestamp UTC).
- [ ] HTML formatted with the same paper / amber / serif aesthetic — visiting the inbox should still feel like Knoch.

**Telemetry (optional, defer):**
- [ ] Log submissions (no PII beyond email + name) to a Vercel KV / Upstash Redis for rate-limit + analytics.

## Design Notes

The Vercel serverless route keeps everything in one repo. Resend has the cleanest API (3 lines to send a formatted email) and a generous free tier. If Enoch already has Postmark / SendGrid / Mailgun set up elsewhere, swap easily.

Cloudflare Turnstile is the right captcha for this brand: invisible by default (most users see no challenge), Google-free, privacy-respecting. The form's premium aesthetic shouldn't be broken by a reCAPTCHA badge.

Honeypot + Turnstile is overkill for low spam volume but cheap; together they stop everything but a determined human spammer.

## Tradeoffs Considered

- **Vercel function vs. Formspree**: chose Vercel function. Lock-in is the issue — once a Formspree URL is in the form's `action` attribute and inquiries are landing somewhere, switching means losing in-flight messages OR running parallel for a migration period. Owning the function from day 1 sidesteps that.
- **Turnstile vs. reCAPTCHA v3**: chose Turnstile. reCAPTCHA shows the Google badge and feeds Google's tracking — both undesirable on a privacy-respecting site.
- **Synchronous email vs. queue**: synchronous is fine for current volume. Queue is overkill until the studio gets viral or hit by a bot wave.

## Related Tickets

- KNOCH-014 (contact form UX — already shipped, just needs the wire)
- KNOCH-021 (a11y — make sure error/success states are announced to screen readers)
- KNOCH-037 (SEO — sitemap should include /contact.html)
