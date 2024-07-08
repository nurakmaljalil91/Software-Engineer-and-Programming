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


