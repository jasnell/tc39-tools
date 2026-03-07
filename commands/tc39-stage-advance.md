---
description: Check if the proposal meets entrance criteria for the next TC39 stage
subtask: true
---

Evaluate whether the current proposal is ready to advance to the next TC39 stage.

## Step 0: Load context

Load the `tc39-proposal` skill for TC39 process reference (stage entrance criteria, required artifacts, reviewer expectations).

Read @spec.emu to determine the current stage from the metadata block (`<pre class="metadata">` stage field).

## Step 1: Determine target stage

The current stage is read from spec.emu metadata. The target is the next stage:
- Stage 0 -> Stage 1
- Stage 1 -> Stage 2
- Stage 2 -> Stage 2.7
- Stage 2.7 -> Stage 3
- Stage 3 -> Stage 4

If `$ARGUMENTS` is provided and is a number (1, 2, 2.7, 3, 4), use that as the target stage instead.

## Step 2: Check entrance criteria

Evaluate each criterion for the target stage. For each, report a status:
- PASS: criterion is clearly met
- FAIL: criterion is clearly not met
- WARN: criterion is partially met or uncertain

### Stage 1 Criteria
- [ ] Champion identified (check spec.emu contributors)
- [ ] Prose outlining problem and solution shape (check for README.md)
- [ ] Public repo exists
- [ ] Initial spec text or API description

### Stage 2 Criteria
- [ ] All high-level APIs and syntax described in spec text
- [ ] Illustrative examples exist (README or slides)
- [ ] Initial spec text with all major semantics
- [ ] Spec text covers all proposed methods/operations
- [ ] Cross-cutting concerns identified (check for interactions with Proxy, SharedArrayBuffer, resizable buffers, etc.)

### Stage 2.7 Criteria
- [ ] Complete spec text — no TODOs, no placeholders, no unresolved semantics
- [ ] Run `!`npm run build 2>&1`` and check for lint errors (all must pass)
- [ ] Spec text follows ECMA-262 editorial conventions (spot-check phrasing, variable naming, completion handling)
- [ ] All edge cases handled (detached buffers, out-of-bounds, invalid inputs)
- [ ] Reviewer sign-offs obtained (check repo issues/PRs for reviewer comments)

### Stage 3 Criteria
- [ ] All Stage 2.7 criteria still met
- [ ] Test262 tests authored (check for `test/` directory or TEST_CASES.md)
- [ ] Pre-implementation experience (polyfill, prototype in an engine)
- [ ] No unresolved design questions (check open GitHub issues)

### Stage 4 Criteria
- [ ] Two compatible implementations passing Test262 tests
- [ ] Significant in-the-field experience
- [ ] PR to tc39/ecma262 with integrated spec text
- [ ] Editor group sign-off

## Step 3: Check for common blockers

Scan for issues that commonly block advancement:
- Open GitHub issues that are tagged as blocking or unresolved design questions: `!`git log --oneline -20 2>/dev/null``
- Spec text quality issues (run a quick editorial review of the first few algorithms)
- Missing `?`/`!` annotations on completion-producing calls
- Placeholder or TODO comments in spec.emu

Use the TC39 MCP tools to verify abstract operations are used correctly (spot-check 2-3 key AO calls).

## Output Format

```markdown
# Stage Advancement Checklist: Stage [current] -> Stage [target]

## Proposal: [title from spec.emu]

## Criteria

| # | Criterion | Status | Notes |
|---|-----------|--------|-------|
| 1 | ...       | PASS/FAIL/WARN | ... |

## Blockers
- [list any FAIL items or significant WARNs]

## Recommendations
- [actionable steps to address each blocker]

## Verdict
[READY / NOT READY — with brief justification]
```
