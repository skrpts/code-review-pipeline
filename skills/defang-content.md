---
type: skill
id: defang-content
title: Defang Content
description: "Neutralises potentially dangerous content in code snippets, URLs, and commands before display or processing"
tags: [Production, Security]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Identifies and neutralises potentially dangerous content in text before it is displayed, stored, or processed further. Prevents accidental execution of malicious code, clicking of harmful URLs, or running of destructive commands.

## What It Defangs

1. **URLs** — replaces `http://` with `hxxp://` and `.` in domains with `[.]` (e.g. `hxxps://malicious[.]example[.]com`)
2. **IP addresses** — replaces dots with `[.]` (e.g. `192[.]168[.]1[.]1`)
3. **Email addresses** — replaces `@` with `[@]` and `.` with `[.]`
4. **Code execution patterns** — wraps shell commands, SQL statements, and script blocks in warning markers
5. **File paths** — flags paths that reference sensitive system locations

## When to Use

- Before displaying user-submitted bug reports that may contain malicious URLs or code
- In code review pipelines where the reviewed code may contain security vulnerabilities
- When documenting security incidents or vulnerabilities
- Before storing content that will be rendered in a web interface

## What It Does NOT Do

- Execute or test the dangerous content
- Remove the content entirely (it preserves it in a safe form)
- Make judgements about intent (it defangs all matching patterns regardless)

## Inputs

Text content that may contain dangerous URLs, code, or commands.

## Outputs

Defanged text with dangerous patterns neutralised. Each defanged item is marked so it can be identified and, if needed, restored.
