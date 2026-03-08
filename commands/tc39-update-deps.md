---
description: Update ecmarkup and biblio dependencies to latest versions
---

Update `ecmarkup` and `@tc39/ecma262-biblio` to their latest versions in the current proposal repository.

## Step 0: Verify prerequisites

Verify the current directory contains a `package.json` file. If not, report an error and stop.

Verify that npm is installed and available. If not, report an error and stop.

## Step 1: Check current and latest versions

Read @package.json and extract current versions of `ecmarkup` and `@tc39/ecma262-biblio`.

Check latest available versions:
!`npm view ecmarkup version 2>/dev/null`
!`npm view @tc39/ecma262-biblio version 2>/dev/null`

## Step 2: Present the plan and confirm

Present what will change:

| Package | Current | Latest |
|---------|---------|--------|
| ecmarkup | {current} | {latest} |
| @tc39/ecma262-biblio | {current} | {latest} |

If both are already at the latest version, report that no update is needed and stop.

**Ask for explicit confirmation before proceeding.** Do NOT run npm install until the user confirms.

## Step 3: Update

```
npm install --save-dev ecmarkup@latest
npm install --save-dev --save-exact @tc39/ecma262-biblio@latest
```

## Step 4: Verify

Run the build to confirm the update does not break the spec:
```
npm run build 2>&1
```

Report:
- Which packages were updated and to which versions
- Whether the build passed or failed
- Any new warnings introduced by the update
