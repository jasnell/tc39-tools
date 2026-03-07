---
description: Build the spec and report ecmarkup lint/build errors
---

Build the ecmarkup spec in the current proposal repository and report any errors or warnings.

First, load the `tc39-proposal` skill for ecmarkup syntax reference.

Verify the current directory contains a `spec.emu` file, which is the source for the spec. If not, report an error and stop.

Verify the current directory contains a `package.json` file with a build script defined. If not, report an error and stop.

Verify that npm is installed and available in the environment. If not, report an error and stop.

Verify that npm dependencies are installed. If not, report an error and stop.

Run the build:
!`npm run build 2>&1`

Analyze the output and present a structured report:

1. **Build Status**: Did the build succeed or fail?
2. **Lint Errors**: List each lint error with:
   - The file and line/column if available
   - The rule or error code
   - A plain-English explanation of what is wrong
   - A suggested fix with the corrected spec text
3. **Lint Warnings**: Same format as errors but lower priority
4. **Summary**: Total counts of errors and warnings

If there are fixable errors, ask whether I want you to apply the fixes to spec.emu.

If the build succeeded with no errors or warnings, just confirm success and note the output location.
