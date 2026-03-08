---
description: Update an existing agenda entry (slides, timebox, etc.) via PR
---

Update an existing entry for the current proposal in the TC39 plenary meeting agenda by creating a pull request to the `tc39/agendas` repository.

## Step 0: Gather context

Read @spec.emu to get the proposal title from the metadata block.

Get the current proposal's repo URL:
!`git remote get-url origin 2>/dev/null`

Detect the GitHub user:
!`gh auth status 2>&1`

Check for slides:
!`ls slides-export.pdf slides.md 2>/dev/null`
!`git branch -a 2>/dev/null | grep -i plenary`

## Step 1: Fetch the current agenda and find the entry

Use the `tc39_get_agenda` MCP tool to fetch the next upcoming meeting agenda.

Search the proposals table, short discussions, and longer discussions for an entry matching this proposal (by name or repo URL).

If the proposal is NOT on the agenda, report this and stop — suggest using `/tc39-add-to-agenda` instead.

If found, display the current entry in full, including all existing links.

## Step 2: Determine what to update

The argument is: `$ARGUMENTS`

If arguments are provided, use them to determine what to change. Common update types:
- **Add slides link**: e.g., `slides https://...` or detect from local files
- **Change timebox**: e.g., `timebox 45m`
- **Update advancement**: e.g., `advancement "for Stage 2.7 or 3"`
- **Add spec PR link**: e.g., `spec-pr https://...`
- **Add test262 PR link**: e.g., `test262-pr https://...`

If no arguments are provided, ask the user what they want to change. Show the current entry and ask which fields to update.

## Step 3: Present the plan and confirm

Show the user:
1. The **current** table row (before)
2. The **updated** table row (after)
3. A clear diff of what changed

**Ask for explicit confirmation before proceeding.** Do NOT create any branches, clones, or PRs until the user confirms.

## Step 4: Create the PR

Clone or fork the `tc39/agendas` repository:
```
gh repo fork tc39/agendas --clone --remote 2>/dev/null || true
```

Create a descriptively named branch (e.g., `update-{proposal-name}-slides`).

Edit the agenda markdown file to replace the existing row with the updated one. Be precise — match the exact existing row to avoid editing the wrong entry.

Commit, push, and create the PR:
```
gh pr create --repo tc39/agendas --title "Update {proposal title}: {what changed}" --body "{brief description of the change}"
```

## Step 5: Report

Report:
- The PR URL
- The updated entry for verification
