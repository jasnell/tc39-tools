---
description: Extract testable assertions from spec.emu into TEST_CASES.md
---

Generate or update TEST_CASES.md by extracting all testable assertions from the proposal's spec.emu file.

First, load the `tc39-proposal` skill for ecmarkup and spec conventions reference.

Read @spec.emu and systematically extract every testable assertion. A testable assertion is any spec behavior that can be verified by a JavaScript test, including:

- Type checks and validation steps (e.g., "If X is not a Y, throw a *TypeError* exception")
- Range checks (e.g., "If X < 0, throw a *RangeError* exception")
- Return values under specific conditions
- Observable side effects (e.g., property access order, iterator protocol usage)
- Edge cases (empty inputs, zero-length, maximum values)
- Interactions between options (e.g., mutually exclusive flags)
- Algorithmic correctness (e.g., correct byte copying, correct element offsets)
- The `this` value requirements
- Prototype and constructor requirements
- Boundary conditions at each clamping/truncation step

For EACH testable assertion, produce an entry in the following format. Do NOT skip any assertion -- be exhaustive.

```markdown
### TC-XXXX: [Brief title]
- **Clause**: `sec-id` — [Section title]
- **Step**: [Algorithm step number(s)]
- **Assertion**: [Exact spec behavior being tested, paraphrased precisely]
- **Category**: positive | negative | boundary | type-check | observable-ordering
- **Preconditions**: [Setup required — what objects/state must exist]
- **Test Description**: [Detailed description of what the test does, including specific input values to use and why they exercise this assertion. This must be detailed enough for an implementer to write the test without re-reading the spec.]
- **Expected**: [Exact expected outcome — return value, exception type, property values, etc.]
```

Number the IDs sequentially: TC-0001, TC-0002, etc.

Group entries by spec section using level-2 headings:

```markdown
## %TypedArray%.concat (`sec-%typedarray%.concat`)
### TC-0001: ...
### TC-0002: ...

## GetConcatenationSources (`sec-getconcatenationsources`)
### TC-0010: ...
```

At the top of TEST_CASES.md, include a header:

```markdown
# Test Cases: [Proposal title from spec.emu metadata]

> Auto-generated from `spec.emu`. This file describes testable assertions
> extracted from the proposal spec text. Each entry provides sufficient
> detail for `/tc39-generate-test262` to produce a complete test262 test file.
>
> **Do not edit auto-generated entries by hand.** Instead, update spec.emu
> and re-run `/tc39-generate-tests`. You may add manual entries at the end
> under a `## Manual Test Cases` section — these will be preserved across
> regenerations.

**Spec version**: [git short hash from !`git rev-parse --short HEAD`]
**Generated**: [current date]
**Total assertions**: [count]
```

If TEST_CASES.md already exists, read it first. Preserve any entries under a `## Manual Test Cases` section at the end. Replace all auto-generated entries.

$ARGUMENTS
