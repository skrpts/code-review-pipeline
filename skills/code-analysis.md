---
type: skill
id: code-analysis
title: Code Analysis
description: "Analyses code for patterns, complexity, and potential bugs across common languages"
tags: [Production, Tested]
connections:
  - target: llm-service
    type: runs_on
  - target: ollama-local
    type: runs_on
metadata:
  complexity: medium
  avg_tokens: 1200
  supported_languages: [javascript, typescript, python, go, rust, java]
---

## Capability

Performs static analysis of source code to identify structural issues, excessive complexity, and potential bugs. Works across multiple languages by recognising common anti-patterns and language-specific pitfalls.

## Analysis Dimensions

### Complexity Assessment

- **Cyclomatic complexity** — counts independent paths through the code. Functions exceeding a threshold of 10 are flagged for refactoring.
- **Cognitive complexity** — measures how difficult code is to understand, weighting nested control flow and breaks in linear flow.
- **Function length** — flags functions exceeding 50 lines as candidates for decomposition.

### Pattern Detection

- Duplicated logic blocks that should be extracted into shared utilities
- Deeply nested conditionals (more than 3 levels) suggesting the need for early returns or guard clauses
- Unused variables, imports, or dead code paths
- Inconsistent error handling (mixing thrown exceptions with returned error values)
- Mutable state shared across scope boundaries without synchronisation

### Bug Risk Indicators

- Off-by-one errors in loop boundaries and array indexing
- Unchecked null or undefined access on values that may be absent
- Race conditions in asynchronous code (missing awaits, unguarded shared state)
- Type coercion pitfalls (loose equality, implicit conversions)
- Resource leaks (opened file handles, database connections, or event listeners not cleaned up)

## Output Format

Returns a structured assessment with:

1. **Summary** — one-paragraph overview of code quality
2. **Findings** — list of issues, each with severity (critical / warning / info), file location, and explanation
3. **Metrics** — cyclomatic complexity score, function count, average function length
4. **Recommendations** — prioritised list of suggested improvements

## Limitations

This skill performs heuristic analysis, not compilation or execution. It may produce false positives for metaprogramming patterns, generated code, or language-specific idioms it does not recognise. Always cross-reference findings with the actual runtime behaviour.
