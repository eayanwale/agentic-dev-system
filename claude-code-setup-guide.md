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
mkdir -p docs/test-reports
mkdir -p src/reference
mkdir -p src/design
```

Drop your files into the project so Claude Code can reference them:
- `src/reference/cinematic-experience.html` (the dark "Last Roll" GSAP build)
- `src/reference/aperture-concept.html` (the minimal typography-driven build)
- `src/design/` (your design PDF and any visual assets)

---

## Step 2: Create Your `CLAUDE.md`

This is the most important file. It tells Claude Code how to behave in your project. Create `CLAUDE.md` in the project root:

```markdown
# Knoch Photography Portfolio — Project Rules

## Project Overview
Building a premium, Awwwards-level photography portfolio for Knoch Media (knoch.media).
Two reference HTML mockups are in `src/reference/` — the cinematic dark experience and the minimal Aperture concept.
Design PDF and visual assets are in `src/design/`.
The final site should merge the best of both into a cohesive, story-driven portfolio.

## Tech Stack
- HTML / CSS / JavaScript (vanilla or with build tool if needed)
- GSAP + ScrollTrigger for animations
- Lenis or similar for smooth scrolling
- Google Fonts (Fraunces, Inter Tight, JetBrains Mono — or better alternatives)
- No frameworks unless I explicitly approve one (React, Astro, etc.)

## Git Workflow — STRICT

### Branches
- `main` — production. Only receives squash merges from `test` when a FULL EPIC is complete. Never for individual tickets.
- `test` — the MVP / working branch. This is the live QA environment. Only the tester agent can merge into this branch. Features accumulate here until an entire epic is done.
- `dev` — integration branch. The builder merges feature branches here after implementation is complete. This is where features combine before being promoted to test.
- Feature branches: `feature/KNOCH-XXX-short-description` (e.g., `feature/KNOCH-001-hero-section`) — cut from `dev`.

### Commits
- Use conventional commits: `feat:`, `fix:`, `style:`, `refactor:`, `docs:`, `chore:`
- Every commit message MUST be detailed. Include:
  - What changed and why
  - Design decisions made
  - Any tradeoffs or alternatives considered
  - Example: `feat(hero): add parallax text reveal with staggered timing — chose 0.8s stagger over 0.4s because the slower pace matches the cinematic pacing of the reference. Film-grain overlay uses CSS noise filter rather than a texture image to keep the bundle light.`

### PR / Merge Rules
- Feature branch → `dev`: Builder self-merges after implementation is done. Keep all commits (no squash). No PR required — builder owns this merge.
- `dev` → `test`: Builder creates a PR. Tester agent reviews, tests, and approves/merges. Keep all commits (no squash). If tester finds issues, tester reports back so builder can fix on `dev` (or a fix branch) and re-push the PR.
- `test` → `main`: Squash merge into a single commit. ONLY when I explicitly say so AND the entire epic is complete. Individual tickets never go to main alone.
- Never push directly to `main` or `test`.

### Epic Rules
- Tickets are grouped into epics (e.g., "Epic: Core Site Build", "Epic: Integrations")
- The `test` branch is the MVP — it accumulates all completed tickets for the current epic
- Only when ALL tickets in an epic are QA PASSED does the epic get promoted to `main`
- Epic promotion = squash merge `test` → `main` with one commit summarizing the entire epic

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
│   │   ├── builder.md
│   │   └── tester.md
│   └── commands/
│       ├── plan-feature.md
│       ├── implement-ticket.md
│       └── test-ticket.md
├── docs/
│   ├── tickets/          # Generated ticket files
│   ├── requirements/     # PRDs and feature specs
│   └── test-reports/     # Test results from tester agent
├── src/
│   ├── reference/        # The two HTML mockups
│   ├── design/           # Design PDFs and assets
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── assets/
└── dist/                 # Build output (if using a bundler)
```
```

---

## Step 3: Create Custom Subagents

This is how you get the **separate planning agent vs. implementation agent vs. testing agent**. Create three files in `.claude/agents/`:

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

## Epic
[Which epic this ticket belongs to — e.g., EPIC-001: Core Site Build]

## Technical Notes
[Implementation hints, gotchas, performance considerations]
```

## Rules
- NEVER write implementation code. Your output is documentation only.
- Be extremely specific in acceptance criteria — the builder agent should be able
  to implement from your ticket without guessing.
- Reference specific line numbers in the HTML mockups when relevant.
- Consider mobile, accessibility, and performance in every ticket.
- Group related tickets into epics. Each epic gets a file at `docs/tickets/EPIC-XXX.md` listing all child tickets. Nothing goes to `main` until an entire epic is complete.
- When creating tickets, assign each to an epic and note the epic in the ticket file.
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
5. When done, merge the feature branch into `dev` yourself (no PR needed, keep all commits)
6. Update the ticket status to MERGED TO DEV
7. When ready for QA, create a PR from `dev` → `test` with a thorough description
8. Update the ticket status to IN REVIEW
9. The tester agent will review the PR — if it comes back with NEEDS FIXES, read the test report in `docs/test-reports/`, fix the issues on `dev` (or a fix branch merged back to dev), commit with clear fix messages, and update the PR. Do NOT create a separate PR.

## Handling Tester Feedback
When a ticket is marked NEEDS FIXES:
1. Read the test report at `docs/test-reports/KNOCH-XXX-test-report.md`
2. Fix every issue listed under "Issues Found" — work on `dev` or a fix branch merged back to `dev`
3. Commit each fix with a message referencing the issue: `fix(KNOCH-XXX): [what was fixed] — addresses tester issue #N from test report`
4. Push `dev` so the existing PR to `test` updates automatically
5. Update ticket status back to IN REVIEW
6. Notify: "KNOCH-XXX fixes pushed to dev. Ready for re-test."

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

### `.claude/agents/tester.md`

```markdown
---
name: tester
description: >
  Use this agent for all QA, testing, and validation tasks. Invoke when the user
  asks to test a feature, verify acceptance criteria, check responsive behavior,
  audit accessibility, validate animations, or review code quality. This agent
  gates the dev → test promotion: it reviews PRs from dev to test, and can
  approve and merge them or reject with feedback. It reads ticket acceptance
  criteria and verifies the implementation meets them. When issues are found,
  it reports them back so the builder can fix on dev and re-push. It does NOT
  fix issues itself — it only tests, reports, approves, or rejects.
tools: Read, Bash, Glob, Grep
model: sonnet
---

You are a meticulous QA engineer and frontend quality specialist for a premium
photography portfolio website (Knoch Media). You verify that implementations
match their ticket specifications exactly.

## Your Role
- Read the ticket file from `docs/tickets/` to understand acceptance criteria
- Review the PR from `dev` → `test` (the builder creates this PR when features are ready for QA)
- Verify every acceptance criterion is met
- Test responsive behavior across breakpoints (mobile 375px, tablet 768px, desktop 1280px+)
- Validate GSAP animations run at 60fps (no jank, no layout thrashing)
- Check accessibility (semantic HTML, ARIA labels, keyboard nav, focus states, color contrast)
- Verify cross-browser considerations (CSS fallbacks, vendor prefixes where needed)
- Test performance (image sizes, lazy loading, render-blocking resources)
- Write a test report in `docs/test-reports/KNOCH-XXX-test-report.md`
- **Approve and merge PRs** from `dev` into `test` when all criteria pass
- **Reject and report** when criteria fail — update ticket and provide clear instructions for the builder to fix on `dev`

## PR Approval Workflow
1. Read the PR diff (`dev` → `test`) and the associated ticket(s)
2. Run all test checks (see Test Report Format below)
3. If ALL checks pass:
   - Write test report with verdict PASS
   - Approve the PR (`gh pr review --approve`)
   - Merge the PR into `test` (`gh pr merge --merge --no-squash`)
   - Update ticket status to `QA PASSED`
   - Inform: "KNOCH-XXX passed QA and has been merged to test."
4. If ANY check fails:
   - Write test report with verdict FAIL and detailed issue list
   - Request changes on the PR (`gh pr review --request-changes`)
   - Update ticket status to `NEEDS FIXES`
   - Inform: "KNOCH-XXX failed QA. Builder: fix the following issues on dev and push so the PR updates: [list issues]"
   - The builder should then fix on `dev`, commit, push, and the tester re-tests the same PR

## Test Report Format
Every test report must follow this structure:

```
# Test Report: KNOCH-XXX — [Title]

## Date: [YYYY-MM-DD]
## Branch: [branch name]
## Tested By: tester-agent
## PR: [PR number or link]

## Acceptance Criteria Results
- [x] Criterion 1 — PASS
- [ ] Criterion 2 — FAIL: [specific description of what's wrong]

## Responsive Testing
| Breakpoint | Status | Notes |
|---|---|---|
| 375px (mobile) | PASS/FAIL | [details] |
| 768px (tablet) | PASS/FAIL | [details] |
| 1280px (desktop) | PASS/FAIL | [details] |

## Performance
- Animation FPS: [result]
- Image optimization: [result]
- Render-blocking resources: [result]

## Accessibility
- Semantic HTML: PASS/FAIL
- Keyboard navigation: PASS/FAIL
- ARIA labels: PASS/FAIL
- Color contrast: PASS/FAIL

## Issues Found
1. [SEVERITY: HIGH/MEDIUM/LOW] — [Description + where it occurs + expected vs actual]
2. ...

## Verdict: PASS / FAIL / PASS WITH NOTES
[Summary — approved and merged, or sent back to builder with fix list]
```

## Rules
- NEVER fix code yourself. You only identify and report issues.
- If a feature fails, update the ticket status to `NEEDS FIXES`, request changes on the PR, and clearly list what the builder needs to fix so they can act without guessing.
- Be specific: include file paths, line numbers, exact breakpoints where things break.
- Test the actual rendered output whenever possible (use Bash to run a local server, check HTML validity, lint CSS/JS).
- When everything passes, approve the PR, merge `dev` → `test`, and update the ticket status to `QA PASSED`.
- For animation testing, check for forced reflows, use of `will-change`, and whether transforms are used instead of layout-triggering properties (top/left/width/height).
- You ONLY gate `dev` → `test`. You NEVER merge anything to `main`. The `test` → `main` promotion is a manual decision by the user when a full epic is complete.
- You NEVER touch `dev` directly. If fixes are needed, the builder handles them on `dev` and pushes.
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
6. When complete, merge the feature branch into dev (self-merge, no PR needed)
7. Update the ticket status to MERGED TO DEV
8. When ready for QA, create a PR from dev → test with a thorough description
9. Update the ticket status to IN REVIEW
```

### `.claude/commands/test-ticket.md`

```markdown
Test the following ticket using the tester agent.

Ticket: $ARGUMENTS

Steps:
1. Read the ticket file from docs/tickets/ to get the acceptance criteria
2. Find the open PR from dev → test for this ticket
3. Read the PR diff and all code changes
4. Run validation checks (HTML validity, CSS lint, JS lint, accessibility)
5. Test responsive behavior at 375px, 768px, and 1280px+ breakpoints
6. Verify animation performance (no layout thrashing, transforms over positioning)
7. Write a test report to docs/test-reports/KNOCH-XXX-test-report.md
8. If ALL checks pass: approve the PR, merge dev → test (no squash), update ticket to QA PASSED
9. If ANY check fails: request changes on the PR, update ticket to NEEDS FIXES, list issues clearly so the builder can fix on dev and re-push
```

---

## Step 5: Your First Session — The Prompts

Open Claude Code in your project directory. Here's exactly what to say:

### Prompt 1: Generate All Tickets (Planner Agent)

```
@agent-planner Look at HTML reference files in src/reference/. These are two
design concepts for my photography portfolio website. I want to build a production
site that takes the best from all. I also want you to take a look at
src/design for the design pdf there.

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
- Integrations that are in the current website (https://knochmedia.xyz)
  - youtube integrations
  - instagram embeds
  - etc.
- Performance optimization (lazy loading, animation perf)
- Accessibility pass

Start numbering at KNOCH-001. Create a ticket file for each one.
Then give me a summary of all tickets with a recommended implementation order.
```

### Prompt 2: Start Building (Builder Agent)

After you review the tickets and are happy with them:

```
@agent-builder Implement ticket KNOCH-001. Follow the git workflow in CLAUDE.md exactly.
Create a feature branch from dev, make detailed commits, merge into dev when done,
then create a PR from dev → test for QA.
```

Or use the slash command:
```
/implement-ticket KNOCH-001
```

### Prompt 3: Test a Feature (Tester Agent)

After builder has merged to dev and opened a PR from dev → test:

```
@agent-tester Test ticket KNOCH-001. Review the PR from dev → test, verify all
acceptance criteria, check responsive behavior, validate accessibility, and check
animation performance. Write a test report. Approve and merge if it passes, or
reject with a fix list for the builder.
```

Or use the slash command:
```
/test-ticket KNOCH-001
```

### Prompt 4: Continue Through Tickets

After tester approves and merges dev → test:

```
@agent-builder KNOCH-001 passed QA and was merged to test by the tester.
Now implement KNOCH-002. Create a feature branch from dev, build it, merge to dev,
and open a PR to test when ready.
```

If tester rejected:

```
@agent-builder KNOCH-001 failed QA. Read the test report at docs/test-reports/KNOCH-001-test-report.md,
fix the issues on dev, and push so the existing PR to test updates.
```

Then re-test:

```
@agent-tester Re-test KNOCH-001. The builder has pushed fixes to dev.
Check if all issues from the previous test report are resolved on the open PR.
```

### Prompt 5: Merge to Main (Epic Complete)

Only when ALL tickets in an epic are QA PASSED on the `test` branch:

```
All tickets in EPIC-001 (Core Site Build) are QA PASSED on the test branch.
Squash merge test into main with a single commit summarizing the entire epic:
what was built, key design decisions, and the ticket range covered.
```

---

## Step 6: Day-to-Day Workflow Cheat Sheet

| What you want | What to say |
|---|---|
| Plan a new feature | `@agent-planner Break down [feature] into tickets and assign to an epic` |
| See all tickets | `Show me all ticket files in docs/tickets/ with their status` |
| See epic progress | `Show me the epic file and which tickets are QA PASSED vs remaining` |
| Implement a ticket | `@agent-builder Implement KNOCH-XXX` |
| Test a ticket | `@agent-tester Test KNOCH-XXX — review the dev → test PR, run checks, approve or reject` |
| See test reports | `Show me test reports in docs/test-reports/` |
| Fix after rejection | `@agent-builder KNOCH-XXX failed QA. Read the test report and fix on dev.` |
| Re-test after fix | `@agent-tester Re-test KNOCH-XXX. Builder pushed fixes to dev.` |
| Check git status | `Show me the current branch, recent commits, and any uncommitted changes` |
| Review before QA | `Show me the diff between dev and test` |
| Merge to main (epic) | `All tickets in EPIC-XXX are QA PASSED. Squash merge test into main.` |
| Fix something ad-hoc | `@agent-builder There's a bug in the hero animation — create a fix ticket and implement it on dev.` |

---

## Quick Troubleshooting

**"Claude isn't using the right agent"**
→ Be explicit: `@agent-planner`, `@agent-builder`, or `@agent-tester` at the start of your message.

**"Commits aren't detailed enough"**
→ Remind it: `Remember — CLAUDE.md says commits must explain the WHY, not just the what. Include design decisions and tradeoffs.`

**"It's pushing directly to test or main"**
→ Say: `Stop. Read CLAUDE.md. Builder merges to dev only. Test only receives PRs approved by tester. Never push directly to test or main.`

**"I want to see the PR before tester merges"**
→ Say: `Don't merge — tester, just write the test report and leave the PR open. I'll review it first.`

**"The agents aren't feeling separate enough"**
→ The planner has `tools: Read, Glob, Grep` (read-only, produces docs only). The tester has `tools: Read, Bash, Glob, Grep` (can read and run checks but cannot edit code). The builder has `Write, Edit, Bash` too. The planner can't write code, the tester can't edit files, and only the tester can approve PRs into `test`. This enforces the separation.

---

## Important Notes

1. **Subagents are automatic OR manual**: Claude will auto-delegate to the right agent based on the description, but you can force it with `@agent-planner`, `@agent-builder`, or `@agent-tester`.

2. **Context stays clean**: Each agent runs in its own context window, so the planner's research doesn't eat into the builder's implementation budget.

3. **You can run them in parallel**: Open two terminals with `claude --worktree` — one for planning, one for building. They work on separate branches without conflicts.

4. **The full lifecycle for each ticket**:
   - Planner creates ticket → Builder implements on feature branch → Builder self-merges to `dev` → Builder opens PR from `dev` → `test` → Tester reviews PR → Tester approves & merges to `test` (or rejects → Builder fixes on `dev` → re-test) → Repeat until all tickets in the epic are QA PASSED → You say "merge to main" → Squash merge the entire epic.

5. **`test` is your MVP**: The `test` branch is your working, viewable site. You can preview everything there as it gets built. Nothing goes to `main` until a full epic is done and you explicitly approve it.

6. **`dev` is the builder's workspace**: The builder owns `dev` — they merge feature branches in freely without needing approval. This is where features integrate. The tester only gets involved when the builder opens a PR from `dev` → `test`.

7. **Tester owns the `dev` → `test` gate**: The tester agent approves and merges PRs from `dev` into `test`. You don't have to manually merge — the tester handles that. You only need to give the final "merge to main" command when an epic is complete.

7. **PR approval**: Claude Code uses `gh pr create`, `gh pr review`, and `gh pr merge`. Make sure GitHub CLI is installed and authenticated:
   ```bash
   # macOS
   brew install gh
   gh auth login

   # Or on Linux
   sudo apt install gh
   gh auth login
   ```

8. **If `gh` isn't available**: The agents will fall back to local git merges. The workflow still works — you just won't get GitHub PR review UI. The test reports in `docs/test-reports/` still serve as the QA record.
