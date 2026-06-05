---
title: GNU Stow
category: tools
tags:
  - tools
  - linux
  - dotfiles
created: 2026-06-05
updated: 2026-06-05
status: active
---

## Overview

[[GNU Stow]] is a symlink farm manager which takes separate packages of software and/or data located in separate directories on the filesystem, and makes them appear to be installed in the same place.

In the context of dotfiles management, Stow is used to centralize all configuration files in one directory (often a Git repository) and symlink them to their expected locations (usually the home directory).

## Installation

### Arch Linux (CachyOS)
```bash
sudo pacman -S stow
```

### Ubuntu / Debian (Pop!_os)
```bash
sudo apt update
sudo apt install stow
```

## How It Works

Stow creates symlinks from the "stow directory" to a "target directory". By default, the target directory is the parent of the directory where Stow is run.

### Typical Directory Structure
```text
~/dotfiles/
├── nvim/
│   └── .config/
│       └── nvim/
│           └── init.lua
├── tmux/
│   └── .tmux.conf
└── zsh/
    └── .zshrc
```

## Basic Usage

Navigate to your dotfiles directory:
```bash
cd ~/dotfiles
```

### Stow a package
To symlink the contents of the `nvim` folder to the home directory:
```bash
stow nvim
```
This will create a symlink at `~/.config/nvim/init.lua` pointing to `~/dotfiles/nvim/.config/nvim/init.lua`.

### Unstow a package
To remove the symlinks:
```bash
stow -D nvim
```

### Restow a package
Useful if you added new files or changed the structure:
```bash
stow -R nvim
```

### Specify Target Directory
If you are not running stow from your home directory's subfolder:
```bash
stow -t ~ nvim
```

## Tips

- **Dotfile Repository:** Keep your `dotfiles` directory in a [[Git]] repository to backup and sync across machines.
- **Ignoring files:** Use a `.stow-local-ignore` file to exclude files from being symlinked.

## Related Notes
- [[Linux]]
- [[Git]]
- [[Pop!_os]]
- [[CachyOS]]
