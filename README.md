## Software Engineer and Programming

Welcome to my [Obsidian](https://obsidian.md/) vault for Software Engineering and Programming. This vault contains notes, references, and resources for various programming languages, tools, and best practices that I use in my work as a software engineer.

## How to use

1. Install [Obsidian](https://obsidian.md/) and create a new vault.
2. Put this vault content inside it.
3. Install the **Dataview** community plugin to enable dynamic lists:
    - Open **Settings** (`Cmd/Ctrl + ,`).
    - Go to **Community plugins** and click **Turn on community plugins**.
    - Click **Browse** and search for "Dataview".
    - Click **Install** and then **Enable**.
    - (Optional) Enable "Enable JavaScript Queries" in Dataview settings if you plan to use advanced queries.

## Recent Notes

```dataview
LIST FROM "" WHERE file.name != this.file.name
SORT file.mday DESC
LIMIT 5
```

## 💻 Core Development

### Programming Languages
```dataview
LIST description
FROM #programming-language
SORT file.name ASC
```

### Programming Concepts
- [[SOLID Principle]] - Five Object-Oriented design guidelines that help develop software that's easier to maintain, understand and extend. 
- [[Outbox Pattern]] 
- [[Monad in CSharp]] - Functional design pattern for safe composition, chaining, and railway-oriented error handling. 

### Algorithms & Data Structures
- [[Algorithms]] Review of fundamental algorithms and data‑structure patterns.

### Testing
- [[xUnit]] Unit testing framework for .NET C#.
- JUnit Unit testing framework for Java.
- pytest Testing framework for Python.
- Selenium For browser automation testing.
- Jest Testing framework for JavaScript.

## 🏗 Architecture & Patterns

### Web Architectures
```dataview
LIST description
FROM #architecture
SORT file.name ASC
```
- [[Micro frontend]]

### Security
- OWASP Open Web Application Security Project guidelines.
- SSL/TLS Protocols for secure communications over a computer network.
- OAuth Open standard for access delegation.

## ⚙️ Infrastructure & DevOps

### Databases
```dataview
LIST description
FROM #database
SORT file.name ASC
```
- MongoDB NoSQL database for modern applications.
- Redis In-memory data structure store used as a database, cache, and message broker.
- MySQL Popular relational database management system.
- [[Vector Database]] High-dimensional vector storage and similarity search for AI context and RAG.

### DevOps and CI/CD
- Kubernetes For container orchestration.
- [[Docker]] A platform for developing, shipping, and running applications inside lightweight, portable containers.
- Jenkins Continuous Integration and Continuous Deployment tool.
- Ansible For automation and configuration management.
- Terraform Infrastructure as Code tool for provisioning cloud resources.
- GitHub Action

### Cloud Services
- Digital Ocean droplets
- AWS Amazon Web Services for cloud computing.
- Azure Microsoft's cloud computing service.
- Google Cloud Platform (GCP) Another major cloud computing service.

### Web Servers
- [[Nginx]] A high-performance web server and reverse proxy server.

## 🌐 Frameworks & Ecosystems

- **Web Frameworks**: [[React]], [[Vue]], [[Angular]], Express.js, GraphQL.
- **.NET Ecosystem**: [[DotNET]], [[EF Core]].

## 🛠 Workspace & Environment

- **Operating Systems**: [[Windows]], [[Linux]] ([[Pop!_os]], [[CachyOS]]), [[Ubuntu Server]], [[WSL]].
- **Programming Tools**: [[Git]], [[GNU Stow]], [[CMake]], [[Neovim]], [[Visual Studio Code]], [[PowerShell]], [[Tmux]], [[Configure Hyprland]], [[Pacman Basic Usage]].
- **AI Tools**: [[AI Tools]] ([[Antigravity CLI]], [[Claude Code]], [[GitHub Copilot CLI]], [[Codex]], [[Gemini CLI]]).
- **Documentation**: Markdown, JIRA, Confluence, [[Changelog Template]], [[README Template]].

## Notes 

- Keep this vault organized by regularly updating and reviewing the content. 
- Use tags and links to connect related notes and topics. 
- Take advantage of Obsidian’s features such as backlinks, graph view, and plugins.

## License 

This vault is shared under the MIT License. Feel free to use, modify, and distribute the content. 

--- 
Happy coding!
