---
type: service
id: github-mcp
title: GitHub MCP
description: "GitHub Model Context Protocol service for pull request access and review posting"
tags: [Production, Code, Review]
connections: []
metadata:
  provider: github
  protocol: mcp
  auth_type: personal_access_token
  env_var: GITHUB_TOKEN
  required_scopes: [repo, pull_request]
---

## Service Description

Provides access to GitHub repositories and pull requests via the Model Context Protocol (MCP). This service is used at both ends of the review pipeline: fetching PR data at the start and posting review results at the end.

## Configuration

### Authentication

Requires a GitHub personal access token (classic or fine-grained) set as the `GITHUB_TOKEN` environment variable. The token must have the following scopes:

- **repo** — read access to repository contents and diffs
- **pull_request** — read and write access to pull request comments and reviews

For fine-grained tokens, the equivalent permissions are:
- Repository: Contents (read)
- Pull requests: Read and write

### MCP Server Setup

The GitHub MCP server must be configured in the user's MCP settings. Typical configuration:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "{GITHUB_TOKEN}"
      }
    }
  }
}
```

## Capabilities Used

### Reading

- `get_pull_request` — retrieve PR metadata (title, description, state, author)
- `get_pull_request_diff` — retrieve the full diff for changed files
- `get_pull_request_files` — list modified files with change statistics
- `get_pull_request_reviews` — fetch existing reviews to avoid duplicate feedback
- `get_file_contents` — read full file contents for context beyond the diff

### Writing

- `create_pull_request_review` — submit the review with status and body
- `create_review_comment` — post inline comments on specific diff lines
- `create_issue_comment` — post general comments (used for the security report)

## Rate Limiting

GitHub's API rate limit is 5,000 requests per hour for authenticated users. The pipeline typically consumes 10-30 requests per review depending on the number of changed files. The workflow tracks remaining rate limit via response headers and warns if approaching the limit.

## Privacy Considerations

All repository data accessed through this service is sent to your configured LLM provider for analysis. Users should ensure their organisation's policies permit sending source code to third-party AI services. The `data_handling: source-code` declaration in the manifest makes this explicit during import.
