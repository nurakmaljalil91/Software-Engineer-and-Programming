## Overview

[[Git]] is a distributed version control that tracks versions of files.

## Installing 

Install [[Git]] from [Git (git-scm.com)](https://git-scm.com/). Check if the [[Git]] is install by run `git --version`.

```bash
$ git --version
git version 2.39.1.windows.1
```

## Updating

If your [[Git]] version is greater than `Git 2.16.1(2)` , use this command

```bash
git update-git-for-windows
```
## Configure

Configure Git config.

```bash
git config --global user.name "nurakmaljalil91"
git config --global user.email "nurakmaljalil91@gmail.com"
```

## Creating Project folder

Create some project for example:

```bash
mkdir my-project
mkdir my-project/src
cd my-project
echo. 2>CMakeLists.txt
cd src
echo. 2>main.cpp
```

or in Linux:

```bash
mkdir my-project
mkdir my-project/src
cd my-project
touch CMakeLists.txt
cd src
touch main.cpp
```

This will create a project with this structure.

```
|-src
| |- main.cpp
CMakeLists.txt
```

## Initialize Git to Project folder

Initialize [[Git]] to the project folder using `git init`.

```bash
cd /my-project
git init
```

> NOTE
> for Windows, open the view -> show -> hidden items, `.git` will show up

## Adding Files to Staging Area

Once you have initialized the [[Git]] repository in your project folder, you can add files to the staging area. The staging area allows you to decide which changes you want to include in your next commit.

To add files to the staging area, use the `git add` command:

```bash
git add CMakeLists.txt src/main.cpp
```

To add all files in the project directory, you can use:

```bash
git add .
```

or use

```bash
git add -A
```

## Committing Changes

After adding files to the staging area, you need to commit these changes to the repository. A commit is a snapshot of the changes in the project at a specific point in time.

To commit the changes, use the `git commit` command:

```bash
git commit -m "Initial commit"
```

The `-m` option allows you to add a commit message that describes the changes.

## Checking Status

To check the status of your working directory and the staging area, use the `git status` command:

```bash
git status
```

This will show you which files are in the staging area, which files have changes that are not yet staged, and which files are not being tracked by [[Git]].

## Viewing Commit History

To view the commit history, use the `git log` command:

```bash
git log
```

This will display a list of all commits made to the repository, along with their commit messages, authors, and dates.

## Creating a Remote Repository

If you want to collaborate with others or keep a backup of your repository, you can create a remote repository. A remote repository is typically hosted on a service like [GitHub](https://github.com/), GitLab, or Bitbucket.

To create a remote repository on GitHub:

1. Go to [GitHub](https://github.com/) and log in to your account.
2. Click on the "+" icon in the upper-right corner and select "New repository."
3. Enter a name for your repository and click "Create repository."

## Adding a Remote Repository

Once you have created a remote repository, you need to add it to your local repository. Use the `git remote add` command:

```bash
git remote add origin https://github.com/your-username/your-repository.git
```

Replace `your-username` and `your-repository` with your [GitHub](https://github.com/) username and the name of your repository.

## Pushing Changes to Remote Repository

To push your local commits to the remote repository, use the `git push` command:

```bash
git push -u origin master
```

The `-u` option sets the remote repository as the default for future pushes.

## Pulling Changes from Remote Repository

To pull changes from the remote repository to your local repository, use the `git pull` command:

```bash
git pull origin master
```

This will update your local repository with the changes from the remote repository.
## Branching

Branches allow you to work on different features or fixes independently of the main codebase. To create a new branch, use the `git branch` command:

```bash
git branch new-feature
```

To switch to the new branch, use the `git checkout` command:

```bash
git checkout new-feature
```

Or, you can create and switch to a new branch in one command:

```bash
git checkout -b new-feature
```

## Merging Branches

Once you have completed work on a branch, you can merge it back into the main branch. First, switch to the branch you want to merge into (e.g., `master`):

```bash
git checkout master
```

Then, use the `git merge` command to merge the changes:

```bash
git merge new-feature
```
## Resolving Merge Conflicts

Sometimes, there may be conflicts when merging branches. [[Git]] will mark the conflicts in the affected files. To resolve the conflicts, open the files and edit them to remove the conflict markers. After resolving the conflicts, add the resolved files to the staging area and commit the changes:

```bash
git add . git commit -m "Resolved merge conflicts"
```

## Viewing Differences Between Commits

[[Git]] allows you to see the differences between various commits, which can be useful for code reviews and tracking changes. To view differences between your working directory and the staging area, use the `git diff` command:

```bash
git diff
```

To see the differences between the staging area and the latest commit, use:

```bash
git diff --staged
```

To view the differences between two specific commits, use:

```bash
git diff commit1 commit2
```

Replace `commit1` and `commit2` with the hash IDs of the commits you want to compare. You can find these IDs using the `git log` command.

## Using .gitignore

The `.gitignore` file tells [[Git]] which files (or patterns) it should ignore. This is useful for excluding files that are not necessary to be tracked, such as build artifacts, temporary files, or sensitive information.

To create a `.gitignore` file, simply add a file named `.gitignore` in the root of your repository:

```bash
cd /my-project 
touch .gitignore
```

Then, open the `.gitignore` file in a text editor and specify the files or patterns you want [[Git]] to ignore. For example:

```bash
# Ignore all .log files 
*.log  

# Ignore all files in the build/ directory 
build/  

# Ignore a specific file 
secret.txt  

# Ignore all .DS_Store files (commonly created by macOS) 
.DS_Store
```

Here are some common patterns used in `.gitignore` files:

- `*.log` - ignores all files ending in `.log`.
- `build/` - ignores the entire `build` directory.
- `secret.txt` - ignores a specific file named `secret.txt`.
- `*.class` - ignores all files ending in `.class` (useful for [[Java]] projects).
## Conclusion

This tutorial has covered the basic workflow of using [[Git]] for version control, including installing [[Git]], configuring it, creating and initializing a repository, adding and committing changes, working with remote repositories, branching, and merging. With these basics, you can start using [[Git]] to manage your projects and collaborate with others effectively.

