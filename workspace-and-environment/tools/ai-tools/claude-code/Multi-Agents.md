---
title: Multi-Agents (Agent Teams)
category: ai-tools
tags:
  - ai-tools
  - claude-code
created: 2026-04-19
updated: 2026-04-19
status: active
---
## Overview

Agent Teams are multiple independent Claude Code sessions working in parallel. One session acts as the **lead** (orchestrator); the others are **teammates**. Unlike [[Subagents]], teammates have their own full context window and can message each other directly.

> **Status:** Experimental — disabled by default.

## Enable Agent Teams

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

Or in `.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

Requires Claude Code v2.1.32 or later.

## Architecture

```
Lead Session (orchestrator)
├── Teammate 1  ──┐
├── Teammate 2    ├── Shared task list + mailbox
└── Teammate 3  ──┘
```

Each teammate runs in its own session, claims tasks from a shared list, and can send messages to other teammates via `SendMessage`.

## How to Use Multi-Agents

Just describe what you want in natural language — the lead session spawns and coordinates teammates automatically:

```text
Create an agent team to investigate why login is slow.
Spawn 3 teammates:
- One investigates database query performance
- One checks network/authentication service latency
- One reviews recent auth code changes
Have them share findings and summarize the most likely cause.
```

The lead will:
1. Spawn teammates in parallel.
2. Assign tasks from a shared task list.
3. Collect findings as teammates finish.
4. Synthesize a final summary.

## Subagents vs Agent Teams

| Aspect | Subagents | Agent Teams |
|--------|-----------|-------------|
| Sessions | Single session | Multiple independent sessions |
| Context per worker | Fresh, summarized | Full context window |
| Communication | Results back to parent only | Teammates message each other |
| Parallelism | Sequential chains | True parallel |
| Token cost | Lower | Much higher |
| Setup | Built-in | Experimental, requires enabling |
| Best for | Self-contained tasks | Parallel research, competing theories |

## Display Modes

```bash
claude --teammate-mode in-process   # All in one terminal (default)
claude --teammate-mode tmux         # Split panes (requires tmux)
```

Or set permanently in `~/.claude.json`:

```json
{
  "teammateMode": "tmux"
}
```

## Quality Gates via Hooks

Use hooks in `settings.json` to reject incomplete work:

| Hook | Fires When | Use `exit 2` to... |
|------|-----------|---------------------|
| `TeammateIdle` | Teammate finishes | Reject completion and keep working |
| `TaskCreated` | Task being created | Reject task and send feedback |
| `TaskCompleted` | Task marked complete | Require rework before accepting |

## When to Use Agent Teams

- Investigating a problem from multiple angles simultaneously.
- Running parallel code reviews (security, performance, test coverage).
- Competing hypotheses — teammates independently reach conclusions then compare.
- Large refactors where different teammates own different modules.

Avoid them for simple or sequential tasks — the token cost scales linearly with team size.

## Learn More

- [[Agents]] - Agent system overview and creating custom agents.
- [[Subagents]] - Single-session isolated workers.
