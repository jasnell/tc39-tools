---
description: Look up a TC39 delegate and summarize their recent meeting activity
subtask: true
---

Look up a TC39 delegate by name, TLA, or partial match and summarize their recent presentations, positions, and discussion contributions from plenary meeting notes.

## Step 0: Determine the delegate

The argument is: `$ARGUMENTS`

If an argument is provided, use it as the delegate search term (e.g., a full name like "Kevin Gibbons", a TLA like "KG", or a partial match like "gibbons").

If no argument is provided, ask the user which delegate they want to look up.

## Step 1: Resolve the delegate

Use the `tc39_lookup_delegate` MCP tool to resolve the input to a full name and TLA.

- If **no match** is found, report this and suggest the user try a different spelling or TLA.
- If **multiple matches** are found, list them and ask the user to pick one.
- If **exactly one match** is found, proceed with that delegate.

Record the resolved **full name** and **TLA** for use in the search steps.

## Step 2: Search for presentations

Use the `tc39_search_notes` MCP tool to search for the delegate's **full name**. This finds sections where they are listed as the Presenter.

Collect all matching sections.

## Step 3: Search for discussion contributions

Use the `tc39_search_notes` MCP tool to search for the delegate's **TLA** (e.g., "KG"). This finds sections where they participated in discussion (meeting transcripts use TLAs like "KG: I think...").

Collect all matching sections. De-duplicate any that overlap with Step 2 results (same meeting + same heading = same section).

## Step 4: Organize and present results

Organize the combined results into a summary grouped by proposal or topic.

### Delegate Profile

Show the delegate's full name and TLA at the top.

### Presentations

List proposals/topics where this delegate was the presenter, sorted by date (most recent first):

| Date | Proposal/Topic | Outcome |
|------|----------------|---------|
| ...  | ...            | ...     |

For each, include the conclusion if available.

### Discussion Contributions

List proposals/topics where this delegate participated in discussion (but was not the presenter), sorted by date (most recent first). For each, summarize:

- The proposal/topic being discussed
- The delegate's apparent position or key points (extracted from the excerpt where their TLA appears)
- The meeting date

### Summary

At the end, provide a brief summary:
- Total presentations found
- Total discussions participated in
- Key areas of focus (recurring topics/proposals)
- Any notable positions or patterns (e.g., consistently advocates for X, raised concerns about Y)
