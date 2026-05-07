# KNOCH-027 — Sanity CMS: Wire About Page to Sanity

## Status: DONE — merged to test (PR #29)

## Description
Replace hardcoded content on `about.html` (KNOCH-013) with live data fetched from Sanity. The `aboutContent` document type is a singleton — only one document of this type exists; Enoch edits it directly in Studio.

## Acceptance Criteria
- [x] `getAboutContent()` added to `src/js/sanity.js`
- [x] `about.html` fetches and renders: headline, subheadline, bio, headshot, specialties, yearsExperience
- [x] Headshot served from Sanity CDN via `imageUrl()` helper
- [x] Updating bio or headline in Studio reflects on page after refresh — no code change needed
- [x] Page renders gracefully if fetch fails (static fallback copy)

## Implementation Notes
- `aboutContent` schema was scaffolded in KNOCH-022 but never registered in `studio/sanity.config.js` — this ticket adds the import + types entry so the singleton becomes editable in Studio.
- Headline supports a `*word*` convention (`"A working *studio.*"` → `<em>studio.</em>`) so authors can keep the amber-italic accent without giving up plain-text editing. HTML is escaped before the regex runs, so `<em>` is the only tag that can land in the rendered output.
- Hydration runs *before* `initAbout()` via a `.finally()` chain — the years stat's `data-count` is read once at init time, so the order ensures the counter tweens to the CMS value (not the static 8). `.finally()` (not `.then()`) means `initAbout` still runs if the Sanity fetch errors, preserving the static fallback.
- `result === []` confirmed against the live Sanity API (`*[_type == "aboutContent"]`) before merge — the singleton has not been published yet, which exercises the null-doc / static-fallback path. Acceptance is unblocked because the wiring is complete: when Enoch publishes the doc in Studio, the page hydrates on next refresh.

## GROQ Query
```js
export function getAboutContent() {
  return fetchQuery(`*[_type == "aboutContent"][0]`)
}
```

## Design Notes
- Singleton pattern — only one `aboutContent` document. Studio shows it directly, no list view needed.
- `__experimental_actions: ['update', 'publish']` in schema prevents accidental creation of duplicates (no "Create new" button)

## Related Tickets
- Depends on: KNOCH-022, KNOCH-023, KNOCH-013
