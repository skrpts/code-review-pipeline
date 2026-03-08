---
type: service
id: anthropic-claude
title: Anthropic Claude
description: "Claude API service providing language model capabilities for analysis and generation"
tags: [Production]
connections: []
metadata:
  provider: anthropic
  models: [claude-sonnet-4-20250514, claude-opus-4-20250514]
  auth_type: api_key
  env_var: ANTHROPIC_API_KEY
---

## Service Description

Provides access to Anthropic's Claude language models via the API. Used by all skills in this skrpt for code analysis, pattern recognition, and natural language generation.

## Configuration

### Authentication

Requires an Anthropic API key set as the `ANTHROPIC_API_KEY` environment variable. The key must have access to the models listed in metadata.

### Model Selection

- **claude-sonnet-4-20250514** — default for code analysis and style checking. Balances quality with speed and cost.
- **claude-opus-4-20250514** — used for security scanning and complex decision-making where accuracy is critical.

The workflow selects the appropriate model based on the task. Individual skills do not hard-code a model preference.

### Rate Limits

The pipeline processes files sequentially within each analysis pass to respect rate limits. If a 429 response is received, the workflow backs off exponentially starting at 1 second.

## Endpoint

```
https://api.anthropic.com/v1/messages
```

## Required Headers

```
x-api-key: {ANTHROPIC_API_KEY}
anthropic-version: 2023-06-01
content-type: application/json
```

## Usage Notes

- All requests include a system prompt tailored to the specific skill being invoked
- Token usage is tracked per-review and included in the review summary metadata
- The service does not retain any data from requests — all analysis is stateless
