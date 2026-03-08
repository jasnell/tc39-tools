---
description: Fetch and summarize open GitHub issues for a proposal
subtask: true
---

Fetch and categorize open GitHub issues for a TC39 proposal repository.

## Step 0: Determine the repository

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as a GitHub repo reference (e.g., `tc39/proposal-temporal` or a full URL). Parse the `{owner}/{repo}` from it.

If no argument is provided, detect the repo from the current directory:
!`git remote get-url origin 2>/dev/null`

Parse `{owner}/{repo}` from the remote URL. If the remote cannot be determined, report an error and stop.

## Step 1: Fetch open issues

Run `gh issue list` for the detected repository:
```
gh issue list --repo {owner}/{repo} --state open --json number,title,labels,createdAt,author,body,comments --limit 100
```

If `gh` is not available or the repo is not found, report the error and stop.

If there are no open issues, report that and stop.

## Step 2: Categorize and summarize

Group issues into categories based on their labels and content:

- **Blocking / Needs Discussion**: Issues labeled "blocking", "needs-consensus", or that represent unresolved design questions requiring committee input
- **Design Questions**: Issues about API shape, semantics, or naming choices
- **Spec Issues**: Issues about spec text correctness or editorial problems
- **Feature Requests**: Requests for additional functionality
- **Editorial**: Typos, formatting, documentation improvements
- **Other**: Anything that does not fit the above categories

## Output Format

```markdown
# Open Issues: {owner}/{repo}

**Total open issues**: {count}

## Blocking / Needs Discussion

| # | Title | Opened | Labels |
|---|-------|--------|--------|
| [#N](url) | ... | ... | ... |

[1-2 sentence summary of each blocking issue and what decision is needed]

## Design Questions
...

## Spec Issues
...

## Editorial
...

## Other
...

## Summary
- {N} blocking issues that should be resolved before stage advancement
- {N} design questions for committee discussion
- {N} editorial issues (non-blocking)
```
