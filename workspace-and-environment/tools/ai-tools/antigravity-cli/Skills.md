---
title: Antigravity Skills
category: ai-tools
tags:
  - ai-tools
  - google
  - antigravity
  - skills
created: 2026-08-14
updated: 2026-08-14
status: active
---

## Overview

In **Antigravity CLI (`agy`)**, **Skills** are modular packages of procedures, knowledge, and runbooks that extend the agent's capabilities. They act as automated "cheatsheets" for specific development workflows, enabling the agent to execute complex multi-step tasks reliably.

---

## How Skills Work: Progressive Disclosure

To prevent polluting the model's context window with unnecessary instructions, Antigravity uses **progressive disclosure**:

1. **Startup**: At session initialization, `agy` loads *only* the `name` and `description` of available skills into memory.
2. **Triggering**: When a user's prompt matches a skill's `description`, `agy` dynamically reads the full `SKILL.md` file (and any linked resources) into the active conversation context.
3. **Execution**: The agent follows the step-by-step instructions and can execute helper scripts within the skill folder.

---

## Skill Locations & Priority

Antigravity CLI automatically discovers skills from two main customization roots:

| Scope | Location | Version Controlled? | Description |
| --- | --- | --- | --- |
| **Workspace** | `.agents/skills/<skill-name>/` | Yes (Check into Git) | Specific to the project; shared across your team |
| **Global** | `~/.gemini/config/skills/<skill-name>/` | No (Local machine) | Available across all projects on your local environment |

### Precedence Hierarchy (Highest to Lowest):
1. Workspace project skills (`.agents/skills/`)
2. Explicitly declared skills in `skills.json`
3. Global skills (`~/.gemini/config/skills/`)
4. Built-in skills packaged with `agy`

---

## Skill Directory Structure

Every skill lives inside its own folder under `skills/`:

```text
skills/<skill-name>/
├── SKILL.md          # Required: Main instructions with YAML frontmatter
├── scripts/          # Optional: Executable scripts (bash, python, node)
├── references/       # Optional: Bulky reference docs (loaded on demand)
├── examples/         # Optional: Code examples and reference implementations
└── templates/        # Optional: Code templates, boilerplate, or configs
```

---

## Anatomy of `SKILL.md`

The `SKILL.md` file must begin with a YAML frontmatter block containing two mandatory fields:

```markdown
---
name: docker-build-deploy
description: Use this skill whenever the user asks to build, test, package, or deploy Docker containers for this project.
---

# Docker Build & Deployment Guide

Follow these steps to build and deploy the containerized service:

## Step 1: Pre-flight Verification
Verify the Docker daemon is active:
`docker info`

## Step 2: Build Image
Run the build script:
[build.sh](./scripts/build.sh)

## Step 3: Run Tests Inside Container
`docker run --rm app-image:test npm test`

## Step 4: Health Check
Confirm the service returns HTTP 200:
`curl -I http://localhost:8080/health`
```

### Frontmatter Rules:
- **`name`**: Unique, lowercase, hyphen-separated string (e.g. `db-migration`, `react-component-gen`).
- **`description`**: Written in third-person. Clearly states **what** the skill does and **when** `agy` should activate it.

---

## How to Create a Custom Skill (Step-by-Step)

### Step 1: Create the Skill Directory
Navigate to your project root and create the skill folder:
```bash
mkdir -p .agents/skills/api-endpoint-gen
```

### Step 2: Create Helper Scripts (Optional)
If your skill requires complex script execution, create a `scripts/` folder:
```bash
mkdir -p .agents/skills/api-endpoint-gen/scripts
```
Create `.agents/skills/api-endpoint-gen/scripts/validate-schema.sh`:
```bash
#!/usr/bin/env bash
echo "Validating OpenAPI schema..."
# Schema validation logic here
```
Make it executable:
```bash
chmod +x .agents/skills/api-endpoint-gen/scripts/validate-schema.sh
```

### Step 3: Write `SKILL.md`
Create `.agents/skills/api-endpoint-gen/SKILL.md`:
```markdown
---
name: api-endpoint-gen
description: Use this skill when asked to create new REST API endpoints, update API routes, or validate OpenAPI schemas.
---

# API Endpoint Generation Guide

1. Check existing routes in `src/routes/`.
2. Validate schema using [validate-schema.sh](./scripts/validate-schema.sh).
3. Create controller, model, and route files following clean architecture.
4. Add unit test under `tests/routes/`.
```

### Step 4: Verify in Antigravity CLI
Start `agy`:
```bash
agy
```
Ask a prompt matching your skill description:
> *"Can you generate a new REST API endpoint for user preferences?"*

`agy` will automatically detect the skill, read `SKILL.md`, and carry out the instructions!

---

## Best Practices

1. **Keep `SKILL.md` Lean**: Use progressive disclosure by placing long documentation or API specs inside `references/` and linking to them.
2. **Use Executable Helpers**: Put multi-command bash sequences into `scripts/` instead of cluttering markdown instructions.
3. **Include Validation**: Always instruct the agent on how to verify success (e.g., checking status codes, running test suites).
4. **Third-Person Descriptions**: Ensure the `description` field clearly tells the agent *when* to trigger the skill.

---

## Related Notes

- [[Antigravity CLI]]
- [[AI Tools]]
- [[Claude Code]]
- [[Git]]
- [[Linux]]
