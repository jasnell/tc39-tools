---
name: tc39-proposal
description: Load when editing, reviewing, or authoring TC39 proposal spec text (.emu files), writing ecmarkup, discussing ECMA-262 editorial conventions, or evaluating TC39 stage advancement requirements. Covers ecmarkup syntax, ECMA-262 spec-writing conventions, and the TC39 process.
license: MIT
compatibility: opencode
---

# TC39 Proposal Authoring

This skill covers three tightly related areas needed when working on TC39 proposals:

1. **Ecmarkup syntax** — the markup language used for ECMA-262 spec text
2. **ECMA-262 editorial conventions** — phrasing, algorithm, and text conventions from the tc39/ecma262 editors
3. **TC39 process** — stage entrance criteria and what reviewers expect

Use the `tc39_search_spec`, `tc39_get_spec_section`, `tc39_list_proposals`, and `tc39_get_proposal` MCP tools to look up existing spec text and proposals for reference.

---

# Part 1: Ecmarkup Syntax Reference

Ecmarkup is the markup language for ECMAScript spec text. Source files use `.emu` or `.html` extension. They are compiled with the `ecmarkup` CLI tool. Proposals use `<emu-import>` or inline everything in a single file.

## Document Metadata

```html
<pre class="metadata">
title: Proposal Name
status: proposal
stage: 2
contributors: Ecma International
</pre>
```

Key metadata fields: `title`, `status` (proposal|draft|standard), `stage` (0-4), `version`, `date`, `shortname`, `contributors`, `copyright`.

## Clauses

```html
<emu-clause id="sec-my-operation" type="abstract operation">
  <h1>MyOperation ( _arg_ )</h1>
  <dl class="header">
    <dt>description</dt>
    <dd>It does something useful.</dd>
  </dl>
  <emu-alg>
    1. Return _arg_.
  </emu-alg>
</emu-clause>
```

### Clause Elements

| Element | Purpose |
|---------|---------|
| `<emu-clause>` | Normative clause |
| `<emu-intro>` | Non-normative introduction |
| `<emu-annex>` | Annex clause (add `normative` attribute if normative) |

### `<emu-clause>` Attributes

| Attribute | Description |
|-----------|-------------|
| `id` | Required. Must be unique. Convention: `sec-descriptive-name`. |
| `type` | Operation type: `"abstract operation"`, `"built-in function"`, `"concrete method"`, `"host-defined abstract operation"`, `"implementation-defined abstract operation"`, `"internal method"`, `"numeric method"`, `"sdo"` |
| `legacy` | Marks clause as Legacy |
| `normative-optional` | Marks clause as Normative Optional |
| `number` | Explicit clause number override |

### Structured Headers

Operations must have a structured header with an `<h1>` for the signature and a `<dl class="header">` for metadata.

```html
<emu-clause id="sec-example" type="abstract operation">
  <h1>
    ExampleOp (
      _input_: an ECMAScript language value,
      optional _hint_: a String,
    ): either a normal completion containing a String or a throw completion
  </h1>
  <dl class="header">
    <dt>description</dt>
    <dd>It converts _input_ to a String, optionally guided by _hint_.</dd>
  </dl>
  ...
</emu-clause>
```

Header `<dl>` fields:
- **description** — explanation of behavior (normative; must be correct but may omit details)
- **effects** — comma-separated effects, e.g. `user-code`
- **for** — (concrete method / internal method) the type this method applies to
- **redefinition** — `"true"` to suppress auto-linking
- **skip global checks** / **skip return checks** — linter directives

Parameter format in `<h1>`: each parameter on its own line, `_name_: type constraint`, optionally preceded by `optional`, comma-terminated.

## Algorithms (`<emu-alg>`)

Algorithm content uses **Ecmarkdown** syntax (not HTML). The content of `<emu-alg>` is plain text parsed as Ecmarkdown.

```html
<emu-alg>
  1. Let _x_ be 0.
  1. If _x_ is *undefined*, then
    1. Return *false*.
  1. Else,
    1. Return *true*.
</emu-alg>
```

Rules:
- Use `1.` for all step numbers (auto-numbered by ecmarkup).
- Indent sub-steps with 4 spaces.
- Steps starting with `1.` become ordered list items.
- Loops: `1. Repeat, while _condition_,` followed by indented sub-steps.
- Labels: `1. [id="step-my-label"] Do something.` for cross-referenceable steps.
- Variable declarations: `1. [declared="x"] ...` when the linter can't infer declaration.
- AO calls are auto-linked: `Get(_O_, _P_)` links to the Get AO.
- Use `?` for ReturnIfAbrupt, `!` for Assert-not-abrupt: `? Get(_O_, _P_)`, `! ToString(_x_)`.

### Ecmarkdown Inline Syntax

| Convention | Ecmarkdown | HTML equivalent |
|-----------|-----------|----------------|
| Variables | `_variable_` | `<var>variable</var>` |
| ES language values | `*undefined*`, `*true*`, `*TypeError*` | `<emu-val>...</emu-val>` |
| Spec constants/enums | `~throw~`, `~empty~`, `~unused~` | `<emu-const>...</emu-const>` |
| Source code | `` `code` `` | `<code>code</code>` |
| Non-terminal refs | `\|FunctionExpression\|` | `<emu-nt>...</emu-nt>` |

## Other Key Elements

### `<emu-note>`
```html
<emu-note>
  <p>This is non-normative explanatory text.</p>
</emu-note>
<emu-note type="editor">Editor's note, removed before ratification.</emu-note>
```

### `<emu-xref>`
```html
<!-- By href (clause/step/table id) -->
<emu-xref href="#sec-get-o-p"></emu-xref>           <!-- renders as section number -->
<emu-xref href="#sec-get-o-p" title></emu-xref>     <!-- renders as title -->

<!-- By aoid (abstract operation) -->
<emu-xref aoid="Get"></emu-xref>
```

### `<emu-table>` and `<emu-figure>`
```html
<emu-table id="table-my-table">
  <emu-caption>Description of Table</emu-caption>
  <table>
    <tr><th>Header</th></tr>
    <tr><td>Value</td></tr>
  </table>
</emu-table>
```

### `<emu-grammar>` (Grammarkdown syntax)
```html
<emu-grammar type="definition">
  MyProduction :
    `keyword` Identifier
    `keyword` Identifier `(` ArgumentList `)`
</emu-grammar>
```

### `<emu-biblio>` (external cross-references)
```html
<emu-biblio href="./biblio.json"></emu-biblio>
```

Proposals should load `@tc39/ecma262-biblio` to auto-link references to the main spec.

### Indicating Changes (`<ins>`/`<del>`)
```html
<!-- Inline changes -->
Return <del>*undefined*</del><ins>*true*</ins>.

<!-- Block changes (entire steps, clauses, grammar RHSes) -->
<ins class="block">
  1. Let _x_ be 42.
</ins>
```

### `<emu-import>`
```html
<emu-import href="./section.html"></emu-import>
```

## Linting

Run with `ecmarkup --lint-spec spec.emu out.html`. The linter checks:
- Variables used without declaration
- Unused variables
- Calls to unknown AOs
- Missing `?`/`!` on completion-returning calls
- Type mismatches in parameters
- Structured header correctness

---

# Part 2: ECMA-262 Editorial Conventions

Source: https://github.com/tc39/ecma262/wiki/Editorial-Conventions

These are the conventions enforced by the ECMA-262 editors. Proposals should follow them to minimize editorial friction at Stage 4.

## General Principles

- Use consistent phrasing as much as possible.
- Minimize the number of foundational concepts without loss of readability.
- AO summaries (descriptions) are normative — every claim must be correct, but they may omit details.
- Preserve old IDs using the `oldids` attribute to avoid breaking fragment links.

## Phrasing Conventions

- Use `%` notation for intrinsic objects (e.g. `%Array.prototype%`), not "the initial value of".
- Test field presence: `_record_ has a [[X]] field`, NOT `_record_.[[X]] is present`.
- Use `a`/`an`/`a new` for values with identity; `the` for values without identity.
- Don't unnecessarily qualify types with "object": `_x_ is an Array`, NOT `_x_ is an Array object`. Exceptions: "Set object", "Error object", "RegExp object", "Proxy object", exotic objects.
- Don't say `the value of _a_`; just use `_a_`.
- Don't say "value" after a spec type following a determiner: prefer `a String` over `a String value`.
- Don't use increment/decrement; use `set _x_ to _x_ + 1`.
- Prefer `the empty String` over `*""*`.
- For type unions:
  - All singletons: `A`, `either A or B`, `one of A, B, or C`
  - Otherwise: `an A`, `either an A or a B`, `either an A, a B, or a C` (multi-inhabitant types listed before singletons)

### Lists

- Create: `a new empty List` (outside AO args); `« value1, value2 »` (inside AO args).
- Concatenate: `the list-concatenation of _listA_ and _listB_`.
- Mutate: `Append _x_ to _list_` or `Prepend _x_ to _list_`.
- Test empty: `_list_ is empty` / `_list_ is not empty`.
- Single element: `the sole element of _list_`.
- Length: `the number of elements in _list_`.

### Comparisons

- Math values / BigInts: use `=`, `!=`, `<`, `<=`, `>`, `>=` (prefer `=` over `!=`).
- Number values: use `is`, `is not`, `<`, `<=`, `>`, `>=`.
- Symbols (non-well-known) / objects / unknown ES values: use `SameValue(..., ...) is *true*`.
- Everything else (booleans, strings, well-known symbols, null, undefined, enums): use `is` / `is not`.
- Prefer `if _a_ is either _b_ or _c_` over `if _a_ is _b_ or _a_ is _c_`.
- Prefer `if _a_ is _c_ and _b_ is _c_` over `if _a_ and _b_ are both _c_`.
- List membership: `_list_ contains _element_` / `_list_ does not contain _element_`.
- When comparing alias vs constant, alias on LHS, constant on RHS.
- When comparing against both: `is either *undefined* or *null*` (this order).

## Algorithm Conventions

- **Assertions should be removable** — a reader should interpret the algorithm the same without them.
- **Each alias introduced at most once** per possible trace through the algorithm.
- **If/else consistency**: either all branches have substeps or all are collapsed (single-line).
- **Single-line if/else**: all branches should "do the same thing" (set same alias, return, perform, etc.).
- **Early-exit if/else (2 branches, similar)**: prefer a single step.
- **Early-exit if/else (2 branches, asymmetric)**: prefer multi-step, no leading `Else,`.
- **If-else cascade (3+ branches)**: prefer multi-step, no leading `Else,`.
- **`~unused~` returning AOs**: invoke with `Perform`, at top level of a step.
- **Prefer spec enums** over sentinel values like -1.
- **Don't reuse enum values** across unrelated contexts (except `~unused~`).
- **Don't mutate Completion Records.**
- **Return constants, not aliases**: `If _x_ is *null*, return *null*.` (not `return _x_`).
- **Completion shorthands**: prefer `is a throw completion`, `is a normal completion` over comparing `[[Type]]`.
- **Lead with validation** steps that early exit, then follow the happy path.
- **Prefer descriptive alias names** over single-letter or abbreviated names.
- **One thing per step** — don't combine what could be 2+ steps into 1.
- **All completion-producing calls** must be annotated: `? Foo()`, `! Foo()`, or `Completion(Foo())`.
- **Algorithms must be consistent** — either always produce Completions or never produce them.
- **Avoid unnecessarily dynamic constructs**: no AO names in table cells as dispatch, no constructing AO names to invoke, no aliasing AOs, no ambient state coordination.

## Text Conventions

- Include Oxford commas.
- Denote characters literally (HTML entities only when required by HTML syntax).
- Record spacing: `{ [[key1]]: value1, [[key2]]: value2 }`.
- List spacing: `« value1, value2 »`.
- Always sign floating-point zeros: `*+0*F` or `*-0*F`, never `*0*F`.

---

# Part 3: TC39 Process and Stage Requirements

Source: https://tc39.es/process-document/

## Stage Summary

| Stage | Status | Key Entrance Criteria |
|-------|--------|----------------------|
| 0 | Strawperson | None. Not being considered by committee. |
| 1 | Under consideration | Champion identified. Prose outlining problem + solution shape. Public repo. |
| 2 | Draft | All high-level APIs and syntax described. Illustrative examples. Initial spec text with all major semantics (TODOs acceptable). |
| 2.7 | Approved in principle | Complete spec text. Assigned reviewers signed off. Editor group signed off. |
| 3 | Recommended for implementation | Sufficient testing. Appropriate pre-implementation experience. |
| 4 | Complete | Two compatible implementations passing Test262. Significant field experience. PR to tc39/ecma262 with integrated spec text. Editor group signed off. |

## What Reviewers Look For

### Stage 1 -> 2
- Problem statement is clear and well-motivated.
- Solution covers the design space adequately.
- Spec text exists for all major semantics, syntax, and APIs.
- Cross-cutting concerns identified and addressed.
- Examples illustrate typical usage.

### Stage 2 -> 2.7
- **Complete spec text** — no TODOs, no placeholders, no unresolved semantics.
- **Assigned reviewers** have reviewed and signed off.
- **Editor group** has reviewed and signed off.
- All edge cases handled (detached buffers, out-of-bounds, invalid inputs, etc.).
- Consistent with existing ECMA-262 editorial conventions.
- Algorithm steps follow the conventions in Part 2 above.

### Stage 2.7 -> 3
- **Test262 tests** authored and submitted (PR to tc39/test262).
- Pre-implementation experience (polyfill, prototype, etc.).
- No unresolved design questions.

### Stage 3 -> 4
- **Two compatible implementations** passing Test262 tests.
- Significant in-the-field experience with shipping implementations.
- **PR to tc39/ecma262** with the integrated spec text.
- Editor group signed off on the PR.

## Required Artifacts by Stage

| Stage | Required Artifacts |
|-------|-------------------|
| 1 | Explainer (README), public repo |
| 2 | Explainer, initial spec text (.emu), examples |
| 2.7 | Complete spec text, reviewer sign-offs |
| 3 | Complete spec text, Test262 tests |
| 4 | Integrated PR to ecma262, two implementations, Test262 merged |

## Common Feedback Patterns

- **"Follow the editorial conventions"** — phrasing doesn't match ECMA-262 style (see Part 2).
- **"Missing error handling"** — not all TypeError/RangeError paths are specified.
- **"Needs to handle detached buffers"** — for anything touching ArrayBuffer/TypedArray.
- **"Use existing AOs"** — duplicating logic that already exists as an abstract operation.
- **"Missing ? or !"** — completion-producing calls must be annotated.
- **"Spec text quality"** — at Stage 2.7+, spec text must be editor-ready, not draft-quality.
- **"Cross-cutting concerns"** — how does this interact with Proxies, SharedArrayBuffer, resizable buffers, etc.?
