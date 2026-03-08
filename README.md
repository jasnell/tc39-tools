# TC39 Proposal Tools

AI-assisted commands and skills for authoring, reviewing, and testing TC39 ECMAScript proposals. Works with [OpenCode](https://opencode.ai) and [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## What's included

### Commands

Slash commands invoked with `/tc39-<name>` in the chat interface.

| Command | Description |
|---------|-------------|
| `/tc39-new-proposal <name> [title]` | Create a new proposal repo from the TC39 template, clone locally, customize metadata, and configure GitHub settings |
| `/tc39-build-spec` | Run `ecmarkup` build on `spec.emu`, report lint/build errors with suggested fixes |
| `/tc39-diff-spec [ref1] [ref2]` | Summarize `spec.emu` changes between commits or branches, classifying normative vs editorial |
| `/tc39-review-spec [name\|url]` | 4-phase structured spec review (correctness, editorial, completeness, design). Accepts a local project, a proposal name, or a GitHub URL |
| `/tc39-stage-advance [N]` | Check whether the proposal meets entrance criteria for the next (or specified) TC39 stage |
| `/tc39-compare-api [method]` | Find similar existing ECMA-262 APIs and compare patterns for consistency |
| `/tc39-prep-slides [N]` | Generate a Slidev slide deck for a TC39 plenary presentation |
| `/tc39-add-slides [N]` | Add Slidev presentation infrastructure and scaffold a `slides.md` |
| `/tc39-generate-tests` | Extract all testable assertions from `spec.emu` into `TEST_CASES.md` |
| `/tc39-generate-test262 [TC-IDs]` | Convert `TEST_CASES.md` entries into test262-format `.js` files |
| `/tc39-check-tests` | Cross-reference `TEST_CASES.md` against `test/` files, report coverage gaps |
| `/tc39-lint-editorial` | Quick editorial conventions check — phrasing, algorithm style, formatting |
| `/tc39-update-deps` | Update `ecmarkup` and `@tc39/ecma262-biblio` to latest versions, verify build |
| `/tc39-open-issues [repo]` | Fetch and categorize open GitHub issues for a proposal |
| `/tc39-cross-proposal [dirs]` | Detect shared abstract operations and potential conflicts across proposals |
| `/tc39-research [topic]` | Research prior art across languages, platforms, and the JS ecosystem |
| `/tc39-view-agenda [YYYY/MM]` | View the full TC39 plenary meeting agenda with all proposals, links, and schedule constraints |
| `/tc39-add-to-agenda [stage]` | Add the current proposal to the next meeting agenda via PR to `tc39/agendas` |
| `/tc39-update-agenda` | Update an existing agenda entry (slides link, timebox, etc.) via PR |
| `/tc39-add-schedule-constraint` | Add a schedule constraint to the meeting agenda via PR |
| `/tc39-delegate-notes <name\|TLA>` | Look up a TC39 delegate and summarize their recent meeting activity |
| `/tc39-proposal-notes [name]` | Summarize the most recent TC39 plenary discussion for a proposal |

### Skills

Skills are loaded automatically when relevant context is detected.

| Skill | Triggers when... |
|-------|------------------|
| `tc39-proposal` | Editing, reviewing, or authoring `.emu` files; discussing ecmarkup syntax, ECMA-262 editorial conventions, or TC39 stage requirements |
| `tc39-sync-tests` | `spec.emu` is modified in a project that has a `TEST_CASES.md`; reports new, updated, and removed testable assertions |

## Prerequisites

### TC39 Spec MCP Server (recommended)

Several commands use MCP tools (`tc39_search_spec`, `tc39_get_spec_section`, `tc39_list_proposals`, `tc39_get_proposal`, `tc39_search_notes`, `tc39_search_test262`, `tc39_get_agenda`, `tc39_lookup_delegate`) to look up ECMA-262 spec sections, search meeting notes, browse test262 tests, fetch TC39 proposals, view meeting agendas, and resolve delegate names. These are provided by the TC39 Spec MCP server:

**Server URL**: `https://tc39-spec-mcp.jasnell.workers.dev/mcp`

See the [Installation](#installation) section below for how to configure the MCP server in each tool.

### Proposal repo setup

Commands assume you're running them from inside a TC39 proposal repository with:
- `spec.emu` — the ecmarkup source file
- `package.json` with a `build` script that runs `ecmarkup` (standard TC39 proposal template)

## Installation

### OpenCode

**1. Install commands** — copy or symlink into `~/.config/opencode/commands/`:

```bash
# Copy
cp commands/*.md ~/.config/opencode/commands/

# Or symlink (updates automatically when you pull)
for f in commands/*.md; do
  ln -sf "$(pwd)/$f" ~/.config/opencode/commands/
done
```

**2. Install skills** — copy or symlink into `~/.config/opencode/skill/`:

```bash
# Copy
cp -r skills/* ~/.config/opencode/skill/

# Or symlink
for d in skills/*/; do
  name=$(basename "$d")
  ln -sfn "$(pwd)/$d" ~/.config/opencode/skill/$name
done
```

**3. Configure the MCP server** — add to `~/.config/opencode/opencode.json`:

```jsonc
{
  "mcp": {
    "tc39": {
      "type": "remote",
      "url": "https://tc39-spec-mcp.jasnell.workers.dev/mcp"
    }
  }
}
```

### Claude Code

Claude Code uses the [Agent Skills](https://agentskills.io) standard. Commands go in `~/.claude/commands/` (legacy path, still fully supported) and skills go in `~/.claude/skills/`.

**1. Install commands**:

```bash
mkdir -p ~/.claude/commands

# Copy
cp commands/*.md ~/.claude/commands/

# Or symlink
for f in commands/*.md; do
  ln -sf "$(pwd)/$f" ~/.claude/commands/
done
```

**2. Install skills**:

```bash
mkdir -p ~/.claude/skills

# Copy
cp -r skills/* ~/.claude/skills/

# Or symlink
for d in skills/*/; do
  name=$(basename "$d")
  ln -sfn "$(pwd)/$d" ~/.claude/skills/$name
done
```

**3. Configure the MCP server** — add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "tc39": {
      "type": "url",
      "url": "https://tc39-spec-mcp.jasnell.workers.dev/mcp"
    }
  }
}
```

### Compatibility notes

The commands were authored in OpenCode format. Most features work identically in both tools:

| Feature | OpenCode | Claude Code | Notes |
|---------|----------|-------------|-------|
| `$ARGUMENTS` | Supported | Supported | Identical behavior |
| `$1`, `$2`, ... | 1-indexed | 0-indexed (`$0`, `$1`) | Commands only use `$ARGUMENTS`, so no issue |
| `!`command`` | Shell injection | Shell injection | Identical — runs before prompt is sent |
| `@file` | File reference | Not built-in | Claude reads files via tools; works in practice |
| `subtask: true` | Runs as subtask | Ignored | Use `context: fork` in Claude; see below |
| `description:` | Shown in TUI | Shown in menu | Identical |

**For Claude Code users**: commands with `subtask: true` in the frontmatter (`tc39-review-spec`, `tc39-stage-advance`, `tc39-compare-api`, `tc39-check-tests`, `tc39-open-issues`, `tc39-cross-proposal`, `tc39-research`, `tc39-delegate-notes`, `tc39-proposal-notes`) are intended to run in isolation. To get the same behavior in Claude Code, change `subtask: true` to `context: fork` in the frontmatter of those files after copying.

## Typical workflow

```
0.  /tc39-new-proposal my-idea   # create a new proposal repo from the TC39 template
1.  /tc39-research               # survey prior art across languages and platforms
2.  Edit spec.emu
3.  /tc39-build-spec             # check for lint/build errors
4.  /tc39-lint-editorial         # quick editorial conventions check
5.  /tc39-review-spec            # thorough 4-phase review
6.  /tc39-compare-api            # check consistency with ECMA-262 patterns
7.  /tc39-cross-proposal         # detect shared AOs across your proposals
8.  /tc39-diff-spec main         # summarize changes since main branch
9.  /tc39-generate-tests         # extract testable assertions → TEST_CASES.md
10. /tc39-generate-test262       # generate test262 .js files from TEST_CASES.md
11. /tc39-check-tests            # verify test coverage
12. /tc39-open-issues            # review open GitHub issues
13. /tc39-stage-advance          # check readiness for next stage
14. /tc39-add-slides             # add Slidev infrastructure (if not present)
15. /tc39-prep-slides            # generate plenary presentation
16. /tc39-add-to-agenda          # add proposal to next meeting agenda via PR
17. /tc39-view-agenda            # review the full meeting agenda
18. /tc39-add-schedule-constraint # add your schedule constraints via PR
19. /tc39-proposal-notes         # review recent plenary discussion of your proposal
20. /tc39-delegate-notes KG      # look up a delegate's recent positions and activity
```

The `tc39-sync-tests` skill activates automatically after step 1 whenever `TEST_CASES.md` exists, keeping test case descriptions in sync with spec changes.

## License

MIT
