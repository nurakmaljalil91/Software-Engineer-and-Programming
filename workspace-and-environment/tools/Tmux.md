---
title: Tmux
category: tools
tags:
  - tmux
  - terminal
  - linux
  - productivity
created: 2026-05-17
updated: 2026-05-17
status: active
description: A terminal multiplexer that allows multiple terminal sessions to be accessed simultaneously in a single window.
---

## Overview

[[Tmux]] (Terminal Multiplexer) is a powerful tool for managing multiple terminal sessions. It allows you to create, access, and control several terminals from a single screen. Most importantly, it keeps sessions alive even if you disconnect.

## Configuration

The configuration file is located at `~/.tmux.conf`.

### Local Setup Highlights

Based on the current `.tmux.conf`, the following custom settings are applied:

- **Mouse Support**: Enabled (`set -g mouse on`).
- **Theme**: Catppuccin Mocha flavor.
- **Status Bar**: Positioned at the bottom with detailed info (Host, Git Branch, Current Directory, Time, Date).
- **Reloading**: Press `Prefix + r` to reload the configuration without restarting tmux.

### Custom Keybindings

The following Vim-like navigation bindings are configured for panes:

| Command | Action |
| --- | --- |
| `Prefix + h` | Select pane to the **Left** |
| `Prefix + j` | Select pane **Down** |
| `Prefix + k` | Select pane **Up** |
| `Prefix + l` | Select pane to the **Right** |

### Automated Scrolling

A custom script is configured to automatically enter **Copy Mode** when scrolling up with the mouse wheel, and exit when scrolling back down to the bottom.

## Basic Usage

### Session Management

| Action | Command |
| --- | --- |
| New Session | `tmux` |
| New Named Session | `tmux new -s <name>` |
| List Sessions | `tmux ls` |
| Attach to Session | `tmux a -t <name>` |
| Kill Session | `tmux kill-session -t <name>` |
| Detach Session | `Prefix + d` |

### Windows (Tabs)

| Action | Shortcut |
| --- | --- |
| Create Window | `Prefix + c` |
| Next Window | `Prefix + n` |
| Previous Window | `Prefix + p` |
| List Windows | `Prefix + w` |
| Rename Window | `Prefix + ,` |

### Panes (Splits)

| Action | Shortcut |
| --- | --- |
| Vertical Split | `Prefix + %` |
| Horizontal Split | `Prefix + "` |
| Close Pane | `Prefix + x` or `Ctrl + d` |
| Swap Pane | `Prefix + {` or `Prefix + }` |

## Plugin Management

This setup uses **TPM (Tmux Plugin Manager)**.

- **Installation Path**: `~/.configurations/tmux/plugins/tpm/tpm`
- **Installed Plugins**:
  - `tmux-plugins/tpm`
  - `catppuccin/tmux`

### TPM Shortcuts

| Action | Shortcut |
| --- | --- |
| Install Plugins | `Prefix + I` (Capital I) |
| Update Plugins | `Prefix + U` |
| Uninstall Plugins | `Prefix + alt + u` |

## Tips

- Use `Prefix + [` to enter **Copy Mode** manually if needed (useful for searching through scrollback).
- Sessions persist even if the terminal emulator is closed or the SSH connection drops.
