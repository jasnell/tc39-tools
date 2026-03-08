---
description: Quick editorial conventions check on spec.emu
---

Perform a fast editorial conventions check on the spec text in the current proposal. This is a lighter alternative to `/tc39-review-spec` — it checks only ECMA-262 editorial conventions, skipping correctness, completeness, and design review.

## Step 0: Load context

Load the `tc39-proposal` skill for ECMA-262 editorial conventions reference.

Read @spec.emu for the full spec text.

## Step 1: Check editorial conventions

Scan the spec text for violations of ECMA-262 editorial conventions. Check every algorithm, clause, and note for the following categories:

### Phrasing
- Intrinsic references use `%Name%` format
- Field/slot presence: "has a [[Field]] field" (not "has a [[Field]]")
- Article usage: "a Number" / "an Object" (not "a number" / "an object" for spec types)
- Type unions: "a Number or undefined" (not "a Number or an undefined")
- List operations: "the list-concatenation of" / "append X to Y"
- Comparisons: "is X" (not "is equal to X"), "is not X" (not "is not equal to X")

### Algorithm style
- Completion handling: every call to an AO that can return abrupt must use `?` or `!`
- Assert steps: "Assert: X" with a full stop, not "Assert X"
- If/else: use "If ... then" / "Else," (not "Otherwise")
- One assertion per step (not "Assert: X and Y")
- Validation before action (parameter checks first, then operations)
- Let bindings: "Let _x_ be ..." (italicized variable names)

### Formatting
- Variable names are in _italics_ (ecmarkdown `_x_`)
- Spec values use *bold* (ecmarkdown `*true*`, `*undefined*`)
- Constants use ~tilde~ (ecmarkdown `~empty~`)
- Record fields use [[DoubleSquareBrackets]]
- Oxford commas in lists of three or more
- Signed zero: *+0*𝔽 and *-0*𝔽 (not just 0)

### Structure
- All `<emu-clause>` elements have an `id` attribute
- Structured headers have `<dl class="header">` with appropriate fields
- `<emu-note>` used for non-normative content (not bare paragraphs)
- Cross-references use `<emu-xref>` where appropriate

Use the TC39 MCP tools to spot-check 2-3 abstract operations referenced in the spec to verify they are called with the correct number of arguments and correct parameter names.

## Output Format

For each finding, report:
- **Location**: clause ID and step number (or line description)
- **Category**: phrasing / algorithm / formatting / structure
- **Issue**: what's wrong
- **Fix**: the corrected text

Group findings by category. At the end, provide a summary:

```markdown
## Editorial Check Summary

| Category | Issues |
|----------|--------|
| Phrasing | N |
| Algorithm | N |
| Formatting | N |
| Structure | N |

**Total**: N issues found
```

If no issues are found, report a clean bill of health.
