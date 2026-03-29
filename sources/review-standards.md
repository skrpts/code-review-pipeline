---
type: source
id: review-standards
title: Review Standards
description: "Coding standards and review criteria used as the baseline for all analysis passes"
tags: [Production, quality:review, quality:standards, technical:code]
connections: []
metadata:
  last_updated: "2026-03-01"
  applies_to: [javascript, typescript, python, go]
---

## Purpose

This document defines the coding standards and review criteria that the security scanning and style checking skills reference during analysis. It serves as the single source of truth for what constitutes acceptable code in a review.

## General Principles

1. **Clarity over cleverness** — code should be immediately understandable to a competent developer unfamiliar with the specific codebase.
2. **Explicit over implicit** — prefer named constants over magic numbers, explicit types over inferred ones in public interfaces, and explicit error handling over silent failures.
3. **Minimal surface area** — export only what is needed. Keep functions focused on a single responsibility. Prefer composition over inheritance.
4. **Fail loudly** — errors should be visible, not swallowed. Logging an error and continuing is only acceptable when there is a defined recovery path.
5. **Test what matters** — critical paths, edge cases, and error handling must have tests. Trivial getters and framework boilerplate need not.

## Security Standards

### Mandatory (block merge if violated)

- No hardcoded credentials, API keys, or secrets in source code
- All user input must be validated and sanitised before use in queries, commands, or rendered output
- Authentication and authorisation checks must be present on all protected endpoints
- Cryptographic operations must use current, recommended algorithms (AES-256, SHA-256 minimum, Argon2 for password hashing)
- Dependencies with known critical vulnerabilities must be updated or patched

### Recommended (flag but do not block)

- Use parameterised queries exclusively — no string concatenation for SQL
- Apply the principle of least privilege to all service accounts and API tokens
- Log security-relevant events (authentication attempts, authorisation failures, input validation rejections)
- Set appropriate security headers on all HTTP responses
- Use HTTPS for all external communication

## Code Quality Standards

### Complexity Thresholds

| Metric | Warning | Block |
|--------|---------|-------|
| Cyclomatic complexity (per function) | > 10 | > 20 |
| Cognitive complexity (per function) | > 15 | > 30 |
| Function length (lines) | > 50 | > 100 |
| File length (lines) | > 300 | > 500 |
| Nesting depth | > 3 | > 5 |
| Function parameters | > 4 | > 7 |

### Error Handling

- All async operations must have error handling (try/catch, .catch(), or error callback)
- Error messages must be descriptive and include context (what operation failed, with what input)
- Do not catch exceptions only to log and re-throw without adding information
- Use typed errors or error codes where the caller needs to distinguish between failure modes

### Naming

- Functions: verb + noun (e.g., `fetchUserProfile`, `validate_input`, `BuildResponse`)
- Booleans: interrogative form (e.g., `isValid`, `has_permission`, `shouldRetry`)
- Collections: plural nouns (e.g., `users`, `file_paths`, `pendingRequests`)
- Constants: descriptive UPPER_SNAKE_CASE (e.g., `MAX_RETRY_ATTEMPTS`, `DEFAULT_TIMEOUT_MS`)
- Avoid abbreviations unless universally understood (e.g., `id`, `url`, `http` are fine; `usr`, `mgr`, `cfg` are not)

## Style Standards

### Universal Rules

- Consistent indentation throughout the file (do not mix tabs and spaces)
- Maximum line length: 100 characters (120 for languages where this is conventional)
- One blank line between function definitions; two blank lines between class definitions
- Trailing whitespace is not permitted
- Files must end with a newline character
- Imports grouped by: standard library, third-party packages, local modules

### Language-Specific

These supplement, not override, the project's own configuration files (.eslintrc, pyproject.toml, etc.).

**TypeScript/JavaScript:**
- Prefer `const` over `let`; never use `var`
- Use strict equality (`===`) exclusively
- Prefer async/await over raw promises
- Destructure objects and arrays where it improves clarity

**Python:**
- Follow PEP 8 with a 100-character line limit
- Use type hints for all function signatures
- Prefer f-strings over `.format()` or `%` formatting
- Use context managers (`with` statements) for resource management

**Go:**
- Follow `gofmt` formatting without exception
- Handle all errors — do not use `_` to discard error returns
- Use meaningful variable names (not single-letter outside of short loops)
- Keep interfaces small (1-3 methods)

## Review Process Standards

- Every finding must include a concrete suggestion, not just a complaint
- Distinguish between blocking issues (must fix) and suggestions (could improve)
- Acknowledge positive aspects of the code — reviews that only criticise are demotivating
- Do not flag existing issues in unchanged code — scope the review to the diff
- If unsure whether something is a problem, frame it as a question rather than a demand
