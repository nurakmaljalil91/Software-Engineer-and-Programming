---
title: MCP Playwright
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

[@playwright/mcp](https://github.com/microsoft/playwright-mcp) by Microsoft lets Claude Code automate browsers (Chrome, Firefox, WebKit, Edge) for E2E testing and web interaction. Uses structured accessibility trees rather than screenshots, so it works without vision models.

## Prerequisites

- Node.js installed
- Playwright browsers (auto-installed on first run)

## Install

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Or manually in `.claude/settings.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

## Learn More

- [[MCP]]
