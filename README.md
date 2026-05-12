# Agentic Dev System

A multi-agent development workflow built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code), designed to manage the full lifecycle of a software project — from planning and ticketing through implementation, QA, and release — using specialized AI agents with enforced separation of concerns.

Currently used to build [knochmedia](https://github.com/eayanwale/knochmedia), a photography portfolio at [knoch.media](https://knoch.media).

## What this is

Three Claude Code subagents that operate like a small engineering team:

**Planner** — A product manager that analyzes requirements, breaks features into tickets with detailed acceptance criteria, and defines implementation order. Read-only access to the codebase. Never writes code.

**Builder** — A frontend developer that reads tickets and implements them on feature branches, following a strict git workflow with detailed commit messages. Owns the `dev` branch.

**Tester** — A QA engineer that reviews PRs from `dev` → `test`, verifies every acceptance criterion, checks responsive behavior and accessibility, writes test reports, and either approves/merges or rejects with a fix list. Never edits code — only gates promotion.

Each agent has restricted tool access so they can't step outside their role. The planner can't write files. The tester can't edit code. The builder can't merge to `test`. This isn't just convention — the tool permissions enforce it.

## How it works

```
Planner creates tickets
        ↓
Builder implements on feature branch
        ↓
Builder merges feature branch → dev
        ↓
Builder opens PR: dev → test
        ↓
Tester reviews PR, writes test report
        ↓
    ┌───┴───┐
  PASS     FAIL
    │        │
    │    Builder fixes on dev,
    │    pushes to update PR
    │        │
    │    Tester re-tests
    │        ↓
Tester merges dev → test
        ↓
(repeat for all tickets in epic)
        ↓
All tickets QA PASSED → squash merge test → main
```

Nothing reaches `main` until every ticket in an epic passes QA. The `test` branch is the working preview — features accumulate there. `main` only receives complete epics as single squash commits.

## Repository structure

```
agentic-dev-system/
├── CLAUDE.md                    # Project rules, git workflow, design direction
├── claude-code-setup-guide.md   # Full setup walkthrough with example prompts
├── .claude/
│   ├── agents/
│   │   ├── planner.md           # Product manager agent
│   │   ├── builder.md           # Implementation agent
│   │   └── tester.md            # QA agent
│   └── commands/
│       ├── plan-feature.md      # /plan-feature slash command
│       ├── implement-ticket.md  # /implement-ticket slash command
│       └── test-ticket.md       # /test-ticket slash command
└── docs/
    ├── tickets/                 # KNOCH-001 through KNOCH-042+
    ├── requirements/            # PRDs and feature specs
    ├── test-reports/            # QA results per ticket
    └── TICKET-SUMMARY.md        # Changelog and status dashboard
```

## Setup

This system is designed to sit alongside the product repo it manages, not inside it.

```bash
# Clone both repos as siblings
git clone https://github.com/eayanwale/agentic-dev-system.git
git clone https://github.com/eayanwale/knochmedia.git

# Directory layout:
# ~/projects/
#   ├── agentic-dev-system/   ← agents, tickets, docs
#   └── knochmedia/           ← the actual website code
```

Prerequisites: [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed, and optionally the [GitHub CLI](https://cli.github.com/) (`gh`) for PR creation and review from the terminal.

Open Claude Code in the product repo directory. The agents reference the ticket and doc structure in this repo.

## Usage

Invoke agents explicitly with `@agent-<name>` or let Claude Code auto-delegate based on the task:

```
# Plan a feature — creates tickets with acceptance criteria
@agent-planner Break down the contact inquiry flow into tickets

# Implement a ticket — creates branch, writes code, opens PR
@agent-builder Implement KNOCH-015

# Test a ticket — reviews PR, writes test report, merges or rejects
@agent-tester Test KNOCH-015
```

Or use the slash commands:

```
/plan-feature contact inquiry multi-step form
/implement-ticket KNOCH-015
/test-ticket KNOCH-015
```

The setup guide (`claude-code-setup-guide.md`) has the full prompt library, day-to-day workflow cheat sheet, and troubleshooting.

## Ticket system

Tickets use the format `KNOCH-XXX` and are grouped into epics. Each ticket file includes status, priority, description, acceptance criteria, design decisions, dependencies, and technical notes.

`docs/TICKET-SUMMARY.md` serves as both a changelog and status dashboard — every status change, PR, merge, and release is logged as a dated entry.

As of the current state: 7 phases, 40+ tickets, 30+ shipped, site live at [knoch.media](https://knoch.media).

## Git workflow

The system enforces a four-branch model:

| Branch | Purpose | Who merges here |
|--------|---------|-----------------|
| `feature/*` | Individual ticket work | Builder creates from `dev` |
| `dev` | Integration branch | Builder self-merges features |
| `test` | QA / staging preview | Tester merges from `dev` via PR |
| `main` | Production | Manual squash merge of complete epics |

Commits follow [conventional commits](https://www.conventionalcommits.org/) with mandatory detail — not just "what changed" but why, what tradeoffs were considered, and what design decisions were made.

## Target product

[knochmedia](https://github.com/eayanwale/knochmedia) — an experimental photography portfolio built with GSAP, ScrollTrigger, and vanilla JS. The agents in this repo are configured for that project's tech stack, design direction, and file structure, but the pattern (planner/builder/tester with enforced separation, ticket-driven workflow, gated promotions) is reusable for other projects.

## Why this exists

This started as an experiment in using Claude Code's subagent system to manage a real project with real shipping deadlines. The key insight: giving each agent a narrow role with restricted permissions produces better results than a single agent doing everything. The planner writes better tickets when it can't jump ahead to code. The builder writes better code when it has a clear spec. The tester catches more issues when it can't quietly fix them.

The full story is in the [setup guide](claude-code-setup-guide.md).
