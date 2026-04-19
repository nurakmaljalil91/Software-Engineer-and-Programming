---
title: MCP Chrome DevTools
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

[chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) lets Claude Code control a live Chrome instance — inspect the DOM, analyze network requests, capture performance traces, take screenshots, and run Puppeteer automation.

## Prerequisites

- Node.js v20.19+
- Chrome (stable or newer) — must be running before starting the server

## Install

```bash
claude mcp add chrome-devtools npx -y chrome-devtools-mcp@latest
```

Or manually in `.claude/settings.json`:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

## Learn More

- [[MCP]]
