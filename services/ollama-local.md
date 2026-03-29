---
type: service
id: ollama-local
title: Ollama Local
description: "Local LLM inference via Ollama for private, offline code review"
tags: [Production, Code, Review]
connections: []
metadata:
  serviceType: llm
  auth_type: none
---

## Service Description

Provides local LLM inference through Ollama, enabling fully offline code review. Useful when working with proprietary codebases where sending code to external APIs is not permitted.

## Configuration

1. Install Ollama from [ollama.com](https://ollama.com)
2. Pull a code-capable model: `ollama pull codellama` or `ollama pull deepseek-coder`
3. Configure skrptiq to use the Ollama provider in app settings

## Model Recommendations

For code review, use a model with strong code understanding:
- **CodeLlama 13B+** — good balance of quality and speed for code analysis
- **DeepSeek Coder 33B** — stronger reasoning but requires more RAM
- **Mixtral 8x7B** — broad capability, handles multiple languages well

Smaller models (7B and under) tend to miss subtle issues and produce shallow reviews. For security scanning in particular, prefer larger models.

## Limitations

- No internet access — cannot check CVE databases or fetch external references
- Context windows are typically smaller than cloud models
- Quality depends heavily on the chosen model and available hardware
- First inference after model load is slower (cold start)

## When to Use

- Reviewing proprietary or sensitive code that cannot leave the network
- Working offline or in air-gapped environments
- Reducing API costs for frequent, low-stakes reviews
- Testing and experimentation before committing to cloud API usage
