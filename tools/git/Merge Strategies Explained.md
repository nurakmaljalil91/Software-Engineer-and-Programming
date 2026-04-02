---
title: Merge Strategies Explained
category: git
tags:
  - git
created: 2026-03-28
updated: 2026-03-28
status: active
---
## 1. Create a Merge Commit

- **What it does**: Takes the head of the feature branch and merges it into the target branch by creating a new “merge commit” (`M`).
- **History shape**: Preserves the full branch topology.

```text
`A───B───C───M   ← main    
      \     /       
       D───E     ← feature`
```

- **Pros**
	- You keep a complete history of exactly what happened and when.  
    - Easy to see “this pull request” as a single point (`M`) in the graph.
- **Cons**
    - History can get noisy with many merge commits.
    - Harder to follow a straight line if you want a clean, linear log.

---
## 2. Squash and Merge

- **What it does**: Takes _all_ commits on the feature branch, “squashes” them into a single new commit (`S`), and applies that on top of the target branch.

- **History shape**: Linear, one commit per PR.

```text
A───B───C───S   ← main          
            (squashed D+E)`
```    
-  **Pros**
	- Keeps history very clean and easy to read—each PR is one commit.
    - Removes “WIP,” fixup, or intermediary commits that clutter history.
- **Cons**
    - You lose the individual commit granularity from the branch.
    - Harder to bisect or revert a specific change inside that squashed commit.

---
## 3. Rebase and Merge

- **What it does**: Replays _each_ commit (`D`, `E`, …) from the feature branch onto the tip of the target branch, then fast-forwards without creating a merge commit. 
- **History shape**: Linear, but preserves individual commits.
```text
`A───B───C───D′───E′   ← main`
```

(_D′_ and _E′_ are rebased versions of _D_ and _E_, with new hashes.)
- **Pros**
    - Keeps a clean, straight-line history.  
    - Retains the logical commits you authored.
- **Cons**
    - Rewrites history: hashes change, so force-pushing feature branches is necessary.
    - Can be confusing if multiple developers share the branch and don’t coordinate rebases.

---

### When to Use Which?

- **Create Merge Commit**: when you want to preserve complete context and branch structure—especially for larger features or long-running branches.    
- **Squash and Merge**: when you prefer a tidy, one-commit-per-PR history and don’t need every intermediary change. Great for small, self-contained fixes.
- **Rebase and Merge**: when you like a linear history _and_ want to keep each logical commit, but you’re comfortable with rebasing and its implications (e.g. force pushes).

---

**Summary Table**

| Strategy              | Merge Commit? | Keeps Individual Commits? | History Shape        |
| --------------------- | ------------- | ------------------------- | -------------------- |
| Create a merge commit | ✅             | ✅                         | Branch graph         |
| Squash and merge      | ❌             | ❌ (squashed into one)     | Linear, one commit   |
| Rebase and merge      | ❌             | ✅ (rebased onto main)     | Linear, many commits |

Choose based on your team’s preferences for history clarity vs. context preservation.