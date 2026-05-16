---
title: Pacman Basic Usage
category: tools
tags:
  - arch
  - linux
  - package-manager
created: 2026-05-17
updated: 2026-05-17
status: active
---

## Overview

`pacman` is the default package manager for Arch Linux and its derivatives (like CachyOS). It combines a simple binary package format with an easy-to-use build system.

Related: [[Linux]], [[CachyOS]]

## Essential Commands

### 1. Synchronize and Update
It is highly recommended to update the entire system before installing any new packages.

```bash
sudo pacman -Syu
```
- `-S`: Sync
- `-y`: Refresh repositories (download fresh package list)
- `-u`: Upgrade all out-of-date packages

### 2. Installing Packages

**Install a specific package:**
```bash
sudo pacman -S package_name
```

**Install without updating system (Not Recommended):**
```bash
sudo pacman -Sy package_name
```

### 3. Removing Packages

**Remove a package:**
```bash
sudo pacman -R package_name
```

**Remove a package and its dependencies (not used by other packages):**
```bash
sudo pacman -Rs package_name
```

**Remove a package, its dependencies, and configuration files:**
```bash
sudo pacman -Rns package_name
```

### 4. Searching and Information

**List all installed packages:**
```bash
pacman -Q
```

**List explicitly installed packages (not dependencies):**
```bash
pacman -Qe
```

**Search for a package in the repositories:**
```bash
pacman -Ss keyword
```

**Search for a package in installed local database:**
```bash
pacman -Qs keyword
```

**Display information about a specific package:**
```bash
pacman -Si package_name   # From repositories
pacman -Qi package_name   # From local database
```

### 5. Cleaning the Cache
Pacman stores downloaded packages in `/var/cache/pacman/pkg/`.

**Remove old versions of cached packages:**
```bash
sudo pacman -Sc
```

**Remove all cached packages:**
```bash
sudo pacman -Scc
```

## Tips for Arch/CachyOS Users

- **Avoid Partial Upgrades:** Never run `pacman -Sy`. Always use `pacman -Syu` to ensure system stability.
- **Check for .pacnew files:** After an update, pacman might create `.pacnew` files for configurations you've modified. Use `pacdiff` to manage them.
- **AUR Helpers:** For packages in the Arch User Repository, use helpers like `paru` or `yay`, which share similar syntax with pacman.
