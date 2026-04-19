---
title: MCP Docker
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

[docker-mcp](https://github.com/QuantGeekDev/docker-mcp) lets Claude Code manage Docker containers, images, volumes, and networks — build, run, stop, and remove containers, execute commands inside them, all via natural language.

## Prerequisites

- Docker Desktop installed and running
- Node.js 18+

## Install

```bash
npx @smithery/cli install docker-mcp --client claude
```

Or manually in `.claude/settings.json`:

```json
{
  "mcpServers": {
    "docker-mcp": {
      "command": "uvx",
      "args": ["docker-mcp"]
    }
  }
}
```

## Learn More

- [[MCP]]
