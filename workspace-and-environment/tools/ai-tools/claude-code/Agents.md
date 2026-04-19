---
title: Claude Code Agents
category: ai-tools
tags:
  - ai-tools
  - claude-code
created: 2026-04-19
updated: 2026-04-19
status: active
---
## Overview

Claude Code has two agent systems for delegating work:

| System | How it works | Best for |
|--------|-------------|----------|
| **Subagents** | Spawned within the same session; return a summary to the parent | Isolated, focused tasks |
| **Multi-Agents (Agent Teams)** | Multiple independent Claude Code sessions working in parallel | Complex parallel investigations |

## How the Agentic Loop Works

1. Claude receives your prompt.
2. If the task matches a subagent's `description`, Claude invokes it via the **Agent tool**.
3. The subagent runs in a fresh context, completes its task, and returns a summary.
4. The parent receives the summary as a tool result and continues.

## Creating Custom Subagents

Place a Markdown file with YAML frontmatter in one of these locations:

| Scope | Path |
|-------|------|
| Project | `.claude/agents/agent-name.md` |
| User (all projects) | `~/.claude/agents/agent-name.md` |

Minimal frontmatter:

```yaml
---
name: code-reviewer
description: Reviews code changes for correctness and style. Use when the user asks for a code review.
model: sonnet
tools: [Read, Grep]
---
You are a careful code reviewer. Focus on logic errors and naming clarity.
```

Key frontmatter fields:

| Field | Purpose |
|-------|---------|
| `name` | Unique ID (lowercase, hyphens) |
| `description` | When Claude should invoke this agent |
| `model` | `sonnet`, `opus`, `haiku`, or `inherit` |
| `tools` | Allowlist of tools the agent can use |
| `permissionMode` | `default`, `acceptEdits`, `auto`, `plan`, `bypassPermissions` |
| `isolation` | `worktree` — gives agent an isolated git copy |
| `maxTurns` | Max agentic turns before stopping |

## Built-in Agents

| Name | Tools | Purpose |
|------|-------|---------|
| `Explore` | Read-only | Fast codebase search; returns summaries |
| `Plan` | Read-only | Research during plan mode |
| `general-purpose` | All | Complex multi-step tasks |

## Learn More

- [[Subagents]] - Isolated single-session workers.
- [[Multi-Agents]] - Parallel independent sessions (Agent Teams).
- [[Claude Code]]
