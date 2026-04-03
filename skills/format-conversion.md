---
type: skill
id: format-conversion
title: Format Conversion
description: "Converts content between output formats: markdown, plain text, email HTML, JSON, CSV, presentation outline"
tags: [Production, Quality]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Converts structured or unstructured content into a specified output format. Handles the formatting, structure, and conventions of the target format while preserving the source content's meaning and organisation.

This replaces per-workflow formatting nodes (markdown-formatting, format-documentation, etc.) with a single shared skill that adapts to the requested output format.

## Supported Formats

| Format | Output |
|--------|--------|
| **Markdown** | Headings, lists, tables, code blocks, emphasis. Clean, readable markdown following CommonMark. |
| **Plain text** | Stripped formatting, preserved structure via whitespace and indentation. Suitable for email bodies or terminal output. |
| **Email HTML** | Inline-styled HTML for email clients. Tables for layout, no external stylesheets. |
| **JSON** | Structured JSON from unstructured text. Extracts fields, arrays, and nested objects. |
| **CSV** | Tabular data extraction. Headers from context, one row per record. |
| **Presentation outline** | Slide-by-slide structure with titles, bullet points, and speaker notes. |

## When to Use

- As the final stage of any pipeline that produces formatted output
- When the same content needs to be delivered in multiple formats
- To standardise output formatting across different workflows

## Inputs

- Source content (any format)
- Target format (one of the supported formats above)
- Optional: format-specific instructions (e.g. heading level, table style, JSON schema)

## Outputs

Content formatted in the requested output format, ready for use.
