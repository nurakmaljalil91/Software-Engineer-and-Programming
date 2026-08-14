---
title: Antigravity CLI
category: ai-tools
tags:
  - ai-tools
  - google
  - cli
  - dev-tools
created: 2026-08-14
updated: 2026-08-14
status: active
---

## Overview

**Antigravity CLI** (invoked via the command `agy`) is Google's AI-first terminal development agent. It serves as the successor to Gemini CLI, providing advanced multi-step reasoning, multi-file code generation, subagent delegation, tool calling, and full terminal integration.

---

## Installation & Setup

### Installation Commands

- **macOS / Linux**:
  ```bash
  curl -fsSL https://antigravity.google/cli/install.sh | bash
  ```
- **Windows (PowerShell)**:
  ```powershell
  irm https://antigravity.google/cli/install.ps1 | iex
  ```

### Authentication
On first run, launch `agy` in your terminal and complete the interactive authentication prompt in your browser.

---

## Basic Usage

### Launching the CLI
Navigate to your project directory and start the interactive Terminal User Interface (TUI):

```bash
agy
```

You can also pass a prompt directly:
```bash
agy "Refactor the authentication module to use async/await"
```

### Useful CLI Flags

| Flag | Description |
| --- | --- |
| `--help` | Show all available CLI subcommands and options |
| `--dangerously-skip-permissions` | Run in unattended mode (auto-approves tool execution) |
| `--agent <agent-name>` | Launch using a specific custom agent configuration |

### Exiting
Press `Ctrl+D` twice or type `/exit` or `/quit` inside the TUI.

---

## Interactive TUI Features

When running inside `agy`, you can use special syntax to interact with your environment:

- **Execute Shell Commands**: Prefix with `!` to run bash commands directly within the prompt:
  ```text
  !git status
  ```
- **File Autocompletion**: Type `@` to invoke file autocompletion and attach workspace file paths:
  ```text
  Check @src/index.ts for performance bottlenecks
  ```

---

## Essential Slash Commands

Inside the TUI, the following slash commands control session flow and agent behavior:

| Command | Description |
| --- | --- |
| `/help` or `?` | Displays all available slash commands |
| `/config` or `/settings` | Opens interactive settings and permissions menu |
| `/clear` | Clears conversation history |
| `/rewind` or `/undo` | Reverts the last conversation step or action |
| `/plan` | Asks the agent to construct a step-by-step implementation plan before coding |
| `/goal` | Runs long-running background tasks with high thoroughness |
| `/grill-me` | Triggers an interactive interview to align on complex design decisions |
| `/schedule` | Sets one-shot timers or recurring cron tasks |
| `/learn` | Teaches and persists custom agent workflows for future sessions |
| `/teamwork-preview` | Launches parallel subagents working collaboratively |

---

## Configuration & Customizations

### Settings File
Global CLI settings are stored in JSON format at:
```text
~/.gemini/antigravity-cli/settings.json
```

### Customizations Engine (Skills & Rules)
- **Global Customizations**: `~/.gemini/config/`
- **Workspace Customizations**: `.agents/` directory within project root
- **Workspace Rules**: `GEMINI.md` or `AGENTS.md` in the project root directory

---

## Skills in Antigravity CLI

**Skills** are modular packages of knowledge and runbooks that teach `agy` specialized multi-step procedures (e.g., deployment workflows, database migrations, code refactoring recipes).

### How Skills Work (Progressive Disclosure)
To conserve token context, Antigravity CLI loads only the `name` and `description` of available skills at start. When a task matches a skill's description, `agy` dynamically reads the full `SKILL.md` instructions.

### Skill Directory Structure
Skills reside in a `skills/` folder inside any customization root:

```text
.agents/skills/<skill-name>/      # Project-level skill
└── SKILL.md                      # Required instruction file
├── scripts/                      # Optional helper scripts
├── references/                   # Optional detailed reference docs
└── templates/                    # Optional templates or assets
```

### Creating a Skill

1. **Create Skill Directory**:
   ```bash
   mkdir -p .agents/skills/docker-deploy
   ```

2. **Create `SKILL.md` with Frontmatter**:
   ```markdown
   ---
   name: docker-deploy
   description: Use this skill whenever the user asks to build, package, or deploy Docker containers for this project.
   ---

   # Docker Deployment Guide

   Follow these steps to deploy the service:

   1. Check Docker daemon status: `docker info`
   2. Build image: `docker build -t app:latest .`
   3. Run health check: `./scripts/health-check.sh`
   ```

### Migration from Gemini CLI
If upgrading from the legacy Gemini CLI, run:
```bash
agy plugin import gemini
```
This imports existing configurations, rules, and plugins into `agy`.

---

## Related Notes

- [[Skills|Antigravity Skills]]
- [[AI Tools]]
- [[Gemini CLI]]
- [[Claude Code]]
- [[GitHub Copilot CLI]]
- [[Git]]
- [[Linux]]
