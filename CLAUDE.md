# Knoch Photography Portfolio — Project Rules

## Target Product
This system operates on [knochmedia](https://github.com/eayanwale/knochmedia) — 
a photography portfolio site at knoch.media

Clone the product repo alongside this one:
    git clone https://github.com/eayanwale/knochmedia.git ../knochmedia

## Project Overview
Building a premium, Awwwards-level photography portfolio for Knoch Media (knoch.media).
Two reference HTML mockups are in `reference/` (repo root) — the cinematic dark experience and the minimal Aperture concept. The folder lives outside `src/` so Vite never touches it; it's reference material in git, not part of the build.
The final site should merge the best of both into a cohesive, story-driven portfolio.

## Tech Stack
- HTML / CSS / JavaScript (vanilla or with build tool if needed)
- GSAP + ScrollTrigger for animations
- Lenis or similar for smooth scrolling
- Google Fonts (Fraunces, Inter Tight, JetBrains Mono — or better alternatives)
- No frameworks unless I explicitly approve one (React, Astro, etc.)

## Git Workflow — STRICT

### Branches
- `main` — production. Only receives squash merges from `test`. One clean commit per feature.
- `test` — QA/staging. Receives merges from feature branches. Keep all individual commits (no squash).
- `dev` — active development. Feature branches are cut from here.
- Feature branches: `feature/KNOCH-XXX-short-description` (e.g., `feature/KNOCH-001-hero-section`)

### Commits
- Use conventional commits: `feat:`, `fix:`, `style:`, `refactor:`, `docs:`, `chore:`
- Every commit message MUST be detailed. Include:
  - What changed and why
  - Design decisions made
  - Any tradeoffs or alternatives considered
  - Example: `feat(hero): add parallax text reveal with staggered timing — chose 0.8s stagger over 0.4s because the slower pace matches the cinematic pacing of the reference. Film-grain overlay uses CSS noise filter rather than a texture image to keep the bundle light.`

### PR / Merge Rules
- Feature branch → `test`: Create a PR with a detailed description. Keep all commits. I will review if possible; if not, merge directly but leave the PR description thorough.
- `test` → `main`: Squash merge into a single commit. Only merge when I say so OR when all tests pass.
- Never push directly to `main` or `test`.

### Ticket System
- Generate ticket IDs in format: `KNOCH-XXX`
- Store ticket files in `docs/tickets/KNOCH-XXX.md`
- Each ticket file includes: title, description, acceptance criteria, design notes, related tickets
- Start numbering at KNOCH-001

## Code Style
- Mobile-first responsive design
- CSS custom properties for all colors, fonts, spacing
- Semantic HTML
- Accessibility: ARIA labels, keyboard navigation, focus states
- Performance: lazy-load images, optimize animations for 60fps
- Comments explaining non-obvious CSS/JS decisions

## Design Direction
- Ultra-modern, premium, cinematic
- Dark mode primary with light sections for contrast
- Strong typography hierarchy
- Story-driven scroll experience
- GSAP ScrollTrigger animations: pinned sections, text reveals, parallax
- At least one "wow" interaction per page section
- Contact: Qualified inquiry flow (not a basic form)

## File Structure
```
knoch-portfolio/
├── CLAUDE.md
├── .claude/
│   ├── agents/
│   │   ├── planner.md
│   │   └── builder.md
│   └── commands/
│       ├── plan-feature.md
│       └── implement-ticket.md
├── docs/
│   ├── tickets/          # Generated ticket files
│   └── requirements/     # PRDs and feature specs
├── src/
│   ├── reference/        # The two HTML mockups
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── assets/
└── dist/                 # Build output (if using a bundler)
