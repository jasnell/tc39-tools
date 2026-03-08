---
description: Add Slidev presentation infrastructure to the proposal
---

Add Slidev slide deck infrastructure to the current TC39 proposal repository and scaffold an initial `slides.md`.

## Step 0: Verify prerequisites

Verify the current directory contains `spec.emu` and `package.json`. If not, report an error and stop.

Read @spec.emu to get the proposal title, current stage, and contributors from the metadata block.

Check if slides.md already exists:
!`ls slides.md 2>/dev/null`

If `slides.md` already exists, ask the user whether to overwrite it with a fresh scaffold or abort (suggest using `/tc39-prep-slides` to regenerate content instead).

## Step 1: Determine target stage

The argument is: `$ARGUMENTS`

If an argument is provided, treat it as the target stage for the presentation. Otherwise, infer the next stage from the current stage in spec.emu:
- Stage 0 → Stage 1 request
- Stage 1 → Stage 2 request
- Stage 2 → Stage 2.7 request
- Stage 2.7 → Stage 3 request
- Stage 3 → Stage 4 request

## Step 2: Present the plan and confirm

Present the plan:
- **Add devDependencies**: `@slidev/cli`, `@slidev/theme-default`, `playwright-chromium`
- **Add npm scripts**: `slides`, `slides:build`
- **Create**: `slides.md` (scaffold for Stage {target} presentation)

**Ask for explicit confirmation before proceeding.** Do NOT modify any files until the user confirms.

## Step 3: Update package.json

Read @package.json and add:

In `devDependencies`:
- `"@slidev/cli": "^52.11.5"`
- `"@slidev/theme-default": "^0.25.0"`
- `"playwright-chromium": "^1.58.2"`

In `scripts`:
- `"slides": "slidev slides.md"`
- `"slides:build": "slidev build slides.md"`

Preserve all existing content in package.json. Only add the new entries.

## Step 4: Create slides.md

Create a minimal `slides.md` scaffold:

```markdown
---
theme: default
title: {title from spec.emu}
info: |
  TC39 Proposal — Stage {target stage} Request
  {contributors from spec.emu}
highlighter: shiki
transition: slide-left
mdc: true
---

# {title}

Stage {target} Request

{contributors}

---

## Recap

<!-- What does this proposal do? -->

---

## API Overview

<!-- Table of proposed methods/operations -->

---

## Stage {target} Request

<!-- Restate entrance criteria and how they are met -->
```

## Step 5: Install and report

```
npm install
```

Report:
- Files created and modified
- Suggest running `/tc39-prep-slides {target}` to fill in the slide content
- Suggest running `npm run slides` to preview the deck locally
