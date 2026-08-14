---
name: learn-programming
description: >-
  Use this skill whenever the user asks to learn, explain, or document a programming language feature, thread/concurrency concept, data structure, algorithm, or software engineering concept by creating an Obsidian note in this vault.
---

# Learn Programming & Note Creation Workflow

This skill guides the agent in creating structured, high-quality Obsidian markdown notes for programming concepts, language features, algorithms, and design patterns within this vault.

---

## Workflow Steps

### Step 1: Identify Topic Type & Validate Language

1. **Language-Specific Feature** (e.g., "Thread", "Pointers", "Auto-properties", "Lifetimes"):
   - Check if a target programming language is specified in the prompt (e.g., C++, C#, Rust, JavaScript, Python, Go, Java).
   - **CRITICAL**: If the feature is language-specific and **no programming language is mentioned**, ask the user:
     > *"Which programming language would you like to create this note for? (e.g., C++, C#, Rust, JavaScript, Go, Python)"*
   - Stop execution until the user specifies the target language.

2. **General Programming Concept / Algorithm** (e.g., "Outbox Pattern", "Binary Search", "Garbage Collection", "Clean Architecture"):
   - Specific language specification is **not required**.
   - Code examples should focus primarily on **C++**, **C#**, **JavaScript**, or **Rust**.

---

### Step 2: Search Existing Vault Notes for Cross-Linking

Before creating the new note, search the vault for existing related notes using file search or `grep_search`:

- Search `core-development/` and `architecture-and-patterns/` for related topics.
- Example: If creating a note for `Thread in C++`, check if `Thread in C#`, `Multithreading`, or `Async` already exist in the vault.
- Keep track of these existing note titles so you can link them using Obsidian internal link syntax (`[[Note Name]]`).

---

### Step 3: Determine File Path & Create Note

Place the note in the appropriate directory structure:

- **Language-Specific Note**: `core-development/programming-languages/<language>/<Topic> in <Language>.md`
  - Example: `core-development/programming-languages/cplusplus/Thread in C++.md`
  - Example: `core-development/programming-languages/csharp/Thread in CSharp.md`
- **Algorithms / Data Structures**: `core-development/algorithms/<Topic>.md`
- **General Concepts**: `core-development/concepts/<Topic>.md`

#### Required Note Template Structure:

```markdown
---
title: <Topic Title>
category: <category-name>
tags:
  - <tag-1>
  - <tag-2>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
status: active
---

## Overview

A clear, concise description of what the concept or feature is, its key use cases, and why it is important.

## Architecture / Visual Diagram

```mermaid
// Include a clean Mermaid diagram (sequenceDiagram, flowchart, or stateDiagram) 
// to visualize execution flow, lifecycle, thread synchronization, or memory layout.
```

## Code Example

Provide a concise, practical, and well-commented code example in the target language (or C++/C#/JavaScript/Rust for general concepts):

```cpp
// Working code snippet demonstrating the concept
```

## Key Considerations & Best Practices

- Bullet points highlighting performance, edge cases, thread safety, or common pitfalls.

## Related Notes

- [[Related Note 1]]
- [[Related Note 2]]
```

---

### Step 4: Update Vault Index Notes

After writing the note, link it in the corresponding hub note:

1. For language-specific notes, add `[[<Note Title>]]` to the relevant language index (e.g., `core-development/programming-languages/cplusplus/C++.md` or `core-development/programming-languages/csharp/CSharp.md`).
2. For algorithms or general concepts, add `[[<Note Title>]]` to `README.md` or `Algorithms.md`.

---

## Guidelines & Quality Standards

- **Mermaid Diagrams**: Ensure Mermaid syntax is valid. Wrap labels containing special characters in quotes.
- **Obsidian Internal Links**: Use exact note filenames without paths (e.g. `[[Thread in CSharp]]`).
- **No Dummy Code**: Code examples must be accurate and compilable/executable syntax.
