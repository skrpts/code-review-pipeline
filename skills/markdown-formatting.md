---
type: skill
id: markdown-formatting
title: Markdown Formatting
description: "Structures and formats documentation as clean, consistent markdown with proper headings, code blocks, and tables"
tags: [Tested, Documentation, Quality]
connections:
  - target: llm-service
    type: runs_on
  - target: documentation-standards
    type: references
---

## Capability

Takes draft documentation and applies consistent markdown formatting: heading hierarchy, fenced code blocks with language hints, parameter tables, cross-reference links, and table of contents generation. Ensures the output is readable both as raw markdown and when rendered. Validates structural consistency (no skipped heading levels, no orphaned links, no empty sections).

## When to Use

- Final formatting pass on generated documentation
- Standardizing markdown structure across multiple documents
- Converting loosely formatted notes into publication-ready docs
- Assembling multiple documentation sections into a single coherent document

## Inputs

Draft documentation content, formatting standards reference

## Outputs

Clean markdown with proper heading hierarchy (H1 → H2 → H3, no skips), fenced code blocks with language tags, consistent parameter tables, and uniform formatting throughout
