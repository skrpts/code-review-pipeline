---
type: skill
id: structured-data-extraction
title: Structured Data Extraction
description: "Extracts structured fields and data points from unstructured text"
tags: [Tested, Automation]
context_params:
  extraction_fields:
    label: "Fields to Extract"
    description: "The specific fields or data points to extract"
    default: ""
    required: true
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Extracts structured fields and key-value pairs from unstructured text input such as code review comments, commit messages, and PR descriptions.

## When to Use

- Parsing structured information from freeform code review output
- Extracting severity levels, file paths, and line numbers from analysis results
- Converting unstructured findings into actionable data

## Inputs

Unstructured text containing embedded data points

## Outputs

Structured key-value pairs, categorised findings, and extracted metadata
