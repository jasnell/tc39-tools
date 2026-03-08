---
description: Add the current proposal to the next TC39 meeting agenda via PR
---

Add the current proposal to the next TC39 plenary meeting agenda by creating a pull request to the `tc39/agendas` repository.

## Step 0: Gather context

Read @spec.emu to get the proposal title, current stage, and contributors from the metadata block.

Detect the GitHub user:
!`gh auth status 2>&1`

Parse the authenticated GitHub username from the output above.

Get the current proposal's repo URL:
!`git remote get-url origin 2>/dev/null`

Check for slides:
!`ls slides-export.pdf slides.md 2>/dev/null`
!`git branch -a 2>/dev/null | grep -i plenary`

Check for open spec PRs against ecma262:
!`gh pr list --repo tc39/ecma262 --author @me --state open --json url,title 2>/dev/null`

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the target stage for advancement (e.g., `/tc39-add-to-agenda 2` means "for Stage 2"). If not provided, infer the next stage from the current stage in spec.emu.

## Step 1: Fetch the current agenda

Use the `tc39_get_agenda` MCP tool (with no `meeting` parameter) to fetch the next upcoming meeting agenda.

Check if this proposal is already on the agenda (match by proposal name or repo URL). If it is, report this and stop — suggest using `/tc39-update-agenda` instead.

## Step 2: Determine the agenda entry

Build the proposals table row. The format is:

```
| {stage} | {timebox} | [{proposal title}]({repo url}) for Stage {target} ([slides]({slides url})) | {presenter} |
```

Where:
- **stage**: the current stage number (not the target)
- **timebox**: suggest a reasonable default based on the advancement type:
  - Stage 0→1: 30m
  - Stage 1→2: 30m
  - Stage 2→2.7: 30m
  - Stage 2.7→3: 30m
  - Stage 3→4: 30m
  - Status update (no advancement): 15m
- **topic**: proposal name linked to repo, advancement text, and links to all available supporting materials (slides, spec PRs, test262 PRs)
- **presenter**: the contributor name from spec.emu

The row must be inserted in the correct sort position within the Proposals table: **stage descending, then timebox ascending, then at the end of entries with the same stage and timebox**.

## Step 3: Present the plan and confirm

Show the user:
1. Which meeting the entry will be added to (date, location)
2. The exact table row that will be inserted
3. Where in the table it will be placed (after which entry)
4. The advancement deadline and whether we are before or after it

**Ask for explicit confirmation before proceeding.** Do NOT create any branches, clones, or PRs until the user confirms. Ask if they want to adjust the timebox or any other detail.

## Step 4: Create the PR

Clone or fork the `tc39/agendas` repository and create a branch:
```
gh repo fork tc39/agendas --clone --remote 2>/dev/null || true
```

If the user has push access to `tc39/agendas`, create a branch directly. Otherwise, work from the fork.

Create a descriptively named branch (e.g., `add-{proposal-name}-{meeting}`).

Edit the agenda markdown file to insert the new row in the correct position within the Proposals table.

Commit, push, and create the PR:
```
gh pr create --repo tc39/agendas --title "Add {proposal title} to {month} {year} agenda" --body "Add [{proposal title}]({repo url}) for Stage {target} advancement."
```

## Step 5: Report

Report:
- The PR URL
- A reminder about the advancement deadline
- A reminder to add slides links later with `/tc39-update-agenda` if slides aren't ready yet
