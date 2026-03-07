---
description: Generate test262 test files from TEST_CASES.md
---

Generate test262-format JavaScript test files from the assertions described in TEST_CASES.md.

First, load the `tc39-proposal` skill for spec conventions. Then read both @spec.emu and @TEST_CASES.md.

If $ARGUMENTS is provided, treat it as a filter — only generate tests matching those TC-IDs (e.g., `TC-0001 TC-0005`) or section names. If no arguments, generate tests for ALL entries.

For each test case entry in TEST_CASES.md, produce a `.js` file in the `test/` directory following the test262 format:

### File naming convention

```
test/<method-name>/<brief-kebab-description>.js
```

Group by method:
- `test/TypedArray.concat/` for %TypedArray%.concat tests
- `test/ArrayBuffer.concat/` for ArrayBuffer.concat tests
- `test/SharedArrayBuffer.concat/` for SharedArrayBuffer.concat tests
- `test/GetConcatenationSources/` for abstract operation tests (tested indirectly)
- `test/ValidateIntegralNumber/` for abstract operation tests (tested indirectly)

### Test file format

Each file MUST follow this exact structure:

```js
// Copyright (C) 2025 James M Snell. All rights reserved.
// This code is governed by the BSD license found in the LICENSE file.
/*---
esid: <spec section id from the TC entry's Clause field>
description: >
  <One-line description from the TC entry title>
info: |
  <Relevant algorithm steps copied from spec.emu, using plain text not ecmarkup>
features: [TypedArray-ArrayBuffer-concat]
---*/

<test body>
```

### Test body conventions

- Use `assert.throws(ErrorType, function() { ... })` for negative tests
- Use `assert.sameValue(actual, expected)` for value comparisons
- Use `assert.compareArray(actual, expected)` for array comparisons  
- Use `assert(condition, message)` for boolean assertions
- For tests verifying byte-level contents, create a Uint8Array view over the result
- For TypedArray tests, test with at least one concrete type (e.g., `Int32Array`) unless the test is specifically about type-name matching
- For observable ordering tests, use Proxy or overridden Symbol.iterator to record operation order
- Keep each test focused on ONE assertion from TEST_CASES.md
- Include the TC-ID in a comment at the top of the test body: `// TC-XXXX`

### Harness includes

Add `includes:` to the frontmatter when needed:
- `[testTypedArray.js]` — when iterating over TypedArray constructors
- `[detachArrayBuffer.js]` — when testing detached buffer behavior
- `[compareArray.js]` — when using `assert.compareArray`

### After generating

Report a summary:
- How many test files were generated
- List each file path and the TC-ID(s) it covers
- Note any TEST_CASES.md entries that could NOT be converted to tests (e.g., if they test unobservable internal behavior) and explain why
