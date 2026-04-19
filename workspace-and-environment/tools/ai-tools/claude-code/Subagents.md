---
title: Subagents
category: ai-tools
tags:
  - ai-tools
  - claude-code
created: 2026-04-19
updated: 2026-04-19
status: active
---
## Overview

A subagent is a specialized worker spawned by the main Claude Code session. It runs in its own isolated context window, completes a focused task, and returns only a summary to the parent — keeping verbose tool output out of the main conversation.

## Subagent vs Main Agent

| Aspect | Subagent | Main Agent |
|--------|----------|------------|
| Context | Fresh — no conversation history | Full conversation history |
| System prompt | Custom (from frontmatter body) | Default Claude Code prompt |
| Output | Returns summary to parent only | Visible to user |
| Can spawn subagents | No | Yes |

## How to Invoke a Subagent

**Automatic** — Claude matches the task to a subagent's `description` and delegates automatically.

**Explicit mention** in your prompt:
```text
Use the code-reviewer agent to check auth.js
```

**@-mention** (CLI only):
```text
@"code-reviewer" look at the auth changes
```

**Run entire session as a subagent:**
```bash
claude --agent code-reviewer
```

## What Subagents Inherit

**Receives:**
- Custom system prompt from frontmatter body
- Task prompt passed by the parent
- Project `CLAUDE.md`
- Allowed tools (subset or all)

**Does NOT receive:**
- Parent's conversation history
- Parent's system prompt
- MCP servers (unless listed in `mcpServers` frontmatter)
- Skills (unless listed in `skills` frontmatter)

## Worktree Isolation

Set `isolation: worktree` in frontmatter to give the subagent its own isolated copy of the repo. This is useful when running subagents in parallel to avoid file conflicts. The worktree is cleaned up automatically if no changes are made.

```yaml
---
name: parallel-worker
isolation: worktree
---
```

## Restricting Which Subagents Can Be Spawned

Use the `Agent(name1, name2)` syntax in the `tools` field to allowlist which subagents this agent can spawn:

```yaml
tools: [Agent(code-reviewer, debugger), Read, Bash]
```

Omit `Agent` entirely to prevent spawning any subagents.

## Learn More

- [[Agents]] - Agent system overview and creating custom agents.
- [[Multi-Agents]] - Running multiple independent sessions in parallel.
