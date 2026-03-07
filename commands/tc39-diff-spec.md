---
description: Summarize spec.emu changes between commits or branches
---

Produce a structured, human-readable summary of changes to spec.emu between two points in git history. This is useful for preparing "changes since last presented" slides for TC39 meetings.

## Step 0: Resolve the diff range

The argument is: `$ARGUMENTS`

- **No argument**: Diff the working tree against HEAD (i.e., uncommitted changes). If there are no changes to spec.emu, try HEAD~1..HEAD instead (last commit). If spec.emu was not modified in either case, report "No spec.emu changes found." and stop.
- **One argument** (a commit, tag, or branch name like `main`, `v2`, `abc1234`): Diff that ref against the current working tree. E.g., `/tc39-diff-spec main` shows everything changed since diverging from main.
- **Two arguments** (e.g., `v1 v2`, `main feature-branch`, `abc1234 def5678`): Diff the first ref against the second.

Obtain the raw diff:
!`git diff $ARGUMENTS -- spec.emu 2>&1 || git diff HEAD -- spec.emu 2>&1`

If the diff is empty, report "No changes to spec.emu in the specified range." and stop.

## Step 1: Load context

Load the `tc39-proposal` skill for ecmarkup syntax and editorial conventions reference.

## Step 2: Analyze the diff

Parse the diff and categorize every change. For each changed section (identified by `<emu-clause id="...">` context), report:

### Structural Changes
- New clauses or abstract operations added
- Clauses removed or renamed
- Reordering of sections

### Algorithm Changes
For each modified `<emu-alg>`, describe:
- Which steps were added, removed, or modified
- The semantic impact of the change (e.g., "adds a new validation step", "changes the error type from TypeError to RangeError", "fixes an off-by-one in the byte offset calculation")
- Whether the change is normative (affects observable behavior) or editorial (rewording only)

### Signature Changes
- Parameter additions/removals/type changes on abstract operations
- Return type changes

### Note and Documentation Changes
- New or updated `<emu-note>` elements
- Changes to `<dl class="header">` descriptions

### Metadata Changes
- Stage number changes
- Title or contributor changes

## Output Format

```markdown
## Spec Changes: <range description>

### Summary
<2-3 sentence overview of what changed and why it matters>

### Normative Changes
1. **<section-id>**: <description of normative change>
   - Steps affected: <step numbers>
   - Impact: <what observable behavior changes>

### Editorial Changes
1. **<section-id>**: <description of editorial change>

### New Sections
- **<section-id>**: <brief description>

### Removed Sections
- **<section-id>**: <brief description>
```

If the changes are small, collapse the categories that have no entries. Keep the output concise — this should be scannable in a few minutes.
