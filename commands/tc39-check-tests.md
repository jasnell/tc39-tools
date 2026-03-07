---
description: Cross-reference TEST_CASES.md against test/ files for coverage gaps
subtask: true
---

Cross-reference the test assertions in TEST_CASES.md against the actual test files in the `test/` directory and report coverage gaps.

## Step 0: Validate inputs

Check that both `TEST_CASES.md` and a `test/` directory exist in the current project.

- If `TEST_CASES.md` does not exist, report: "No TEST_CASES.md found. Run `/tc39-generate-tests` first to extract testable assertions from spec.emu." Then stop.
- If the `test/` directory does not exist, report: "No test/ directory found. Run `/tc39-generate-test262` first to generate test files." Then stop.

## Step 1: Parse TEST_CASES.md

Read @TEST_CASES.md and extract all test case entries. For each, record:
- TC-ID (e.g., TC-0001)
- Title
- Clause / section ID
- Category (positive, negative, boundary, etc.)

## Step 2: Scan test files

Read all `.js` files under `test/`:
!`find test/ -name '*.js' -type f 2>/dev/null`

For each test file, extract:
- The TC-ID(s) referenced in comments (look for `// TC-XXXX` patterns)
- The `esid` from the test262 frontmatter
- The `description` from the frontmatter
- The `features` tag

## Step 3: Cross-reference and report

### Coverage Matrix

```markdown
| TC-ID | Title | Category | Test File | Status |
|-------|-------|----------|-----------|--------|
| TC-0001 | ... | negative | test/ArrayBuffer.concat/throws-on-non-iterable.js | COVERED |
| TC-0002 | ... | positive | — | MISSING |
| — | — | — | test/ArrayBuffer.concat/extra-test.js | ORPHAN |
```

Status values:
- **COVERED**: A test file references this TC-ID
- **MISSING**: No test file references this TC-ID
- **ORPHAN**: A test file doesn't map to any TC-ID in TEST_CASES.md
- **STALE**: A test file references a TC-ID that no longer exists or whose assertion has changed

### Summary

```markdown
## Coverage Summary

- Total assertions: [N]
- Covered: [N] ([%])
- Missing: [N] ([%])
- Orphan test files: [N]
- Stale test files: [N]

### Missing by section
- [section-id]: [N] missing (TC-XXXX, TC-YYYY, ...)

### Missing by category
- negative: [N] missing
- positive: [N] missing
- boundary: [N] missing
```

### Recommendations

List the highest-priority missing tests — prioritize:
1. Negative tests (error handling) — most commonly requested by reviewers
2. Boundary tests (edge cases)
3. Positive tests (happy path)

$ARGUMENTS
