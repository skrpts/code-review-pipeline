---
type: skill
id: style-checking
title: Style Checking
description: "Validates code against style guidelines, naming conventions, and formatting standards"
tags: [Production, Code, Review]
connections:
  - target: llm-service
    type: runs_on
  - target: review-standards
    type: references
metadata:
  complexity: low
  avg_tokens: 800
---

## Capability

Checks code against a defined set of style guidelines to enforce consistency across a codebase. Goes beyond what automated formatters catch by evaluating naming conventions, comment quality, file organisation, and idiomatic usage patterns.

## Checks Performed

### Naming Conventions

- Variables and functions follow the project's casing convention (camelCase, snake_case, etc.)
- Boolean variables use interrogative prefixes (is, has, should, can)
- Constants use UPPER_SNAKE_CASE where the language convention expects it
- Type and class names use PascalCase
- File names match the project's convention (kebab-case, camelCase, or matching the default export)

### Code Organisation

- Imports are grouped logically (standard library, third-party, local) with consistent ordering
- Related functions are co-located rather than scattered across the file
- Module exports are clearly defined and not buried in the middle of the file
- Files do not exceed a reasonable length (guideline: 300 lines for logic files)

### Documentation and Comments

- Public functions and classes have doc comments explaining purpose, parameters, and return values
- Comments explain *why*, not *what* — obvious code is not annotated
- TODO and FIXME comments include an owner or ticket reference
- No commented-out code left in the codebase

### Idiomatic Usage

- Language-specific idioms are preferred over generic patterns (e.g., list comprehensions in Python, destructuring in JavaScript)
- Modern language features are used where available (optional chaining, pattern matching, async/await)
- Deprecated APIs or patterns are flagged with migration guidance
- Error handling follows the language's established conventions

### Formatting

- Consistent indentation (tabs or spaces, not mixed)
- Line length within project limits
- Consistent brace style and semicolon usage
- Trailing whitespace and missing newlines at end of file

## Output Format

Returns a list of style violations, each with:

1. **Rule** — which guideline was violated
2. **Location** — file path and line number
3. **Actual** — what the code currently does
4. **Expected** — what it should look like
5. **Severity** — error (must fix) or warning (should fix)
6. **Auto-fixable** — whether a formatter or codemod could resolve this automatically

## Configuration

Adapts to the project's existing style by reading configuration files (.eslintrc, .prettierrc, pyproject.toml, .editorconfig) when available. Falls back to widely-accepted community defaults when no configuration is present.
