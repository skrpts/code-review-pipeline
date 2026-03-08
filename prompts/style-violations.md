---
type: prompt
id: style-violations
title: Style Violations
description: "Lists style guide violations with specific fix suggestions and auto-fix indicators"
tags: [Tested]
connections:
  - target: style-checking
    type: derived_from
metadata:
  output_format: markdown
  avg_tokens: 500
---

## Purpose

Formats style checking results into a clear, scannable list of violations that the author can work through systematically. Distinguishes between auto-fixable issues (run a formatter) and manual fixes (requires human judgement).

## Prompt

You are reporting style guide violations found during a code review. Organise the findings into two sections: issues that can be fixed automatically and issues that require manual correction.

### Auto-Fixable Issues

These can be resolved by running the project's configured formatter or linter with auto-fix enabled.

For each issue, list:

- **File** — path and line number
- **Rule** — which style rule was violated
- **Current** — what the code looks like now
- **Expected** — what the formatter would produce

If there are many auto-fixable issues (more than 10), summarise them by category and suggest running the formatter rather than listing each one individually.

### Manual Fixes Required

These require the author's judgement and cannot be resolved by tooling alone.

For each issue, provide:

- **File** — path and line number
- **Rule** — which guideline was violated
- **Issue** — what is wrong
- **Suggestion** — how to fix it, with a code example if helpful
- **Rationale** — brief explanation of why this rule exists

### Summary Table

| Category | Count | Auto-fixable |
|----------|-------|--------------|
| Naming | {n} | {y/n} |
| Formatting | {n} | {y/n} |
| Organisation | {n} | {y/n} |
| Documentation | {n} | {y/n} |
| Idiom | {n} | {y/n} |

### Recommendations

If the project lacks a formatter configuration, suggest setting one up. If violations are concentrated in a specific area, note the pattern so the author can address the root cause rather than fixing symptoms individually.

## Formatting Rules

- Use British English throughout
- Keep examples short — show only the relevant lines, not full functions
- Group violations by file when multiple issues affect the same file
- Be precise about line numbers — vague references waste the author's time
