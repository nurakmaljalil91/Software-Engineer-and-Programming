---
title: Configure Hyprland
category: tools
tags:
  - hyprland
  - wayland
  - nvidia
  - linux
created: 2026-05-17
updated: 2026-05-17
status: active
---

## Overview

Hyprland is a dynamic tiling Wayland compositor based on wlroots that doesn't sacrifice on appearance. This note explains the configuration found in `~/.config/hypr/hyprland.conf`, specifically optimized for NVIDIA hybrid graphics (Intel + NVIDIA).

Related: [[CachyOS]], [[Linux]], [[Configure WezTerm]], [[Connect to WiFi in CachyOS]]

## Configuration Analysis

The configuration is structured to handle the complexities of NVIDIA on Wayland while maintaining a clean and functional desktop environment.

### 1. Environment Variables (NVIDIA Hybrid Fixes)

These variables are critical for stability on hybrid graphics systems.

| Variable | Description |
|---|---|
| `LIBVA_DRIVER_NAME=nvidia` | Forces VA-API to use NVIDIA. |
| `WLR_NO_HARDWARE_CURSORS=1` | Fixes invisible cursor issues on NVIDIA. |
| `__GLX_VENDOR_LIBRARY_NAME=nvidia` | Ensures GLX uses NVIDIA. |
| `ELECTRON_OZONE_PLATFORM_HINT=wayland` | Enables native Wayland support for Electron apps (VS Code, Discord). |
| `AQ_DRM_NO_ATOMIC=1` | Can help with flickering/stability on some NVIDIA setups. |

### 2. Monitor Setup

```conf
monitor=,preferred,auto,1
```
- Sets the monitor to its preferred resolution and automatically handles positioning.
- The `1` at the end indicates a scale factor of 1 (no scaling).

### 3. Input & Gestures

- **Keyboard:** Uses the standard `us` layout.
- **Mouse:** `follow_mouse = 1` (focus follows mouse movement).
- **Touchpad:** `natural_scroll = true` enabled for a more modern scrolling feel.

### 4. General & Layout

- **Gaps:** 5px internal (between windows), 10px external (between window and screen edge).
- **Layout:** Uses `dwindle`, which recursively splits the screen as new windows are opened.
- **NVIDIA Specific:** `allow_tearing = false` is set to prevent flickering.

### 5. Decoration & Aesthetics

- **Rounding:** 10px corners for a soft look.
- **Shadows & Blur:** Both are **disabled** in this config to prevent "gray box" artifacts and flickering often seen on NVIDIA hybrid setups.

### 6. Key Bindings

The `$mainMod` is set to `SUPER` (Windows key).

| Keybind | Action | Command/App |
|---|---|---|
| `SUPER + Q` | Open Terminal | `wezterm` |
| `SUPER + C` | Close Window | `killactive` |
| `SUPER + M` | Exit Hyprland | `hyprctl dispatch exit` |
| `SUPER + E` | File Manager | `dolphin` |
| `SUPER + V` | Toggle Floating | `togglefloating` |
| `SUPER + R` | App Launcher | `rofi -show drun` |
| `SUPER + B` | Open Browser | `google-chrome-stable` |
| `SUPER + H/J/K/L` | Move Focus | Left/Down/Up/Right |
| `SUPER + 1-9` | Switch Workspace | Workspace 1-9 |
| `SUPER + SHIFT + 1-4` | Move Window | To Workspace 1-4 |
| `SUPER + SHIFT + H/L` | Cycle Workspaces | Previous/Next |

### 7. Startup Applications

- **Wallpaper:** `swaybg` sets the CachyOS GreenSpace wallpaper.
- **Status Bar:** `waybar` is launched on startup.

## Troubleshooting NVIDIA Issues

If you experience flickering or "gray boxes":
1. Ensure `blur` and `shadow` are disabled in the `decoration` section.
2. Verify `WLR_NO_HARDWARE_CURSORS=1` is set in the environment variables.
3. Check if `AQ_DRM_NO_ATOMIC=1` helps or hinders stability.
