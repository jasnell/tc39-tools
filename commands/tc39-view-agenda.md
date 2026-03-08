---
description: View the TC39 plenary meeting agenda
---

Fetch and display the TC39 plenary meeting agenda, showing all proposals, discussions, and schedule constraints.

## Step 0: Determine which meeting

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the meeting identifier in `YYYY/MM` format (e.g., `2026/03`).

If no argument is provided, fetch the next upcoming meeting.

## Step 1: Fetch the agenda

Use the `tc39_get_agenda` MCP tool to fetch the agenda for the meeting. If no meeting argument was given, call it without the `meeting` parameter to get the next upcoming meeting.

## Step 2: Present the agenda

Display the full agenda with all details. For every item, preserve and display all links (proposal repo, slides, spec PRs, test262 PRs, etc.) — these are critical for delegates reviewing the agenda.

### Meeting Info

Show the meeting title, dates, location, and host.

### Proposals

Present the full proposals table with all columns and links. For each proposal, show:
- **Stage** number
- **Timebox** in minutes
- **Proposal name** linked to the proposal repository
- **Advancement goal** (e.g., "for Stage 2", "update", "withdraw")
- **Supporting materials** — all links to slides, spec PRs, test262 PRs, etc.
- **Presenter** name

Sort order should match the agenda: stage descending, then timebox ascending.

### Short Discussions (≤30m)

Show the timebox, topic (with any links), and presenter for each.

### Longer / Open-Ended Discussions

Show the timebox, topic (with any links), and presenter for each.

### Schedule Constraints

List all constraints verbatim.

### Summary

At the end, include:
- Total number of proposals
- Total proposal discussion time (sum of all timeboxes)
- Number of proposals seeking each stage advancement (e.g., "3 proposals seeking Stage 2, 1 seeking Stage 4")
- Number of short and long discussions
- Number of schedule constraints
