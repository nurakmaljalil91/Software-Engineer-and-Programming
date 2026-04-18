---
title: Configure WezTerm
category: tools
tags:
  - wezterm
  - terminal
  - configuration
created: 2026-04-18
updated: 2026-04-18
status: active
description: Detailed configuration for WezTerm terminal emulator, including fonts, appearance, and performance settings.
---

## WezTerm Configuration (Lua)

The configuration file is located at `~/.wezterm.lua`.

```lua
local wezterm = require("wezterm")
local config = {}

-- Use the config builder for better error message
if wezterm.config_builder then
	config = wezterm.config_builder()
end

-- Font & Ligatures
config.font = wezterm.font("JetBrainsMono Nerd Font", { weight = "Regular" })
config.font_size = 12.0
config.line_height = 1.1

-- Appearance
config.color_scheme = "Catppuccin Mocha" -- One of the best for dev work
config.window_background_opacity = 0.90 -- Slightly transparent for that Linux look
config.hide_tab_bar_if_only_one_tab = true -- Performance

config.window_decorations = "RESIZE"

config.front_end = "WebGpu"

return config
```

### Key Settings Explained

- **Font**: Uses JetBrainsMono Nerd Font for best compatibility with icons (like those in Oh My Posh).
- **Opacity**: Set to `0.90` to give a modern, slightly transparent look typical of Linux power-user setups.
- **Front End**: `WebGpu` is preferred on systems with modern GPUs (like Pop!_OS with NVIDIA) for high-performance rendering.
