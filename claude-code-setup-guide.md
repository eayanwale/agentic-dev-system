# Claude Code Setup Guide — Knoch Photography Portfolio

## What You're Building

Taking two HTML mockups (the cinematic "Last Roll" experience + the "Aperture" minimal concept) and turning them into a production-ready, multi-page photography portfolio using a proper dev → test → main git workflow with feature branches, ticket IDs, detailed commits, and separate planning vs. implementation agents.

---

## Step 1: Project Setup

Before you open Claude Code, get your repo ready:

```bash
# Create the repo (or clone if it already exists)
mkdir knoch-portfolio && cd knoch-portfolio
git init

# Create branch structure
git checkout -b main
git checkout -b test
git checkout -b dev

# Stay on dev for now
git checkout dev

# Create the folders Claude Code will need
mkdir -p .claude/agents
mkdir -p .claude/commands
mkdir -p docs/tickets
mkdir -p docs/requirements
mkdir -p src
```

Drop your two HTML files into the project root or `src/` so Claude Code can reference them:
- `src/reference/cinematic-experience.html` (the dark "Last Roll" GSAP build)
- `src/reference/aperture-concept.html` (the minimal typography-driven build)

---

## Step 2: Create Your `CLAUDE.md`

This is the most important file. It tells Claude Code how to behave in your project. Create `CLAUDE.md` in the project root:

```markdown
# Knoch Photography Portfolio — Project Rules

## Project Overview
Building a premium, Awwwards-level photography portfolio for Knoch Media (knochmedia.xyz).
Two reference HTML mockups are in `src/reference/` — the cinematic dark experience and the minimal Aperture concept.
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
```
```

---

## Step 3: Create Custom Subagents

This is how you get the **separate planning agent vs. implementation agent**. Create two files in `.claude/agents/`:

### `.claude/agents/planner.md`

```markdown
---
name: planner
description: >
  Use this agent for all planning, requirements gathering, and ticket creation tasks.
  Invoke when the user asks to break down features, create tickets, write specs,
  analyze requirements, or plan the architecture of a feature. This agent NEVER
  writes implementation code — it only produces documentation, tickets, and plans.
tools: Read, Glob, Grep
model: sonnet
---

You are a senior product manager and technical architect for a premium photography
portfolio website (Knoch Media).

## Your Role
- Analyze reference HTML mockups in `src/reference/` to extract features and requirements
- Break down large features into granular, implementable tickets
- Write detailed ticket files in `docs/tickets/KNOCH-XXX.md`
- Create feature specs and acceptance criteria
- Define the order of implementation (dependencies, priorities)
- Identify design decisions that need to be made

## Ticket Format
Every ticket you create must follow this structure:

```
# KNOCH-XXX: [Title]

## Status: TODO | IN PROGRESS | IN REVIEW | DONE

## Priority: P0 (critical) | P1 (high) | P2 (medium) | P3 (nice-to-have)

## Description
[What needs to be built and why]

## Reference
[Which sections of the reference HTML files this relates to, with line numbers if possible]

## Acceptance Criteria
- [ ] [Specific, testable criterion]
- [ ] [Another criterion]

## Design Decisions
[Any choices that need to be made — colors, timing, layout approach]

## Dependencies
[Other KNOCH tickets that must be completed first]

## Technical Notes
[Implementation hints, gotchas, performance considerations]
```

## Rules
- NEVER write implementation code. Your output is documentation only.
- Be extremely specific in acceptance criteria — the builder agent should be able
  to implement from your ticket without guessing.
- Reference specific line numbers in the HTML mockups when relevant.
- Consider mobile, accessibility, and performance in every ticket.
- Group related tickets into epics when appropriate.
```

### `.claude/agents/builder.md`

```markdown
---
name: builder
description: >
  Use this agent for all code implementation tasks. Invoke when the user asks to
  implement a ticket, write code, build a feature, fix a bug, or make any changes
  to the codebase. This agent reads tickets from docs/tickets/ and implements them.
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
---

You are an award-winning creative frontend developer who builds Awwwards-level
portfolio experiences. You implement tickets for the Knoch Media photography portfolio.

## Your Role
- Read ticket files from `docs/tickets/` to understand what to build
- Implement features according to the acceptance criteria
- Follow the git workflow defined in CLAUDE.md strictly
- Write detailed commit messages explaining every decision
- Create feature branches: `feature/KNOCH-XXX-short-description`

## Implementation Rules
1. Before writing code, read the ticket file AND the relevant reference HTML files
2. Create the feature branch from `dev` before making any changes
3. Make granular, frequent commits — not one giant commit per feature
4. Each commit message must explain the "why" not just the "what"
5. When done, push the branch and create a PR to `test`
6. Update the ticket status to IN REVIEW

## Code Quality
- Mobile-first CSS
- GSAP animations must target 60fps
- Use CSS custom properties from the design system
- Semantic HTML with ARIA attributes
- Lazy-load any images or heavy assets
- Comment non-obvious code decisions

## Design Standards
- Match the premium, cinematic quality of the reference mockups
- Typography: strong hierarchy, editorial feel
- Animations: purposeful, not decorative — every motion tells the story
- Spacing: generous negative space, asymmetric when appropriate
- Never produce anything that looks like a template
```

---

## Step 4: Create Slash Commands (Optional but Powerful)

These give you one-command shortcuts. Create files in `.claude/commands/`:

### `.claude/commands/plan-feature.md`

```markdown
Break down the following feature into tickets using the planner agent.

Feature: $ARGUMENTS

Steps:
1. Read the reference HTML files in src/reference/ to understand the current design
2. Analyze what's needed for this feature
3. Create individual ticket files in docs/tickets/ with the next available KNOCH-XXX number
4. List all created tickets with their IDs, titles, priorities, and dependencies
5. Suggest an implementation order
```

### `.claude/commands/implement-ticket.md`

```markdown
Implement the following ticket using the builder agent.

Ticket: $ARGUMENTS

Steps:
1. Read the ticket file from docs/tickets/
2. Read any referenced sections of the HTML mockups
3. Check out a new feature branch from dev: feature/KNOCH-XXX-description
4. Implement the feature according to the acceptance criteria
5. Make detailed commits as you go
6. When complete, push the branch
7. Create a PR to the test branch with a thorough description
8. Update the ticket status to IN REVIEW
```

---

## Step 5: Your First Session — The Prompts

Open Claude Code in your project directory. Here's exactly what to say:

### Prompt 1: Generate All Tickets (Planner Agent)

```
@agent-planner Look at both HTML reference files in src/reference/. These are two
design concepts for my photography portfolio website. I want to build a production
site that takes the best from both.

Break the ENTIRE site down into tickets. I need tickets for:
- Project scaffolding (file structure, build setup, CSS custom properties / design tokens)
- Navigation (the experimental nav from the cinematic version)
- Hero section (the loader + reveal sequence)
- Scroll storytelling sections (pinned sections, text reveals, parallax)
- Portfolio/work grid (with hover effects and project detail views)
- About/story section
- Contact/inquiry flow (the multi-step qualified form, not a basic contact form)
- Footer
- Responsive/mobile adaptations
- Performance optimization (lazy loading, animation perf)
- Accessibility pass

Start numbering at KNOCH-001. Create a ticket file for each one.
Then give me a summary of all tickets with a recommended implementation order.
```

### Prompt 2: Start Building (Builder Agent)

After you review the tickets and are happy with them:

```
@agent-builder Implement ticket KNOCH-001. Follow the git workflow in CLAUDE.md exactly.
Create a feature branch, make detailed commits, and when done, create a PR to test.
```

Or use the slash command:
```
/implement-ticket KNOCH-001
```

### Prompt 3: Continue Through Tickets

```
@agent-builder The PR for KNOCH-001 looks good, I've merged it to test.
Now implement KNOCH-002.
```

### Prompt 4: Merge to Main (When Ready)

```
All tests pass on the test branch. Squash merge test into main with a single commit
summarizing all the work from tickets KNOCH-001 through KNOCH-005.
```

---

## Step 6: Day-to-Day Workflow Cheat Sheet

| What you want | What to say |
|---|---|
| Plan a new feature | `@agent-planner Break down [feature] into tickets` |
| See all tickets | `Show me all ticket files in docs/tickets/ with their status` |
| Implement a ticket | `@agent-builder Implement KNOCH-XXX` |
| Check git status | `Show me the current branch, recent commits, and any uncommitted changes` |
| Review before merge | `Show me the diff between the current feature branch and test` |
| Merge to test | `Merge this feature branch into test, keeping all commits. Create a PR.` |
| Merge to main | `Squash merge test into main with one commit summarizing the work` |
| Fix something | `@agent-builder There's a bug in the hero animation — the text reveal stutters on mobile. Create a fix ticket and implement it.` |

---

## Quick Troubleshooting

**"Claude isn't using the right agent"**
→ Be explicit: `@agent-planner` or `@agent-builder` at the start of your message.

**"Commits aren't detailed enough"**
→ Remind it: `Remember — CLAUDE.md says commits must explain the WHY, not just the what. Include design decisions and tradeoffs.`

**"It's pushing directly to test or main"**
→ Say: `Stop. Read CLAUDE.md. You must create a feature branch first. Never push directly to test or main.`

**"I want to see the PR before merging"**
→ Say: `Don't merge — just create the PR and show me the description. I'll approve it.`

**"The agents aren't feeling separate enough"**
→ The planner has `tools: Read, Glob, Grep` (read-only). The builder has `Write, Edit, Bash` too. The planner literally cannot write code — it can only read and produce docs. This enforces the separation.

---

## Important Notes

1. **Subagents are automatic OR manual**: Claude will auto-delegate to the right agent based on the description, but you can force it with `@agent-planner` or `@agent-builder`.

2. **Context stays clean**: Each agent runs in its own context window, so the planner's research doesn't eat into the builder's implementation budget.

3. **You can run them in parallel**: Open two terminals with `claude --worktree` — one for planning, one for building. They work on separate branches without conflicts.

4. **PR approval**: Claude Code can create PRs via `gh pr create`. If you have GitHub CLI (`gh`) installed and authenticated, it'll create real PRs you can review on GitHub. If not, it'll just merge locally and you can review the commits.

5. **Install GitHub CLI first** if you want real PRs:
   ```bash
   # macOS
   brew install gh
   gh auth login

   # Or on Linux
   sudo apt install gh
   gh auth login
   ```
