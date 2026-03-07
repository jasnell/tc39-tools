---
description: Find similar ECMA-262 APIs and compare patterns for consistency
subtask: true
---

Find existing ECMA-262 methods and abstract operations that are similar to the APIs in this proposal, then compare patterns to ensure consistency.

## Step 0: Determine what to compare

If `$ARGUMENTS` is provided, treat it as the name of a specific method or pattern to compare (e.g., `ArrayBuffer.concat`, `options bag`, `static factory`).

If no argument, read @spec.emu and automatically identify all proposed methods and abstract operations.

## Step 1: Load context

Load the `tc39-proposal` skill for editorial conventions reference.

## Step 2: Find comparable APIs in ECMA-262

For each proposed method or AO, use the TC39 MCP tools (`tc39_search_spec`, `tc39_get_spec_section`) to find and fetch the most relevant existing spec sections. Look for:

**For static factory/concat methods**, compare with:
- `Array.from` / `Array.of`
- `TypedArray.from` / `TypedArray.of`
- `Object.assign` / `Object.fromEntries`
- `ArrayBuffer.isView`
- Any other static methods on built-in constructors

**For methods accepting iterables**, compare with:
- `Promise.all` / `Promise.allSettled` / `Promise.any` / `Promise.race`
- `Map` / `Set` constructors
- `Array.from`
- How they consume the iterable (upfront vs lazy)

**For options bag patterns**, compare with:
- `structuredClone(value, options)`
- `Intl` constructors (extensive options bag usage)
- `String.prototype.normalize`
- `Array.prototype.toSorted` / `toSpliced`
- How they validate the options object, read properties, handle defaults

**For abstract operations on buffers/typed arrays**, compare with:
- `ValidateTypedArray`
- `AllocateArrayBuffer` / `AllocateSharedArrayBuffer`
- `CloneArrayBuffer`
- `GetViewByteLength` / `GetArrayBufferMaxByteLengthOption`
- `IsDetachedBuffer` / `IsFixedLengthArrayBuffer`

## Step 3: Report comparison

For each proposed API, produce a comparison table and analysis:

```markdown
## [Proposed Method] vs [Comparable Method(s)]

### Pattern Comparison

| Aspect | This Proposal | [Comparable] | Consistent? |
|--------|--------------|--------------|-------------|
| Argument validation order | ... | ... | Yes/No |
| Error types thrown | ... | ... | Yes/No |
| Iterable consumption | ... | ... | Yes/No |
| Options reading pattern | ... | ... | Yes/No |
| Return type | ... | ... | Yes/No |
| Completion handling | ... | ... | Yes/No |

### Inconsistencies Found
- [describe each inconsistency and whether it's intentional or a bug]

### Recommendations
- [specific changes to align with existing patterns, or justification for diverging]
```

## Step 4: Summary

End with an overall consistency score and the most important alignment issues to address before stage advancement.
