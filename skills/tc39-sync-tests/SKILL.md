---
name: tc39-sync-tests
description: Load when spec.emu has been modified and TEST_CASES.md exists in the project. Automatically detects changes to testable assertions and updates TEST_CASES.md, reporting what was added, updated, or removed. Triggers after any edit to spec.emu in a project that has a TEST_CASES.md file.
license: MIT
compatibility: opencode
---

# Test Case Synchronization

When spec.emu is modified in a project that contains TEST_CASES.md, this skill ensures the test cases stay in sync with the spec.

## When to activate

Activate this skill when ALL of the following are true:
- The user has just edited or you have just edited `spec.emu`
- A `TEST_CASES.md` file exists in the project root
- The edits to spec.emu may have changed testable assertions (algorithm steps, error conditions, type checks, return values, etc.)

Do NOT activate for:
- Editorial-only changes (rewording notes, fixing typos in non-normative text)
- Metadata-only changes (stage number, title, contributors)
- Changes to `<emu-note>` content only

## What to do

### Step 1: Identify what changed in spec.emu

Compare the current spec.emu against the version before the edit. Focus on changes to:
- Algorithm steps in `<emu-alg>` blocks
- Parameter types or return types in structured headers
- New or removed `<emu-clause>` sections
- Error conditions (throw steps)
- Type checks and validation steps
- Observable behavior (property access order, iterator protocol, etc.)

### Step 2: Map changes to existing test cases

Read TEST_CASES.md and identify:
- **New assertions**: Spec changes that introduce testable behavior not covered by any existing TC-ID
- **Updated assertions**: Spec changes that modify the behavior described by an existing TC-ID (e.g., error type changed from TypeError to RangeError, step reordered, condition changed)
- **Removed assertions**: Spec sections or steps that were deleted, making existing TC-IDs obsolete

### Step 3: Update TEST_CASES.md

Apply the changes:
- **New assertions**: Add new TC-XXXX entries at the end of the appropriate section. Use the next available TC-ID number (scan existing IDs to find the highest, then increment).
- **Updated assertions**: Modify the existing entry in place — update the Step, Assertion, Test Description, and Expected fields to match the new spec text. Do NOT change the TC-ID.
- **Removed assertions**: Do NOT delete entries. Instead, add a `- **Status**: REMOVED — [reason]` field to the entry. This preserves traceability.
- **Manual entries**: Never modify entries under the `## Manual Test Cases` section.

Update the header metadata:
- Update the `**Spec version**` to the current git short hash
- Update the `**Generated**` date
- Update the `**Total assertions**` count

### Step 4: Report changes

After updating TEST_CASES.md, report a concise summary:

```
## Test Case Sync Report

**Spec change**: [1-2 sentence summary of what changed in spec.emu]

### New assertions (N)
- TC-XXXX: [title] — [which spec change triggered this]

### Updated assertions (N)
- TC-XXXX: [title] — [what changed: "error type changed from TypeError to RangeError", "step reordered", etc.]

### Removed assertions (N)
- TC-XXXX: [title] — [why: "step deleted", "section removed", etc.]

### Unchanged assertions (N)
[no detail needed, just the count]
```

If there are new or updated assertions, suggest running `/tc39-check-tests` to identify which test files need updating.
