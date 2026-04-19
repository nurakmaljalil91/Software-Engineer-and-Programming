---
title: MCP Postman
category: ai-tools
tags:
  - ai-tools
  - claude-code
  - mcp
created: 2026-04-19
updated: 2026-04-19
status: active
---
## Overview

[@postman/postman-mcp-server](https://github.com/postmanlabs/postman-mcp-server) lets Claude Code run Postman collections, execute API requests, manage environments, and generate client code from API definitions. Requires a Postman API key.

## Prerequisites

- Node.js installed
- A Postman account and API key — get it from [Postman API Keys](https://go.postman.co/settings/me/api-keys)

## Install

```bash
claude mcp add postman --env POSTMAN_API_KEY=YOUR_API_KEY -- npx @postman/postman-mcp-server@latest
```

Or manually in `.claude/settings.json`:

```json
{
  "mcpServers": {
    "postman": {
      "command": "npx",
      "args": ["@postman/postman-mcp-server@latest", "--minimal"],
      "env": {
        "POSTMAN_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

## Modes

| Flag | Tools Available | Use When |
|------|----------------|----------|
| `--minimal` (default) | 37 essential tools | Day-to-day API testing |
| `--full` | 100+ tools | Full Postman API access |
| `--code` | Includes code generation | Generating client SDKs |

## Learn More

- [[MCP]]
