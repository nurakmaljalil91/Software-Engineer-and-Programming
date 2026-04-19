# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is a personal **Obsidian vault** — a Markdown-based knowledge base for software engineering and programming. There is no build system, test suite, or application to run. The "codebase" is a collection of `.md` notes organized into topical directories.

## Structure

```
architecture-and-patterns/   # Clean Architecture, DDD, SOLID, microfrontend
core-development/            # Programming languages (C++, C#, Python, JS/TS, Go, Rust, Java), algorithms, testing concepts
frameworks-and-ecosystems/   # .NET/EF Core, React, Vue, Angular
infrastructure-and-devops/   # PostgreSQL, MSSQL, Docker, Nginx
workspace-and-environment/   # OS (Linux, Windows, WSL, Ubuntu), tools (Git, Neovim, CMake, WezTerm), templates
```

Each subdirectory typically contains a hub file (e.g., `Git.md`, `PostgreSQL.md`) that links to more specific notes on sub-topics.

## Obsidian Conventions

- **Internal links** use `[[Note Name]]` syntax — these are Obsidian wiki-links, not standard Markdown.
- **Tags** (e.g., `#programming-language`, `#database`, `#architecture`) are used to drive dynamic `dataview` queries in `README.md`.
- The **Dataview** community plugin must be installed and enabled for the README's dynamic lists to render correctly.
- New notes should include an "Overview" section and link to related notes.
- Use the templates in `workspace-and-environment/templates/` (`README Template.md`, `Changelog Template.md`) when creating new notes.

## Adding or Editing Notes

- Place new notes in the most relevant existing subdirectory.
- Use Obsidian-style internal links (`[[Note Name]]`) to cross-reference related notes.
- Add the appropriate tag in the frontmatter if the note should appear in a `dataview` query (e.g., `tags: [programming-language]`).
- Hub/index notes for a topic live at the top level of their directory (e.g., `Git.md` inside `tools/git/`).
