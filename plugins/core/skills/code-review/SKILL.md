---
name: code-review
description: Reviews a diff or set of files for correctness bugs, unclear logic, and missed edge cases, independent of language or stack. Use when the user asks for a general code review and no more specific frontend/backend review skill applies.
---

# Code Review

Perform a general-purpose review of code changes, focused on correctness rather than style.

## Steps

1. Identify scope: if the user names specific files, review those. Otherwise default to the current diff (`git diff`, or `git diff <base>...HEAD` if working on a branch).

2. Read each changed file in full (not just the diff hunk) when the change touches logic that depends on surrounding context — e.g. a modified function whose callers or shared state live nearby.

3. Look for:
   - Logic bugs: off-by-one errors, incorrect conditionals, wrong operator precedence, inverted booleans.
   - Missed edge cases: empty inputs, null/undefined, zero, negative numbers, concurrent access, network failures.
   - Error handling: swallowed exceptions, missing error propagation, resources not cleaned up on failure paths.
   - Consistency: does the change match patterns already established elsewhere in the codebase, or does it silently diverge?

4. For each finding, report file and line number, what's wrong, a concrete failure scenario (input/state that triggers it), and a suggested fix.

5. Do not report style nits (formatting, naming) unless the user asks for that separately — stay focused on correctness. Do not rewrite the code; describe the fix unless asked to apply it.

6. If nothing significant is found, say so briefly rather than inventing minor issues to fill space.
