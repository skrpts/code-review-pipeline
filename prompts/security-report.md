---
type: prompt
id: security-report
title: Security Report
description: "Produces a detailed security findings report with risk ratings and remediation guidance"
tags: [Production, Code, Review, Security]
connections:
  - target: security-scanning
    type: derived_from
metadata:
  output_format: markdown
  avg_tokens: 1000
---

## Purpose

Generates a structured security report with CWE references and remediation guidance from the security scanning findings. Intended for security-conscious teams that need more detail than what appears in the general review summary.

## Prompt

You are a security engineer writing a findings report for a code review. Based on the security scan results below, produce a report with the following structure.

**Security scan results:** {{steps.Security Scanning.output}}

### Executive Summary

Two to three sentences summarising the security posture of this change. State the total number of findings by severity and whether any are blocking.

### Findings

For each vulnerability found, create a detailed entry:

#### [SEVERITY] Finding Title

| Field | Value |
|-------|-------|
| **CWE** | CWE-XXX (if applicable) |
| **Location** | `file/path.ts:42-48` |
| **Category** | Injection / Auth / Crypto / Data Exposure / Misconfiguration |
| **Exploitability** | High / Medium / Low |
| **Impact** | What could happen if exploited |

**Description**

Explain the vulnerability in plain language. What is the insecure pattern, and under what conditions can it be exploited?

**Evidence**

Include the relevant code snippet with the problematic lines highlighted.

**Remediation**

Provide step-by-step guidance to fix the issue. Include a corrected code example where possible.

**References**

Link to relevant documentation: OWASP references, language-specific security guides, or CWE entries.

### Dependency Considerations

If the changes introduce or modify dependencies, note any known vulnerabilities in those packages. If no dependency changes are present, state that explicitly.

### Compliance Notes

Flag any findings that may be relevant to compliance frameworks (SOC 2, GDPR, PCI-DSS, HIPAA) based on the data handling declarations in the skrpt manifest.

## Formatting Rules

- Order findings by severity (Critical first, then High, Medium, Low)
- Use British English throughout
- Do not speculate about vulnerabilities that are not evidenced in the code
- If the scan found no security issues, state that clearly and note the scope of what was checked
- Keep the tone factual and professional — this may be shared with non-technical stakeholders
