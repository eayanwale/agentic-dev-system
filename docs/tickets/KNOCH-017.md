# KNOCH-017 — YouTube Integration

## Status: TODO
## Priority: P2 (medium)
## Epic: EPIC-004 — Integrations

## Title
YouTube Integration: Wedding Film Reel Embeds and Lightbox Video Player

## Description
Knoch Media's current site (knochmedia.xyz) features YouTube videos — wedding film trailers and a showreel. The new site needs two YouTube integration patterns: (1) a reel/trailer lightbox triggered from portfolio tiles and the reel cards, and (2) an embedded autoplay showreel in the hero or a dedicated "films" section. Both must be performance-conscious — YouTube iframes are not loaded until the user signals intent (click).

## Acceptance Criteria
**Lightbox modal (for individual film tiles):**
- [ ] `.video-modal` element: `position: fixed; inset: 0; z-index: 5000; background: rgba(0,0,0,0.92); display: none; align-items: center; justify-content: center`
- [ ] Inner container: `width: min(90vw, 1280px); aspect-ratio: 16/9`
- [ ] YouTube `<iframe>` injected dynamically on modal open with `?autoplay=1&rel=0&modestbranding=1&color=white` params — removed on close to stop video and prevent background audio
- [ ] Modal open: GSAP `autoAlpha: 0 → 1, duration: 0.4` + `scale: 0.95 → 1`
- [ ] Modal close: click outside, press Escape, or close button (×)
- [ ] Escape key listener registered on modal open, removed on modal close
- [ ] Focus trap within modal while open (KNOCH-021)
- [ ] Triggered from: tile `.t7` (The Hartleys trailer), reel card `FRAME 06` (UMD Athletics if it has film)
- [ ] Each trigger element has `data-youtube-id="XXXXXXXXXXX"` attribute

**Showreel section (hero or films section):**
- [ ] A "Play showreel" button in the hero sub-text or CTA section
- [ ] Clicking opens the same lightbox modal with the main channel reel video ID
- [ ] The button has a play icon (inline SVG) and `"PLAY REEL"` text in mono uppercase
- [ ] YouTube thumbnail preview shown as background image on the play button (fetched via `img.youtube.com/vi/{id}/maxresdefault.jpg`)

**Performance:**
- [ ] NO YouTube iframe loaded on page load — only injected on click
- [ ] YouTube thumbnail images use the native YouTube thumbnail URL (no additional hosted images)
- [ ] `loading="lazy"` on thumbnail `<img>` elements

## Design Notes
The Cinema Vérité reference (`concept_02_cinema_verite demo.html`) shows a video player UI with `"▶ PLAY REEL"` button, timecode overlay `TC 00:01:42:11`, and `"▢ 16:9 · 24FPS"` aspect ratio label. These cinematic chrome details should appear on the "Play showreel" button area, not inside the actual iframe (the iframe is standard YouTube).

YouTube channel: knochmedia.xyz links to YouTube but the specific channel URL/video IDs will need to be filled in during implementation. Use `data-youtube-id` placeholder `"PLACEHOLDER"` until provided.

The `color=white` parameter makes the YouTube progress bar white instead of red — closer to the amber/paper design language.

## Tradeoffs Considered
- Lazy iframe injection vs. iframe with srcdoc poster: Injecting the iframe on click is the cleanest approach — no YouTube JS API needed, no consent issues with preloaded iframes. The tradeoff is a ~0.5s load delay on click (acceptable and expected by users who click "play").
- YouTube vs. Vimeo: YouTube is what the current site uses. Vimeo Pro would give more styling control and no ads, but it's an additional cost and requires migrating existing films. Stay with YouTube.

## Related Tickets
- KNOCH-010, KNOCH-012 (tiles that trigger the lightbox)
- KNOCH-005 (showreel play button may appear in hero)
- KNOCH-021 (focus trap in video modal)
