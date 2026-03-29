---
type: workflow
id: pr-review-pipeline
title: PR Review Pipeline
description: "Orchestrates the full pull request review: fetch, analyse, check style, scan security, summarise, decide"
tags: [Production, Tested, quality:review, technical:code]
connections:
  - target: code-analysis
    type: uses
  - target: security-scanning
    type: uses
  - target: style-checking
    type: uses
  - target: review-summary
    type: uses
  - target: inline-comments
    type: uses
  - target: security-report
    type: uses
  - target: style-violations
    type: uses
  - target: approval-decision
    type: uses
  - target: structured-data-extraction
    type: uses
  - target: github-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
metadata:
  estimated_duration: "30-90 seconds"
  avg_tokens: 5000
  trigger: manual
---

## Overview

This workflow runs a complete automated code review on a pull request. It fetches the PR diff from GitHub, runs three parallel analysis passes, then generates structured output and a final decision.

## Pipeline Stages

### Stage 1: Fetch PR Data

**Input:** {{input.pr_url}}

Using the GitHub MCP service, fetch the pull request at {{input.pr_url}} and retrieve:

- PR metadata (title, description, author, base branch, head branch)
- The full diff (changed files with context)
- List of modified files with change statistics (additions, deletions)
- Any existing review comments (to avoid duplicating feedback)

**Output:** Structured PR data object passed to all subsequent stages.

### Stage 2: Parallel Analysis

Three analysis passes run concurrently against the changed files:

#### 2a. Code Analysis

Invoke the **code-analysis** skill against each modified file. Focus analysis on the changed lines and their immediate context (surrounding functions and classes). Do not analyse unchanged files unless a change affects their public interface.

#### 2b. Style Checking

Skip this pass if {{input.skip_style}} is set to `true`. Otherwise, invoke the **style-checking** skill against each modified file. Load the repository's style configuration (.eslintrc, .prettierrc, etc.) if available. Only flag style issues in changed lines — existing violations in untouched code are out of scope for this review.

#### 2c. Security Scanning

Skip this pass if {{input.skip_security}} is set to `true`. Otherwise, invoke the **security-scanning** skill against each modified file. Additionally scan any new or modified configuration files, dependency manifests, and environment variable references. Cross-reference findings with the review standards source document.

### Stage 3: Generate Outputs

Using the combined findings from Stage 2, generate four outputs in parallel:

#### 3a. Review Summary

Invoke the **review-summary** prompt to produce the top-level PR comment. This is the first thing the author sees.

#### 3b. Inline Comments

Invoke the **inline-comments** prompt to produce per-line comments. These are posted as review comments on specific lines of the diff.

#### 3c. Security Report

Invoke the **security-report** prompt only if security findings exist. If no security issues were found, skip this step and note it in the review summary.

#### 3d. Style Violations

Invoke the **style-violations** prompt to produce the categorised list of style issues. If all violations are auto-fixable, note that the author can run their formatter to resolve them.

### Stage 4: Decision

Invoke the **approval-decision** prompt with the full set of findings. This produces the final approve/request-changes/comment decision, which is attached to the review summary.

### Stage 5: Post Review

Using the GitHub MCP service:

1. Post the review summary as a PR comment
2. Post inline comments on the relevant lines
3. Submit the review with the appropriate status (APPROVE, REQUEST_CHANGES, or COMMENT)
4. If a security report was generated, post it as a separate comment (collapsed by default for readability)

## Error Handling

- If the GitHub MCP service is unreachable, abort and report the connection error
- If any analysis skill fails, continue with the remaining skills and note the gap in the review summary
- If the PR has no code changes (only documentation or assets), skip code analysis and security scanning; run style checking only against markdown files
- If the diff is too large (more than 5,000 changed lines), split into batches and process sequentially to stay within token limits

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `max_findings` | 50 | Maximum number of findings to include in the review |
| `severity_threshold` | low | Minimum severity to report (low, medium, high, critical). Set via {{input.severity_threshold}} |
| `skip_style` | false | Skip style checking pass |
| `skip_security` | false | Skip security scanning pass |
| `auto_approve` | false | If true, automatically approve PRs with no high/critical findings |
| `batch_size` | 2000 | Maximum changed lines per analysis batch |

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.pr_url}}` | Yes | GitHub pull request URL | `https://github.com/acme/api/pull/42` |
| `{{input.severity_threshold}}` | No | Minimum severity to report. Default: `low` | `medium` |
| `{{input.skip_style}}` | No | Skip style checking pass. Default: `false` | `true` |
| `{{input.skip_security}}` | No | Skip security scanning pass. Default: `false` | `true` |

## Outputs

| Name | Description |
|------|-------------|
| Review summary | Top-level PR comment summarising all findings, posted as a GitHub PR comment |
| Inline comments | Per-line review comments posted directly on the diff |
| Security report | Detailed security findings (only if issues found), posted as a collapsed comment |
| Style violations | Categorised list of style issues with auto-fix availability noted |
| Approval decision | Final verdict: APPROVE, REQUEST_CHANGES, or COMMENT |

## Setup

Before running this workflow:

1. **GitHub MCP server** — install and configure the GitHub MCP server in your skrptiq settings. The workflow uses it to fetch PR data and post review comments.
2. **GitHub access** — ensure the MCP server has read/write access to the target repository (a personal access token with `repo` scope, or a GitHub App with appropriate permissions).
3. **Network access** — the workflow needs to reach `github.com` to fetch PR data and post results.

No specific AI provider or API key is required beyond your configured skrptiq LLM provider.

## Provider Notes

- Code analysis and security scanning steps benefit from a model with strong reasoning capabilities.
- Style checking is lighter — a faster, smaller model works well here.
- The final approval decision step benefits from a model that handles nuanced judgements well.
- Large PRs (1000+ changed lines) will use more tokens — consider a provider with generous context limits.

## Example Input

To test this workflow immediately after import:

```
PR URL: https://github.com/octocat/Hello-World/pull/1
```

For a more realistic test, point it at any open PR in a repository you have access to. The workflow adapts to any language and framework — it reads the project's own style configuration and adjusts its analysis accordingly.
