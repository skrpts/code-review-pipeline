---
type: skill
id: approval-decision-making
title: Approval Decision
description: "Weighs the full set of review findings and issues a final approve, request-changes, or comment verdict"
tags: [Production, Code, Review]
connections:
  - target: llm-service
    type: runs_on
metadata:
  complexity: medium
  avg_tokens: 300
---

## Capability

Takes the consolidated review summary alongside the security, style, and inline findings and produces a single, auditable decision: approve, request changes, or comment only. The verdict is tied to explicit criteria rather than gut feel, so it is reproducible and reviewable.

### Method

1. **Gather:** Read the review summary and the underlying security, style, and inline findings as one evidence base.
2. **Apply criteria:** Test the findings against fixed decision rules — critical or high-severity security issues block; a small number of straightforward high-severity items may still pass with conditions; style-only findings comment rather than block.
3. **Decide with confidence:** Emit the verdict, a confidence score, the blocking findings, the conditions for re-review, and the positive notes that acknowledge what was done well.

### Output Structure

A JSON decision object carrying the verdict, confidence, reasoning, blocking findings, re-review conditions, and positive notes. This is the terminal judgement attached to the posted review.
