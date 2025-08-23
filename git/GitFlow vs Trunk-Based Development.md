## GitFlow

**Branching strategy:** Uses multiple long-lived branches.

 Common branches:       
- `main` (production)
- `develop` (integration branch)
- `feature/*` (new features)
- `release/*` (release prep)
- `hotfix/*` (urgent fixes)

**Workflow:**    

1. Developers create a `feature` branch from `develop`.
2. Work gets merged into `develop`.
3. When ready to release, a `release` branch is created.
4. After testing, `release` merges into both `main` and `develop`.
5. Urgent fixes are handled via `hotfix/*` branches from `main`.

**Pros:** 
- Clear structure for teams with scheduled releases.
- Isolates work and ensures stability in `main`.

**Cons:**
- Heavyweight, many merges.
- Slows down CI/CD.
- High chance of merge conflicts if features live too long.
 
---
## Trunk-Based Development (TBD)

**Branching strategy:** Everyone commits to `main` (the “trunk”) frequently.
- Short-lived branches may exist (a few hours to a day), but they are merged quickly.     

 **Workflow:** 
 1. Developers branch off `main` for small tasks.
 2. Commit small, incremental changes.
 3. Merge to `main` quickly (often multiple times per day).       
 4. Feature flags / toggles are often used to hide unfinished work in production.       

**Pros:** 
- Encourages continuous integration.
- Fewer long-lived branches → fewer merge conflicts.
- Ideal for CI/CD and fast releases.

**Cons:**
- Requires strong testing and automation.
- Developers must coordinate closely.
- Harder for teams used to “big-bang” releases.

---
## When to Use

**GitFlow:** Good for large enterprises with slower release cycles, strong QA gates, and multiple environments.

**Trunk-Based:** Best for teams practicing DevOps, CI/CD, and frequent production deployments.