---
description: Create a new TC39 proposal repository from the template
---

Create a new TC39 proposal repository from the `tc39/template-for-proposals` GitHub template, clone it locally, and set up the project with customized metadata and build tooling.

## Step 0: Gather inputs

The argument is: `$ARGUMENTS`

Parse the arguments:
- First argument: proposal name in kebab-case (e.g., `structured-clone-v2`). Used as the repo name with `proposal-` prefix.
- Second argument (optional): proposal title in natural language (e.g., "Structured Clone v2"). If not provided, derive from the name by converting kebab-case to Title Case.

If no arguments were provided, ask the user for a proposal name.

Detect the current GitHub user:
!`gh auth status 2>&1`

Parse the authenticated GitHub username from the output above. Do NOT hardcode any username.

Ask the user for any information not provided via arguments:
- **Proposal title** (if not given as second argument)
- **Stage** (default: 0)
- **Contributors** (default: the detected GitHub user's display name, or "TBD")
- **Clone directory**: the parent directory where `proposal-{name}/` will be created. Suggest the parent of the current working directory as a default.

## Step 1: Present the plan and confirm

Before performing ANY actions, present the following summary:

```
## New Proposal Setup Plan

- **GitHub repo**: {user}/proposal-{name}
- **Template**: tc39/template-for-proposals
- **Clone to**: {directory}/proposal-{name}
- **Spec title**: {title}
- **Stage**: {stage}
- **Contributors**: {contributors}

### Actions to be performed:
1. Create GitHub repo from template
2. Clone locally
3. Update ecmarkup and @tc39/ecma262-biblio to latest
4. Customize spec.emu, package.json, and README.md
5. Configure GitHub repo settings (Issues, Pages, workflow permissions)
6. Run initial build to verify
7. Commit and push all customizations
```

**Ask for explicit confirmation before proceeding.** Do NOT perform any of the steps below until the user confirms.

## Step 2: Create and clone the repository

Run in the user's chosen parent directory:
```
gh repo create {user}/proposal-{name} --template tc39/template-for-proposals --public --clone --description "{title} — TC39 Proposal"
```

If creation fails (e.g., name already taken), report the error and stop.

Change into the cloned `proposal-{name}/` directory for all subsequent steps.

## Step 3: Update dependencies

```
npm install --save-dev ecmarkup@latest
npm install --save-dev --save-exact @tc39/ecma262-biblio@latest
```

## Step 4: Customize project files

### spec.emu

Update the `<pre class="metadata">` block:
- `title`: the proposal title
- `stage`: the stage number
- `contributors`: the contributors string

Leave the rest of spec.emu as-is (the template provides a skeleton clause).

### package.json

Update these fields to match the new repo:
- `"name"`: `"proposal-{name}"`
- `"description"`: `"{title} — TC39 Proposal"`
- `"homepage"`: `"https://github.com/{user}/proposal-{name}#readme"`
- `"repository.url"`: `"git+https://github.com/{user}/proposal-{name}.git"`
- `"bugs.url"`: `"https://github.com/{user}/proposal-{name}/issues"`

### README.md

Replace the template README with a proposal skeleton:

```markdown
# {title}

**Stage**: {stage}

**Champions**: {contributors}

You can browse the [ecmarkup output](https://{user}.github.io/proposal-{name}/)
or browse the [source](https://github.com/{user}/proposal-{name}/blob/HEAD/spec.emu).

## Problem

<!-- Describe the problem this proposal solves -->

## Proposal

<!-- Describe the proposed solution -->

## Examples

<!-- Show code examples demonstrating the API -->
```

## Step 5: Configure GitHub repo settings

```bash
# Enable Issues, disable Wiki and Projects, enable branch hygiene
gh api repos/{user}/proposal-{name} -X PATCH \
  -f has_issues=true \
  -f has_wiki=false \
  -f has_projects=false \
  -f delete_branch_on_merge=true \
  -f allow_update_branch=true

# Set workflow permissions to read-write (needed for gh-pages deployment)
gh api repos/{user}/proposal-{name}/actions/permissions/workflow -X PUT \
  -f default_workflow_permissions=write \
  -F can_approve_pull_request_reviews=false
```

Note: GitHub Pages will auto-configure when the `publish-main.yml` workflow first runs and creates the `gh-pages` branch.

## Step 6: Build and verify

```
npm install
npm run build
```

Report whether the build succeeded. If it fails, note the error but continue — the user will edit spec.emu next.

## Step 7: Commit and push

```
git add -A
git commit -m "Initialize proposal: {title}"
git push
```

## Step 8: Report

Summarize what was created:
- Repository URL: `https://github.com/{user}/proposal-{name}`
- Local path: `{directory}/proposal-{name}`
- Spec preview: `https://{user}.github.io/proposal-{name}/` (available after the first GitHub Actions run)
- Build status
- Next steps: edit `spec.emu` and `README.md`, then run `/tc39-build-spec` to verify
