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
7. Write or update the test report at docs/test-reports/KNOCH-XXX-test-report.md:
   - First run: create the full report
   - Re-test (ticket is NEEDS FIXES): update the top-level Result field in the header table to the new outcome (e.g. `**PASSED** *(re-test YYYY-MM-DD)*`), then append a `## Re-test — YYYY-MM-DD` section at the bottom
8. If ALL checks pass:
   - Approve the PR, merge dev → test (no squash)
   - Update `## Status:` in the ticket file to `QA PASSED`
   - Update `docs/TICKET-SUMMARY.md`: status symbol → ✅, add Changelog entry, update header counts
   - Update `dashboard.html`: badge → QA Passed, remove the PR's row from the Open PRs section, update stat counts
9. If ANY check fails:
   - Request changes on the PR
   - Update `## Status:` in the ticket file to `NEEDS FIXES`
   - Update `docs/TICKET-SUMMARY.md`: status symbol → 🔁, add Changelog entry listing each issue, update header counts
   - Update `dashboard.html`: badge → Needs Fixes, update stat counts
   - List issues clearly in the Changelog entry so the builder can fix on dev and re-push