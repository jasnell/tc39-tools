---
description: Generate a Slidev slide deck for a TC39 plenary presentation
---

Generate or update a Slidev slide deck (`slides.md`) for presenting this proposal at a TC39 plenary meeting.

## Step 0: Gather context

Load the `tc39-proposal` skill for TC39 process reference.

Read @spec.emu to get the proposal title, current stage, contributors, and all proposed APIs.

Check if @slides.md already exists. If it does, read it to understand the existing structure and what was previously presented.

Get recent spec changes if there's git history:
!`git log --oneline -20 -- spec.emu 2>/dev/null`

Check for open issues:
!`git log --oneline -10 2>/dev/null`

Check the repo for a README:
!`ls README.md 2>/dev/null && head -5 README.md 2>/dev/null`

## Step 1: Determine the presentation type

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the target stage advancement (e.g., `/tc39-prep-slides 2` means "preparing to request Stage 2"). If not provided, infer from the current stage in spec.emu (presenting for the next stage).

Supported presentation types:
- **Stage 1 request**: Focus on problem motivation, solution space, why the committee should invest time
- **Stage 2 request**: Focus on API design, spec text status, key design decisions, open questions
- **Stage 2.7 request**: Focus on spec completeness, editorial quality, reviewer feedback addressed, remaining issues
- **Stage 3 request**: Focus on test coverage, implementation experience, resolved issues since 2.7
- **Stage 4 request**: Focus on implementation status, test262 results, field experience
- **Update (no advancement)**: Focus on changes since last presented, open question resolution, new issues

## Step 2: Generate the slide deck

Use Slidev markdown format. The deck should follow this structure:

```markdown
---
theme: default
title: [Proposal Title]
info: |
  TC39 Proposal — Stage [N] [Advancement/Update]
  [Author]
highlighter: shiki
transition: slide-left
mdc: true
---
```

### Required slides (in order):

1. **Title slide** — proposal name, "Proposing advancement to Stage N" or "Status Update", author name
2. **Recap** (if not first presentation) — 1-2 slides reminding the committee what this proposal does, with a concise code example showing the problem and the solution
3. **API Overview** — table of proposed methods with inputs/outputs
4. **Per-method detail** — one slide per method showing usage example and key behaviors
5. **Changes since last presented** (if applicable) — bullet list of normative and editorial changes. Use diff-spec logic: summarize what changed and why.
6. **Open Issues** — one slide per significant open issue, with the issue number, who raised it, the question, and options being considered. End each with what you're seeking from the committee.
7. **Resolved Issues** (if applicable) — brief list of issues closed since last presented
8. **Summary table** — all open issues with status and what's being sought
9. **Stage advancement request** — restate the entrance criteria and show how they're met

### Formatting conventions:
- Use `---` between slides
- Use `layout: section` for section dividers
- Code examples should be concise (5-10 lines max)
- Tables for comparisons and summaries
- Link to GitHub issues with `[#N](url)`
- Keep each slide scannable in under 30 seconds

## Step 3: Write the file

Write the result to `slides.md` in the current directory. If the file already exists, replace it entirely (the old content was used for reference only).

Report a summary of what's in the deck: number of slides, which open issues are covered, and any suggestions for what to prepare for committee Q&A.
