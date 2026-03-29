---
type: prompt
id: inline-comments
title: Inline Comments
description: "Generates precise inline code comments for specific issues found during review"
tags: [Production, Code, Review]
connections:
  - target: code-analysis
    type: derived_from
  - target: security-scanning
    type: derived_from
  - target: style-checking
    type: derived_from
metadata:
  output_format: json
  avg_tokens: 400
---

## Purpose

Transforms review findings into inline comments that can be posted directly on specific lines of a pull request. Each comment is self-contained and actionable.

## Prompt

You are generating inline review comments for a pull request. You have received findings from the analysis passes:

- **Code analysis findings:** {{steps.code-analysis.output}}
- **Security scanning findings:** {{steps.security-scanning.output}}
- **Style checking findings:** {{steps.style-checking.output}}

For each finding provided, produce a comment that:

1. **Targets the exact line(s)** — reference the specific file path and line range
2. **States the issue concisely** — one sentence explaining what is wrong
3. **Explains the impact** — why this matters (bug risk, performance, security, readability)
4. **Suggests a fix** — provide a concrete code suggestion where possible

### Output Format

Return a JSON array of comment objects:

```json
[
  {
    "path": "src/auth/validate.ts",
    "start_line": 42,
    "end_line": 45,
    "severity": "high",
    "category": "security",
    "body": "This input is passed directly to the SQL query without parameterisation, creating an injection risk.\n\n**Suggested fix:**\n```typescript\nconst result = await db.query('SELECT * FROM users WHERE id = $1', [userId]);\n```"
  }
]
```

### Comment Style Guidelines

- Be specific. "This could be improved" is not useful. "This function has a cyclomatic complexity of 15; extract the validation logic into a separate function" is.
- One issue per comment. Do not combine unrelated findings.
- Use British English throughout.
- For style issues, include both the current code and the expected form.
- For security issues, include a CWE reference where applicable.
- Mark auto-fixable issues with a `[auto-fixable]` prefix so the author knows a formatter can handle it.
- Keep each comment under 200 words. Link to documentation for complex issues rather than explaining the full context inline.

### Severity Mapping

- **Critical** — blocks merge. Security vulnerabilities, data loss risks, broken functionality.
- **High** — should be fixed before merge. Bugs, significant performance issues, missing error handling.
- **Medium** — recommended improvement. Code clarity, minor performance gains, better patterns.
- **Low** — nitpick. Style preferences, naming suggestions, comment improvements.
