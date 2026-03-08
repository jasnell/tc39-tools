---
description: Research prior art for a TC39 proposal concept across languages and platforms
subtask: true
---

Research how a given concept or API pattern is handled across programming languages, platforms, and the JavaScript ecosystem. Produces a structured prior art survey suitable for inclusion in a TC39 proposal README.

## Step 0: Determine the research topic

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the topic to research (e.g., "error codes", "buffer concatenation", "ownership transfer", "subsequence search").

If no argument is provided, read @spec.emu and @README.md from the current directory to infer the core concept from the proposal title and problem statement. If neither exists, ask the user for a topic.

## Step 1: Search TC39 proposals

Use the `tc39_list_proposals` and `tc39_get_proposal` MCP tools to find related TC39 proposals — both active and finished (Stage 4). Look for:
- Proposals that address the same problem space
- Proposals that use similar API patterns
- Proposals that this concept depends on or interacts with

## Step 2: Research the JavaScript ecosystem

Research how the concept is currently handled in:

### JavaScript runtimes
- **Node.js**: Check for relevant APIs, `ERR_*` codes, or built-in utilities
- **Deno**: Check for equivalent APIs
- **Bun**: Check for equivalent APIs
- **Cloudflare Workers**: Check for relevant APIs

### Web Platform
- **DOM / Web APIs**: Check for related interfaces (e.g., DOMException, Streams API, Structured Clone)
- **WHATWG specs**: Check for related specifications
- **MDN**: Check for documentation of current approaches and their limitations

### Popular libraries
Search for widely-used npm packages that address this concept. Focus on libraries with >1M weekly downloads or significant ecosystem adoption. Note their API design choices.

## Step 3: Research other programming languages

Research how at least 4-5 of these languages handle the equivalent concept:

- **Python**: Standard library and built-in types
- **Rust**: Standard library (std), common traits/patterns
- **Go**: Standard library (built-in types, io, errors packages)
- **Java**: Standard library (java.lang, java.util, java.nio)
- **C#/.NET**: System namespace, common patterns
- **Swift**: Foundation, standard library
- **C++**: Standard library (STL, std::)

For each language, note:
- The API or pattern used
- Whether it's built-in or library-level
- Key design decisions (naming, parameter order, error handling)
- What works well and what developers complain about

## Step 4: Identify patterns and themes

Across all sources, identify:
- **Consensus patterns**: What most implementations agree on
- **Divergent approaches**: Where implementations disagree and why
- **Common pitfalls**: Problems that arise from current approaches
- **Naming conventions**: What names are most commonly used for this concept

## Output Format

```markdown
# Prior Art Survey: {topic}

## TC39 Proposals
| Proposal | Stage | Relationship |
|----------|-------|--------------|
| ...      | ...   | ...          |

[Brief description of each related proposal and how it relates]

## JavaScript Ecosystem

### Runtimes
| Runtime | API | Notes |
|---------|-----|-------|
| Node.js | ... | ...   |
| Deno    | ... | ...   |
| Bun     | ... | ...   |

### Web Platform
- [relevant APIs and specs]

### Libraries
| Package | Downloads | API | Notes |
|---------|-----------|-----|-------|
| ...     | ...       | ... | ...   |

## Other Languages

### {Language}
- **API**: ...
- **Example**: (brief code snippet)
- **Notes**: ...

[Repeat for each language]

## Patterns and Themes
- **Consensus**: ...
- **Divergence**: ...
- **Common pitfalls**: ...

## Implications for This Proposal
- [What the prior art suggests about API design choices]
- [What patterns to follow or avoid]
- [What the proposal can learn from other implementations]
```
