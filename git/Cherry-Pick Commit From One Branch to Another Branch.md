
## Steps
### Switch to your target branch

First, make sure you are on the branch you want to apply commits to:

```bash
git checkout target-brach
```

### Identify  the commits you need

List the commit SHAs on the source branch:

```bash
git log --oneline source-branch
# e.g.
# a1b2c3d Fix typo in README
# e4f5g6h Add error-handling in API
# i7j8k9l Improve performance of parser
```

Copy the SHAs(`a1b2c3d`, `e4f5g6h`, etc.) of the commits you want

### Cherry-pick by listing individual SHAs

f you need only _specific_ commits (non-contiguous), pass them all in one command:

```bash
git cherry-pick a1b2c3d e4f5g6h i7j8k9l
```

Git will replay them in the order you list.
### Cherry-pick a contiguous range

If the commits you want form a continuous sequence on the source branch, you can use the `..<end>` range syntax.

`A..B` means: “all commits reachable from `B` _excluding_ those reachable from `A`.”

If you actually want `A` through `B` inclusive, do `A^..B`.

Example: on `source-branch` you have commits

```css
X — A — B — C — D  (HEAD)
```

To pick **B, C, D**:

```bash
git cherry-pick A..D
```

To pick **A, B, C**:

```bash
git cherry-pick A^..C
````
### Resolving conflicts

If Git stops with a conflict:

Edit the conflicted files to fix.
   
```bash
git add <file1> <file2> … git cherry-pick --continue
```

If you decide you don’t want to proceed:

```bash
git cherry-pick --abort
```
### Optional flags


`--no-commit` (or `-n`): apply the changes to your working tree/index but don’t create the commit yet.

`-x`: appends “(cherry-picked from commit …)” to the commit message:

```bash
git cherry-pick -x a1b2c3d
```

### Verify the result

Once done, you can confirm that the commits arrived cleanly:

```bash
git log --oneline
```

You should see the cherry-picked commits at the top of your history.
### Summary

`git checkout target-branch`

Find SHAs on `source-branch` (`git log`)

Either
    - `git cherry-pick sha1 sha2 sha3`
    - or `git cherry-pick <old-sha>.. <new-sha>`
    
Resolve conflicts (`git add` + `git cherry-pick --continue`)

Verify with `git log`

That’s it! You’ve now replayed just the commits you wanted onto your target branch.