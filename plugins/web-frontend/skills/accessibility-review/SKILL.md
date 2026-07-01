---
name: accessibility-review
description: Reviews frontend markup/components for accessibility issues (semantic HTML, ARIA usage, color contrast, keyboard navigation, focus management, alt text). Use when the user asks for an accessibility/a11y review, WCAG compliance check, or "is this accessible" question about UI code.
---

# Accessibility Review

Audit the given component(s) or page(s) for accessibility issues and report concrete, actionable fixes.

## Steps

1. Identify scope: if the user names specific files, review those. Otherwise ask which component/page/directory to review, or default to files changed in the current diff (`git diff`) if one exists.

2. Read the target files and check for common issues:
   - **Semantic HTML**: divs/spans used where a native element (`button`, `nav`, `header`, `main`, `label`, etc.) would provide built-in semantics and behavior.
   - **ARIA**: missing or incorrect `role`, `aria-label`, `aria-labelledby`, `aria-describedby`; redundant ARIA on elements that already have implicit roles; ARIA attributes that reference non-existent IDs.
   - **Images/icons**: missing `alt` text on `<img>`, decorative images not marked `alt=""`, icon-only buttons without an accessible name.
   - **Forms**: inputs without associated `<label>`, missing error message association, no visible focus indicator.
   - **Keyboard navigation**: click handlers on non-interactive elements without a matching `keydown`/`keypress` handler and `tabIndex`; focus traps in modals; illogical tab order.
   - **Color contrast**: hardcoded colors that look like they'd fail WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text) — flag for manual verification with a contrast checker since exact rendered colors can't always be determined from source.
   - **Headings**: skipped heading levels, missing page-level `h1`.

3. For each issue found, report:
   - File and line number.
   - What's wrong and which WCAG guideline/success criterion it relates to (when applicable).
   - A concrete code-level fix.

4. Do not silently rewrite files. Present findings first; only apply fixes if the user confirms or explicitly asked for fixes to be applied.

5. Keep the report concise — group minor/nitpick items separately from significant/blocking issues.
