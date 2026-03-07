---
description: Structured review of a TC39 proposal spec.emu
subtask: true
---

Perform a thorough structured review of a TC39 proposal's spec.emu file, simulating the review a TC39 delegate would perform.

## Step 0: Resolve the spec to review

The argument is: `$ARGUMENTS`

- **If no argument is given**: Review the local project. Check that `spec.emu` exists in the current working directory. If it does not exist, report an error: "No spec.emu found in the current directory. Either run this command from a proposal repo or provide a proposal name or GitHub URL as an argument." Then stop — do not continue with the review.
- **If the argument is a GitHub URL** (e.g., `https://github.com/tc39/proposal-foo`): Use WebFetch to fetch the raw `spec.emu` from the repo's default branch (`https://raw.githubusercontent.com/<owner>/<repo>/HEAD/spec.emu`). If the fetch fails or returns 404, report an error: "No spec.emu found at <url>." Then stop.
- **If the argument is a proposal name** (e.g., `temporal`, `iterator-helpers`): Use the `tc39_get_proposal` tool with `include_spec: true` to fetch the proposal's spec text. If the proposal is not found or has no spec.emu, report an error: "Could not find spec.emu for proposal '<name>'." Then stop.

Once you have the spec text, proceed with the review.

## Step 1: Load context

Load the `tc39-proposal` skill for ecmarkup syntax, ECMA-262 editorial conventions, and TC39 process reference.

If reviewing the local project, read @spec.emu. Otherwise use the fetched content.

## Step 2: Review

Use the TC39 MCP tools (`tc39_search_spec`, `tc39_get_spec_section`) to look up every abstract operation and built-in referenced in the spec to verify correct usage — parameter order, return type handling, completion value handling.

Perform the review in these phases, reporting findings for each:

### Phase 1: Correctness

- **AO signatures**: Does each abstract operation have correct parameter types, return type, and description?
- **Completion handling**: Is every `?` and `!` used correctly? Are throw completions propagated? Are normal completions unwrapped?
- **Type consistency**: Are mathematical values vs Number values used correctly? Are spec type annotations consistent?
- **Algorithm logic**: Are there off-by-one errors, missing edge cases, or unreachable steps?
- **Cross-references**: Do all `<emu-xref>` targets exist? Are step labels correct?
- **Internal slot access**: Are internal slots checked before access? Is the correct guard used (e.g., checking for [[TypedArrayName]] before accessing TypedArray slots)?

### Phase 2: Editorial Conventions

- **Phrasing**: Does the spec text follow ECMA-262 editorial conventions? (e.g., "performs the following steps when called", "is either a normal completion containing ... or a throw completion")
- **Variable naming**: Do variable names follow conventions? (_camelCase_ for algorithm variables, [[SlotNames]] for internal slots)
- **List formatting**: Are lists formatted correctly per the ECMA-262 style?
- **Note placement**: Are `<emu-note>` elements used appropriately and placed after the relevant algorithm?

### Phase 3: Completeness

- **Missing error cases**: Are all error conditions covered? (wrong types, detached buffers, out-of-range values, etc.)
- **Missing observable steps**: Are property accesses, iterator operations, and type conversions in the right order?
- **Missing edge cases**: Zero-length inputs, single-element inputs, maximum-size inputs, empty iterables
- **Missing `Assert` steps**: Should any implicit assumptions be made explicit with Assert?

### Phase 4: Design Feedback

- **API design**: Is the API consistent with existing ECMA-262 patterns? Compare with similar methods (e.g., `Array.from`, `TypedArray.from`, `structuredClone`)
- **Options bag pattern**: Is the options bag handled consistently with other ECMA-262 methods that use options?
- **Naming**: Are method names, option names, and parameter names clear and consistent with existing conventions?
- **Cross-proposal dependencies**: Are dependencies on other proposals (e.g., Immutable ArrayBuffer) handled cleanly?

## Output Format

For each finding, report:
- **Severity**: error (spec bug), warning (likely issue), suggestion (improvement), nit (editorial)
- **Location**: Section ID and step number
- **Issue**: What's wrong
- **Recommendation**: How to fix it, with corrected spec text where applicable

End with a summary table of findings by severity and phase.
