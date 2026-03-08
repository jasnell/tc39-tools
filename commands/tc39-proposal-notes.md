---
description: Summarize the most recent TC39 plenary discussion for a proposal
subtask: true
---

Search TC39 plenary meeting notes for a given proposal and produce a detailed summary of its most recent discussion, including positions taken, concerns raised, and the outcome.

## Step 0: Determine the proposal

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the proposal name or search term (e.g., "TypedArray concat", "Temporal", "iterator helpers", "error codes").

If no argument is provided, check if the current directory is a proposal repo by looking for @spec.emu or @README.md. If found, infer the proposal name from the repo directory name (strip the `proposal-` prefix) or the README title. If neither exists, ask the user.

## Step 1: Search meeting notes

Use the `tc39_search_notes` MCP tool to search for the proposal name. Request up to 10 results so we can find the most recent discussion.

If no results are found, try alternate search terms:
- If the name contains hyphens, try with spaces instead
- Try shorter variations (e.g., "TypedArray concat" instead of "TypedArray.prototype.concat")
- Report if still no results found and suggest alternate terms

## Step 2: Identify the most recent discussion

From the search results, identify the **most recent meeting** where this proposal was discussed. There may be multiple sections from the same meeting (e.g., continuation sessions across days). Collect all sections from that most recent meeting.

If the proposal was discussed across multiple days within the same meeting, include all days.

## Step 3: Produce the summary

### Meeting Info

- **Meeting**: month and year (e.g., "February 2025")
- **Date(s)**: specific day(s) discussed
- **Presenter**: who presented
- **Stage at time of discussion**: if identifiable from the heading or body

### Presentation Summary

Summarize what the presenter covered:
- Key changes since last presentation (if mentioned)
- Design decisions being presented
- Open questions raised by the presenter
- What the presenter was asking for (stage advancement, feedback, consensus on a specific question)

### Committee Discussion

Summarize the discussion that followed. For each significant point:
- **Who** raised it (use full name, resolve TLA via `tc39_lookup_delegate` if needed)
- **What** they said or asked — their position, concern, question, or support
- **How it was addressed** — if the presenter or others responded

Group related exchanges together rather than listing them chronologically. Focus on:
- **Support signals**: Who expressed support and for what aspects
- **Concerns raised**: Technical objections, design questions, scope issues
- **Blocking issues**: Anything that prevented advancement
- **Action items**: Things the champion agreed to investigate or change

### Outcome

- **Conclusion**: The formal conclusion (consensus reached, stage advancement, not advanced, needs follow-up, etc.)
- **Next steps**: What the champion committed to doing before the next presentation

### Previous Discussions

After the detailed summary of the most recent meeting, include a brief chronological list of **all other meetings** where this proposal appeared in the search results:

| Date | Heading | Presenter | Outcome |
|------|---------|-----------|---------|
| ...  | ...     | ...       | ...     |

This gives context for how the proposal has progressed over time.
