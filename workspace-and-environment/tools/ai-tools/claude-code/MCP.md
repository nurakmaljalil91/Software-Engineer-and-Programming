---
title: MCP (Model Context Protocol)
category: ai-tools
tags:
  - ai-tools
  - claude-code
created: 2026-04-19
updated: 2026-04-19
status: active
---
## Overview

MCP (Model Context Protocol) is an open protocol that lets Claude Code connect to external tools and services. Each MCP server exposes a set of tools Claude can call during a session.

## Adding an MCP Server

**Via CLI (recommended):**
```bash
claude mcp add <name> <command>
```

**Via project config** (`.claude/settings.json`):
```json
{
  "mcpServers": {
    "<name>": {
      "command": "npx",
      "args": ["<package>@latest"]
    }
  }
}
```

**Scopes:**
- `--scope project` (default) — stored in `.claude/settings.json`, shared with the repo.
- `--scope user` — stored in `~/.claude/settings.json`, available across all projects.
- `--scope local` — stored in `.claude/settings.local.json`, not committed.

## Listing & Removing Servers

```bash
claude mcp list
claude mcp remove <name>
```

## MCP Servers

- [[MCP Chrome DevTools]] - Control live Chrome instances for debugging and automation.
- [[MCP Docker]] - Manage containers, images, and networks.
- [[MCP Playwright]] - Cross-browser automation and E2E testing.
- [[MCP Postman]] - Run collections, manage environments, and test APIs.
