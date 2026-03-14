---
type: skill
id: security-scanning
title: Security Scanning
description: "Identifies security vulnerabilities, insecure patterns, and dependency risks in code"
tags: [Production, Tested]
connections:
  - target: llm-service
    type: runs_on
  - target: review-standards
    type: references
metadata:
  complexity: high
  avg_tokens: 1500
  vulnerability_categories: [injection, auth, crypto, data-exposure, misconfiguration]
---

## Capability

Scans source code for security vulnerabilities by matching against known vulnerability patterns, checking for insecure API usage, and identifying data exposure risks. Operates as a complement to dedicated SAST tools — not a replacement — by catching issues that require contextual understanding.

## Vulnerability Categories

### Injection Flaws

- SQL injection via string concatenation or template literals in queries
- Command injection through unsanitised input passed to shell execution
- Cross-site scripting (XSS) from unescaped user input rendered in HTML
- Path traversal allowing access to files outside intended directories
- LDAP and XML injection in enterprise codebases

### Authentication and Authorisation

- Hardcoded credentials, API keys, or tokens in source files
- Missing authentication checks on protected endpoints
- Broken access control where user roles are not verified before privileged operations
- Session management issues (predictable tokens, missing expiry, no invalidation on logout)

### Cryptographic Weaknesses

- Use of deprecated algorithms (MD5, SHA-1 for security purposes, DES)
- Weak random number generation for security-sensitive values
- Missing or improper TLS certificate validation
- Encryption keys stored alongside the data they protect

### Data Exposure

- Sensitive data logged to console, files, or monitoring systems
- API responses leaking internal implementation details or stack traces
- PII stored without encryption or transmitted over unencrypted channels
- Overly permissive CORS configurations

### Misconfiguration

- Debug mode enabled in production configuration files
- Default credentials left in configuration
- Overly permissive file or directory permissions
- Missing security headers (CSP, HSTS, X-Frame-Options)

## Output Format

Returns findings grouped by severity:

1. **Critical** — exploitable vulnerabilities requiring immediate attention
2. **High** — significant risks that should be addressed before merging
3. **Medium** — issues that increase attack surface but are not directly exploitable
4. **Low** — best-practice deviations and hardening recommendations

Each finding includes the file path, line range, vulnerability type (CWE reference where applicable), explanation, and a suggested remediation.

## Limitations

Cannot detect vulnerabilities that depend on runtime state, deployment configuration, or network topology. Does not replace penetration testing or runtime application security testing (RAST). Dependency vulnerability scanning requires access to package manifests and a vulnerability database — this skill focuses on first-party code.
