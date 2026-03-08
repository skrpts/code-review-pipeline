---
type: prompt
id: approval-decision
title: Approval Decision
description: "Makes an approve or request-changes decision with structured reasoning"
tags: [Needs Review]
connections:
  - target: code-analysis
    type: derived_from
  - target: security-scanning
    type: derived_from
  - target: style-checking
    type: derived_from
metadata:
  output_format: json
  avg_tokens: 300
---

## Purpose

Takes the full set of review findings and produces a final decision: approve the pull request or request changes. The decision is structured and auditable, with clear reasoning tied to specific findings.

## Prompt

You are making the final review decision on a pull request. Based on all findings from code analysis, security scanning, and style checking, produce a decision.

### Decision Criteria

**Approve** if all of the following are true:

- No critical-severity findings
- No high-severity security findings
- No more than 3 high-severity code quality findings
- All high-severity findings have straightforward fixes (not architectural issues)

**Request Changes** if any of the following are true:

- One or more critical-severity findings of any category
- One or more high-severity security findings
- More than 3 high-severity code quality findings
- Architectural issues that require significant rework

**Comment Only** (neither approve nor block) if:

- Only medium and low severity findings remain
- The issues are genuine but do not justify blocking the merge
- The findings are primarily style or documentation related

### Output Format

```json
{
  "decision": "approve | request_changes | comment",
  "confidence": 0.85,
  "reasoning": "Brief explanation of why this decision was reached",
  "blocking_findings": [
    {
      "id": "finding-reference",
      "severity": "critical",
      "category": "security",
      "summary": "SQL injection in user input handler"
    }
  ],
  "conditions": [
    "Address the SQL injection vulnerability in src/auth/validate.ts",
    "Add error handling to the file upload endpoint"
  ],
  "positive_notes": [
    "Comprehensive test coverage for the new API endpoints",
    "Clear separation of concerns in the service layer"
  ]
}
```

### Decision Guidelines

- Be decisive. A review that always requests changes is as useless as one that always approves.
- The confidence score (0.0 to 1.0) reflects how certain you are about the decision. Lower confidence when findings are ambiguous or when context is missing.
- Conditions are what must change before re-review. Keep this list short and specific.
- Positive notes are not optional — every review should acknowledge what was done well.
- Use British English throughout.
