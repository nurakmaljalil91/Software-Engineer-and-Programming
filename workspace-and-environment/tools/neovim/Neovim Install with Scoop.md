---
title: Neovim Install with Scoop
created: 2026-04-09
updated: 2026-04-09
tags:
  - Neovim
  - scoop
  - windows
category: neovim
status: active
id: Neovim Install with Scoop
---
- Scoop is excellent because it installs apps in your user directory (no Admin pop-ups) and handles "shims" beautifully.

 - If you don't have Scoop, install it via PowerShell:
 
```PowerShell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Out-String | Invoke-Expression
```

- Install [[Neovim]]

```PowerShell
scoop install neovim
```

## Install Ripgrep

```PowerShell
scoop install ripgrep
```

## Install Fd Finder

```PowerShell
scoop install fd
```

## Install Tree-Sitter CLI

```PowerShell
scoop install tree-sitter
```

## Install Zig

```PowerShell
scoop install zig
```

