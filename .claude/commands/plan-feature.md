Break down the following feature into tickets using the planner agent.

Feature: $ARGUMENTS

Steps:
1. Read the reference HTML files in reference/ (repo root) to understand the current design
2. Analyze what's needed for this feature
3. Create individual ticket files in docs/tickets/ with the next available KNOCH-XXX number
4. Add `## Status: TODO` and `## Priority:` fields at the top of every new ticket file
5. Update `docs/TICKET-SUMMARY.md`:
   - Add a new row for each ticket in the correct Phase table with status `⬜`
   - Update the header counts (Total tickets, Open)
   - Add a Changelog entry at the bottom of the file listing all newly created tickets
6. List all created tickets with their IDs, titles, priorities, and dependencies
7. Suggest an implementation order
