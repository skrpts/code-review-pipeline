---
type: service
id: llm-service
title: LLM Service
description: "Language model service for analysis, synthesis, and document generation"
tags: [Production, Tested]
connections: []
metadata:
  serviceType: llm
  auth_type: api_key
---

## LLM Service

This skrpt uses a language model for all analytical and generative tasks within the pipeline. The LLM handles structured analysis, content synthesis, document generation, and quality validation across each stage.

### Usage Pattern

The LLM is invoked at each stage of the pipeline. Earlier stages produce structured analysis (frameworks, assessments, breakdowns), while later stages synthesise outputs into coherent documents. The final assembly stage is the most token-intensive, requiring cross-referencing across all previous outputs.

### Configuration

- **Temperature:** 0.3 for structured analysis tasks, 0.5 for narrative and synthesis tasks
- **Max tokens:** 4000 per invocation, 8000–10000 for final assembly stages
- **Context window:** The full pipeline accumulates context. Each stage receives the outputs of all previous stages. The assembly stage requires the full context window.

### Requirements

- A configured LLM provider in skrptiq settings
- Sufficient token quota for the full pipeline
- No external network access required beyond your AI provider's endpoint
