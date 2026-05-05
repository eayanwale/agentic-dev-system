Test the following ticket using the tester agent.

Ticket: $ARGUMENTS

## Auto-detect next ticket to test (when no ticket ID is provided)

If $ARGUMENTS is empty or blank, do NOT ask the user for a ticket ID. Instead:

1. Read `docs/TICKET-SUMMARY.md`
2. Look for tickets in this priority order:
   a. Status `🔁` (Needs Revision / NEEDS FIXES) — these are highest priority; fixes have been pushed and a re-test is due
   b. Status `🔵` (In Progress / IN REVIEW) — builder has opened a PR and it is awaiting QA
3. Select the **first** matching ticket (top-to-bottom order within each priority group)
4. Verify there is an open PR from `dev` → `test` for this ticket by running `gh pr list --base test --state open`
5. Announce: "No ticket ID provided — auto-selected **KNOCH-XXX: [Title]** for testing (status: [current status])." then proceed

If no tickets are in a testable state (IN REVIEW or NEEDS FIXES with a live PR), stop and inform the user that there is nothing currently queued for QA.

---

Steps:
1. Read the ticket file from docs/tickets/ to get the acceptance criteria
2. Find the open PR from dev → test for this ticket
3. Read the PR diff and all code changes
4. Run validation checks (HTML validity, CSS lint, JS lint, accessibility)
5. Test responsive behavior at 375px, 768px, and 1280px+ breakpoints
6. Verify animation performance (no layout thrashing, transforms over positioning)
7. Write a test report to docs/test-reports/KNOCH-XXX-test-report.md
7. Write or update the test report at docs/test-reports/KNOCH-XXX-test-report.md:
   - First run: create the full report
   - Re-test (ticket is NEEDS FIXES): update the top-level Result field in the header table to the new outcome (e.g. `**PASSED** *(re-test YYYY-MM-DD)*`), then append a `## Re-test — YYYY-MM-DD` section at the bottom
8. If ALL checks pass:
   - Approve the PR, merge dev → test (no squash)
   - Tick off all `- [ ]` items in the PR Test plan section → `- [x]` using `gh pr edit <number> --body "<updated body>"`
   - Update `## Status:` in the ticket file to `QA PASSED`
   - Update `docs/TICKET-SUMMARY.md`: status symbol → ✅, add Changelog entry, update header counts
   - Update `dashboard.html`: badge → QA Passed, remove the PR's row from the Open PRs section, update stat counts
9. If ANY check fails:
   - Request changes on the PR
   - Tick off only passing `- [ ]` items in the PR Test plan; leave failing items unchecked using `gh pr edit <number> --body "<updated body>"`
   - Update `## Status:` in the ticket file to `NEEDS FIXES`
   - Update `docs/TICKET-SUMMARY.md`: status symbol → 🔁, add Changelog entry listing each issue, update header counts
   - Update `dashboard.html`: badge → Needs Fixes, update stat counts
   - List issues clearly in the Changelog entry so the builder can fix on dev and re-push