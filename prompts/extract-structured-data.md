---
type: prompt
id: extract-structured-data
title: "Extract Structured Data"
description: "Extracts structured fields and key-value pairs from unstructured text"
tags: [Production, Automation]
connections:
  - target: structured-data-extraction
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Drives the structured data extraction skill.

## Prompt

You are a data extraction specialist. Extract structured information from the unstructured text below.

### Text to Process

{{steps.previous.output}}

### Fields to Extract

{{step.context.extraction_fields}}

### Instructions

1. Read the text carefully and identify all instances of the requested fields
2. Extract each field value exactly as stated in the text — do not infer or fabricate
3. If a field is not present in the text, mark it as "Not found"
4. If a field has multiple values, list all of them

### Output Format

Return the extracted data as a structured list:

| Field | Value | Confidence | Source Quote |
|-------|-------|------------|-------------|
| [field name] | [extracted value] | High / Medium / Low | [relevant text snippet] |

Flag any ambiguous extractions where the text could be interpreted differently.

## Formatting Rules

- Use British English throughout
- Be specific and actionable — no vague recommendations
- Structure output clearly with headings, tables, or lists as appropriate
