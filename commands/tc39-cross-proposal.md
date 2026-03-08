---
description: Detect shared abstract operations and patterns across TC39 proposals
subtask: true
---

Scan multiple TC39 proposal spec texts to detect shared abstract operations, common patterns, and potential conflicts.

## Step 0: Gather context

Load the `tc39-proposal` skill for ecmarkup syntax reference.

The argument is: `$ARGUMENTS`

If arguments are provided, treat them as directory paths to proposal repos to scan.

If no arguments are provided, scan common proposal locations. Check which of these directories exist and contain a `spec.emu` file:
!`for d in /home/jsnell/projects/other/proposal-*/; do [ -f "$d/spec.emu" ] && echo "$d"; done 2>/dev/null`

If no proposals are found, report an error and stop.

## Step 1: Extract definitions from each proposal

For each proposal directory found, read its `spec.emu` and extract:

1. **Abstract operations defined** — every `<emu-clause type="abstract operation">` with its name, parameters, and return type
2. **Built-in methods defined** — every `<emu-clause type="numeric method">` or similar built-in definitions
3. **Abstract operations called** — every AO invocation in algorithm steps (both defined locally and external references to ECMA-262)
4. **Internal slots used** — every `[[SlotName]]` referenced
5. **Well-known intrinsics used** — every `%Name%` reference
6. **Spec metadata** — title, stage, contributors from the `<pre class="metadata">` block

## Step 2: Cross-reference

Compare the extracted definitions across all proposals:

### Shared AO Definitions
Identify abstract operations that are defined in more than one proposal (same name). For each:
- Compare parameter lists (same? different?)
- Compare algorithm steps (semantically equivalent? divergent?)
- Flag whether they could be unified or if differences are intentional

### Shared AO Dependencies
Identify abstract operations from ECMA-262 that multiple proposals depend on. Highlight any that might need modification if both proposals advance.

### Naming Conflicts
Check for clause IDs, AO names, or built-in method names that could collide.

### Common Patterns
Identify similar algorithm patterns across proposals (e.g., both do iterable validation the same way, both handle options bags similarly).

## Output Format

```markdown
# Cross-Proposal Analysis

## Proposals Scanned

| Proposal | Stage | AOs Defined | Built-ins Defined |
|----------|-------|-------------|-------------------|
| ...      | ...   | ...         | ...               |

## Shared Abstract Operations

### {AO Name}
- **Defined in**: proposal-A, proposal-B
- **Parameters**: (comparison)
- **Compatibility**: IDENTICAL / COMPATIBLE / CONFLICTING
- **Recommendation**: (merge, coordinate, or diverge intentionally)

## Shared Dependencies on ECMA-262

| ECMA-262 AO | Used by |
|--------------|---------|
| ...          | ...     |

## Potential Conflicts
- (any clause ID, name, or slot collisions)

## Common Patterns
- (similar algorithm structures that could be extracted)

## Recommendations
- (actionable steps for coordination)
```
