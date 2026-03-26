# GEMINI.md

## Directory Overview
This directory is an **Obsidian Vault** serving as a comprehensive personal knowledge base for software engineering and programming. It contains structured notes, references, and code snippets covering a wide range of topics, including:
- **Programming Languages:** C++, C#, Python, JavaScript/TypeScript, Go, Rust, Java.
- **Architectures & Patterns:** Clean Architecture, Domain-Driven Design (DDD), SOLID principles, Outbox pattern.
- **Databases:** PostgreSQL, MS SQL Server, EF Core.
- **Tools & DevOps:** Git, CMake, Neovim, Docker, Nginx, Linux/WSL, Ubuntu Server.
- **Web Frameworks:** React, Vue, Angular, .NET.

The repository is organized into topical subdirectories, each containing Markdown files (`.md`) that use Obsidian-style internal linking (`[[Note Name]]`).

## Key Files
- **`README.md`**: The primary entry point and high-level index for the entire vault. It categorizes the notes and provides a roadmap of the contents.
- **`architectures/Clean Architecture.md`**: A detailed guide on layered architecture, specifically tailored for .NET implementations.
- **`concepts/SOLID Principle.md`**: Explains fundamental object-oriented design principles with C# examples.
- **`tools/git/Git.md`**: A hub for version control notes, linking to cheat sheets, strategies (GitFlow vs. Trunk-Based), and troubleshooting guides.
- **`postgresql/PostgreSQL.md`**: Contains practical SQL snippets for backups, migrations, and performance optimization.
- **`algorithms/Algorithms.md`**: An index for fundamental algorithms and data structure patterns.

## Usage
- **Reference & Learning:** Use this vault to look up implementation details, architectural patterns, or command-line references for various technologies.
- **Cross-Linking:** Most notes are interconnected via internal links (`[[...]]`). When researching a topic, follow these links to find related concepts or specific setup guides.
- **Code Snippets:** Many notes contain ready-to-use code examples in C#, SQL, Bash, and PowerShell. These are intended to be used as templates for real-world projects.
- **Adding Content:** When adding new notes, follow the existing pattern of including an "Overview" section and linking to related notes within the vault. Use the templates in the `templates/` directory for consistency.

## Tech Stack (Represented in Notes)
- **Primary Languages:** C#, C++, SQL, TypeScript.
- **Frameworks:** .NET (EF Core, MediatR), React, Angular.
- **Infrastructure:** Docker, PostgreSQL, Nginx, Linux.
