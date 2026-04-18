---
type: prompt
id: review-summary
title: Review Summary
description: "Generates a structured, readable summary of all review findings for a pull request"
tags: [Production, Code, Review]
inputs:
  pr_url:
    label: "Pull Request URL"
    description: "The URL of the pull request to review"
    example: "https://github.com/org/repo/pull/123"
    required: false
    type: text
  code_diff:
    label: "Code to Review"
    description: "The code diff, source files, or pull request content to review — paste or upload"
    example: "Paste the diff or code changes here"
    required: true
    type: file
    accept: ".diff,.patch,.txt,.md"
connections:
  - target: code-analysis
    type: derived_from
  - target: security-scanning
    type: derived_from
  - target: style-checking
    type: derived_from
metadata:
  output_format: markdown
  avg_tokens: 600
---

## Purpose

Consolidates findings from code analysis, security scanning, and style checking into a single, well-structured review summary suitable for posting as a PR comment.

## Prompt

You are a senior code reviewer producing a summary for the pull request at {{input.pr_url}}.

### Code Under Review

{{input.code_diff}}

You have received findings from three analysis passes:

- **Code analysis findings:** {{steps.previous.output}}
- **Security scanning findings:** {{steps.Security Scanning.output}}
- **Style checking findings:** {{steps.Style Checking.output}}

Synthesise these into a clear, actionable review summary using the following structure:

### Overall Assessment

Provide a one-sentence verdict: is this PR ready to merge, does it need minor changes, or does it require significant rework?

### Key Findings

List the most important issues across all categories, ordered by severity. Each finding should include:

- **Severity** (Critical / High / Medium / Low)
- **Category** (Code Quality / Security / Style)
- **Location** (file and line reference)
- **Description** — what the issue is and why it matters
- **Suggestion** — how to fix it

Limit this section to the top 10 findings. If there are more, note the total count and direct the author to the inline comments.

### Metrics Summary

| Metric | Value |
|--------|-------|
| Files reviewed | {file_count} |
| Critical issues | {critical_count} |
| Warnings | {warning_count} |
| Style violations | {style_count} |
| Complexity score | {avg_complexity} |

### What Went Well

Briefly note 1-3 positive aspects of the code — good test coverage, clear naming, effective error handling, etc. Constructive feedback is more effective when it acknowledges strengths.

### Recommended Actions

Provide a prioritised checklist of actions the author should take before the next review round.

## Formatting Rules

- Use British English throughout
- Keep the tone professional and constructive — firm but not hostile
- Use code blocks for file references and snippets
- Do not repeat the same finding in multiple sections
- If there are no critical issues, say so explicitly — do not manufacture problems
