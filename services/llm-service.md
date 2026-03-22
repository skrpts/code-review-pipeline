---
type: service
id: llm-service
title: LLM Service
description: "Language model service providing analysis and generation capabilities"
tags: [Production]
connections: []
metadata:
  serviceType: llm
  auth_type: api_key
---

## Service Description

Provides access to a large language model for code analysis, pattern recognition, and natural language generation. Used by all skills in this skrpt.

## Configuration

The LLM provider is configured in the skrptiq app settings. This skrpt works with any supported provider — no specific vendor is required.

## Token Usage

A typical code review consumes roughly 2,000–8,000 tokens per skill invocation, depending on the size of the diff. A full pipeline run across all skills (code quality, security scanning, review summary, and security report) lands in the range of 10,000–30,000 tokens total. Large PRs with many changed files will sit at the upper end. Trivially small changes (a single-line fix) may use under 5,000 tokens end-to-end.

## Model Considerations

Code review benefits from models with strong reasoning capabilities — the pipeline asks the model to spot subtle logic errors, security flaws, and structural problems, not just summarise text. Weaker or heavily quantised models tend to produce shallow findings. If your provider offers a choice, favour reasoning quality over speed for this skrpt.

## Context Window and Large PRs

When a PR diff exceeds the model's context window, the pipeline processes files individually rather than as a single batch. Each skill receives one file's diff at a time, and the review summary skill then aggregates the per-file findings into a single report. This means very large PRs still produce useful output, but cross-file issues (e.g., a function renamed in one file but not updated in another) may not be caught.

## Which Skills Use This Service

Every skill in the pipeline calls the LLM service:

- **code-quality-analysis** — sends the diff with a code quality system prompt; expects structured findings with severity ratings
- **security-scanning** — sends the diff with a security-focused system prompt; expects vulnerability entries with CWE references
- **review-summary** — receives aggregated findings from the other skills and produces the final review document
- **security-report** — receives security scan output and produces a detailed standalone security report

## Response Format

All skills expect the model to return structured markdown. Each skill's system prompt specifies the exact heading structure and field layout. The pipeline does not parse JSON from the model — it relies on consistent markdown formatting with clearly labelled sections so that downstream skills can extract what they need.
