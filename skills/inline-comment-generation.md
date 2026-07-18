---
type: skill
id: inline-comment-generation
title: Inline Comments
description: "Turns analysis findings into precise, self-contained inline review comments anchored to specific diff lines"
tags: [Production, Code, Review]
connections:
  - target: llm-service
    type: runs_on
metadata:
  complexity: medium
  avg_tokens: 400
---

## Capability

Converts the combined findings from security scanning and style checking into per-line review comments that can be posted directly on a pull request diff. Each comment is anchored to an exact file and line range, states a single issue, explains its impact, and offers a concrete fix.

### Method

1. **Anchor:** Map each finding to the precise file path and line range it affects, so the comment lands on the right diff hunk.
2. **State one issue per comment:** Split combined findings so that no comment mixes unrelated concerns.
3. **Explain and remediate:** Give the reason the issue matters (bug risk, security, readability, performance) and a concrete code suggestion where one applies.
4. **Classify:** Tag severity and category, and mark auto-fixable style issues so the author knows a formatter can resolve them.

### Output Structure

A JSON array of comment objects, each carrying the file path, start and end line, severity, category, and a self-contained comment body. The array feeds the review summary and the final approval decision downstream.
