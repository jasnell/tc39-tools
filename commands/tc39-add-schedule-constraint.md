---
description: Add a schedule constraint to the TC39 meeting agenda via PR
---

Add a schedule constraint to the next TC39 plenary meeting agenda by creating a pull request to the `tc39/agendas` repository.

## Step 0: Gather context

Detect the GitHub user:
!`gh auth status 2>&1`

Parse the authenticated GitHub username and display name.

## Step 1: Fetch the current agenda

Use the `tc39_get_agenda` MCP tool to fetch the next upcoming meeting agenda.

Display the meeting dates and any existing schedule constraints so the user can see what's already been submitted.

## Step 2: Get the constraint details

The argument is: `$ARGUMENTS`

If arguments are provided, use them as the constraint text.

If no arguments are provided, ask the user for:
- Their **full name** (and abbreviation if they have one)
- **Availability**: which days/times they cannot attend, or which days/times they prefer
- **Sessions of interest**: which specific proposals or topics they need to be present for
- Whether this is a **hard constraint** or a **preference**

Format the constraint as a single bullet point following the established convention in the agenda. Examples of the format:

```
- Philip Chimento: Temporal not on Tuesday. Preferably on Wednesday.
- Jesse Alama (JMN): Cannot attend first half of March 11th, and not at all on March 12th.
- Nicolò Ribaudo: Cannot attend the first two hours on Thursday. I only *need* to be available for my topics.
```

## Step 3: Determine which section

Check whether the meeting is more than 3 days away:
- If **more than 3 days**: add to the "Normal Constraints" section
- If **3 days or fewer**: add to the "Late-breaking Schedule Constraints" section

## Step 4: Present the plan and confirm

Show the user:
1. Which meeting the constraint will be added to
2. The exact formatted constraint line
3. Which section it will be added to (Normal or Late-breaking)

**Ask for explicit confirmation before proceeding.** Do NOT create any branches, clones, or PRs until the user confirms.

## Step 5: Create the PR

Clone or fork the `tc39/agendas` repository:
```
gh repo fork tc39/agendas --clone --remote 2>/dev/null || true
```

Create a branch (e.g., `schedule-constraint-{username}`).

Edit the agenda markdown file to append the constraint line to the appropriate section, after any existing constraints and before any HTML comments.

Commit, push, and create the PR:
```
gh pr create --repo tc39/agendas --title "Schedule constraint: {name}" --body "Adding schedule constraint for the {month} {year} meeting."
```

## Step 6: Report

Report:
- The PR URL
- A reminder that constraints should be submitted at least 3 days before the meeting
