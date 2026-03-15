Here’s a compact Git cheat-sheet, organized by task. Each bullet shows the basic command—feel free to add flags (`-h`, `--help`) to learn more.

---

### **Setup & Configuration**

```bash
git config --global user.name "Your Name"
git config --global user.email you@example.com
git config --global core.editor <editor>
git config --list
```

### **Create & Clone Repos**

```bash
git init
git clone <repo-url>
git clone --branch <branch> <repo-url>
```

### **Basic Snapshotting**

```bash
git status
git add <file>...
git add .
git reset <file>           # unstage
git commit -m "message"
git commit --amend
```

### **Branching & Checking Out**

```bash
git branch                 # list branches
git branch <name>          # create branch
git branch -d <name>       # delete branch
git checkout <branch>
git checkout -b <name>     # new + switch
git switch <branch>
git switch -c <name>
```

### **Merging & Rebasing**

```bash
git merge <branch>
git merge --no-ff <branch>
git rebase <upstream>
git rebase --onto <newbase> <oldbase> <branch>
git rebase --abort
git rebase --continue
```

### **Squash & Fixup**

```bash
git rebase -i HEAD~<n>     # pick/squash/fixup in editor
git merge --squash <branch>
git commit                 # after squash merge
```

### **Undoing Changes**

```bash
git checkout -- <file>     # discard working-dir changes
git revert <commit>        # inverse commit
git reset --soft HEAD~1    # uncommit, keep changes
git reset --mixed HEAD~1   # uncommit + unstage
git reset --hard HEAD~1    # destroy changes
```

### **Stashing**

```bash
git stash
git stash save "msg"
git stash list
git stash apply [<stash>]
git stash pop
git stash drop [<stash>]
```

### **Remote Repos**

```bash
git remote                   # list
git remote -v                # verbose list
git remote add <name> <url>
git remote remove <name>
git fetch <remote>
git fetch --all
git pull [<remote> [<branch>]]
git push [<remote>] [<branch>]
git push --set-upstream <remote> <branch>
git push --force-with-lease
```

### **Tags**

```bash
git tag                         # list tags
git tag <name>                  # lightweight
git tag -a <name> -m "msg"      # annotated
git push <remote> --tags
git tag -d <name>
```

### **Inspecting History**

```bash
git log
git log --oneline
git log --graph --decorate --all
git show <commit>
git diff [<commit>] [<commit>] [--] [<path>]
git diff --staged
```

### **Searching & Bisect**

```bash
git grep "pattern" [--] [<path>]
git bisect start
git bisect bad
git bisect good <commit>
git bisect reset
```

### **Cherry-Picking**

```bash
git cherry-pick <commit>
git cherry-pick --abort
```

### **Submodules**

```bash
git submodule add <url> [<path>]
git submodule update --init --recursive
git submodule foreach git pull
```
### **Reflog & Recovery**

```bash
git reflog
git reset --hard HEAD@{<n>}
```

### **Cleaning**

```bash
git clean -n    # dry-run
git clean -f    # remove untracked files
git clean -fd   # also remove dirs
```

### **Archiving**

```bash
git archive --format=zip HEAD > source.zip
```

---

Keep this list handy—or turn it into a one-pager for your desk. Happy versioning!