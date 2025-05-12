## What is a Git Submodule

A submodule allows you to keep a Git repository as a subdirectory of another Git repository. It's commonly used to include libraries or modules that are maintained in separate repos.
## Add a Submodule

- Use the command:

```bash
git submodule add <repository-url> [path]
```

- Example:

```bash
git submodule add https://github.com/example/libcool.git external/libcool
```

## Clone a Repository with Submodules

- Clone with all submodules:

```bash
git clone --recurse-submodules <repository-url>
```

- If already cloned:

```bash
git submodule init
git submodule update
```

## Update a Submodule

- Navigate to the submodule directory:

```bash
cd external/libcool
```

- Pull the latest changes:

```bash
git checkout main
git pull origin main
```

- Go back to the root and commit the update:
  
```bash
git add external/libcool
git commit -m "Update libcool submodule"
```

## Update All Submodules Recursively

- Pull latest commits from all submodules:
  
```bash
git submodule update --remote --merge --recursive
```

## Remove a Submodule

- Remove tracking and the folder:
  
```bash
git submodule deinit -f external/libcool
git rm -f external/libcool
rm -rf .git/modules/external/libcool
```

## Nested Submodules

If a submodule contains its own submodules (nested):

- Add the parent submodule:
  
```bash
git submodule add https://github.com/example/RepoA.git external/RepoA
```

- Initialize and update all nested submodules:
  
```bash
git submodule update --init --recursive
```

- When cloning the main project with nested submodules:
  
```bash
git clone --recurse-submodules <repository-url>
```

- To update all nested submodules:
  
```bash
git submodule update --remote --recursive
```

## Notes

- Submodules are locked to a specific commit. If the submodule is updated, you need to commit the change in the parent repository.
- The `.gitmodules` file stores the path and URL of each submodule.
