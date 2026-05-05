Implement the following ticket using the builder agent.

Ticket: $ARGUMENTS

## Auto-detect next ticket (when no ticket ID is provided)

If $ARGUMENTS is empty or blank, do NOT ask the user for a ticket ID. Instead:

1. Read `docs/TICKET-SUMMARY.md`
2. Walk the phases in order (Phase 1 → Phase 2 → Phase 3 → Phase 4+)
3. Within each phase, walk tickets top-to-bottom as they appear in the table
4. For each ticket, check:
   - Status is `⬜` (Open — not started)
   - Every ticket listed in its "Depends on" or "Notes" column (e.g. "Depends on KNOCH-001") has status `✅` or `🚀`
   - The ticket file `docs/tickets/KNOCH-XXX.md` also lists any explicit dependencies under a `## Dependencies` section — check those too
5. Select the **first** ticket that satisfies both conditions above
6. Announce: "No ticket ID provided — auto-selected **KNOCH-XXX: [Title]** as the next ready ticket." then proceed with that ticket as the target

If every open ticket has an unresolved dependency, stop and inform the user which tickets are blocked and what they depend on.

---

Steps:
1. Read the ticket file from docs/tickets/
2. Read any referenced sections of the HTML mockups
3. Check out a new feature branch from dev: feature/KNOCH-XXX-description
4. Update the ticket's `## Status:` field to `IN PROGRESS` and update **both**:
   - `docs/TICKET-SUMMARY.md` (status symbol → 🔵, fill in Branch column, add Changelog entry, update header counts)
   - `dashboard.html` (update the badge in the ticket's `<tr>`, fill in Branch `<td>`, update stat card counts)
5. Implement the feature according to the acceptance criteria
6. Make detailed commits as you go
7. When complete, merge the feature branch into dev (self-merge, no PR needed)
8. Update the ticket's `## Status:` to `MERGED TO DEV` in the ticket file AND in `docs/TICKET-SUMMARY.md`
9. When ready for QA, create a PR from dev → test with a thorough description
10. Update the ticket's `## Status:` to `IN REVIEW` in the ticket file AND in **both**:
    - `docs/TICKET-SUMMARY.md` (symbol → 🔵, Changelog entry, header counts)
    - `dashboard.html` (badge → In Review, add PR row to the Open PRs section, update stat counts)