---
title: Configure CachyOS and Hyprland
category: linux
tags:
  - linux
  - cachyos
  - hyprland
  - configuration
created: 2026-05-23
updated: 2026-05-23
status: active
---

## Overview

Configuration notes for CachyOS + Hyprland on **HP ZBook Power G9**. Covers keybindings, brightness, Waybar customisation, and system-level tweaks.

Related: [[CachyOS]], [[Hyprland NVIDIA GPU Acceleration]]

---

## Customize Brightness

### Tools

| Tool | Purpose |
|---|---|
| `brightnessctl` | Reads and writes backlight brightness |
| `swayosd` | On-screen overlay indicator (shows brightness bar) |

Install both:

```bash
sudo pacman -S brightnessctl swayosd
```

### How it works on HP ZBook G9

The BIOS has **Action Keys Mode enabled**, which means the Fn row sends media keycodes directly without pressing Fn. So:

- `Fn + F3` → the system sees plain `F3`
- `Fn + F4` → the system sees plain `F4`

Binding to `XF86MonBrightnessDown` / `XF86MonBrightnessUp` does **not** work on this machine because of the BIOS setting.

### Hyprland keybinds (`hyprland.conf`)

```ini
# Brightness (F3 = down, F4 = up — BIOS Action Keys Mode is enabled)
bindel = , F3, exec, swayosd-client --brightness lower
bindel = , F4, exec, swayosd-client --brightness raise
```

`bindel` (not `bind`) allows the key to repeat while held down.

### Start SwayOSD on login

Add to the startup section in `hyprland.conf`:

```ini
exec-once = swayosd-server
```

### Backlight device

The backlight is exposed at:

```
/sys/class/backlight/intel_backlight/
```

Max brightness: `96000`. SwayOSD/brightnessctl both target this device automatically.

---

## Customize Volume and Microphone

Audio is handled by **PipeWire** with `wpctl`. SwayOSD shows volume and mute overlays.

### How it works on HP ZBook G9

BIOS Action Keys Mode maps Fn+F5–F8 to plain `F5`–`F8`. Confirmed working.

### Hyprland keybinds (`hyprland.conf`)

```ini
# Volume (BIOS Action Keys Mode — plain F5–F8)
bind   = , F5, exec, swayosd-client --output-volume mute-toggle
bindel = , F6, exec, swayosd-client --output-volume lower
bindel = , F7, exec, swayosd-client --output-volume raise
bind   = , F8, exec, swayosd-client --input-volume mute-toggle
```

| Key | Action |
|-----|--------|
| F5 | Toggle speaker mute |
| F6 | Volume down (repeats while held) |
| F7 | Volume up (repeats while held) |
| F8 | Toggle microphone mute |

`bind` is used for mute toggles (one-shot); `bindel` for volume steps (repeatable).

---

## Fn Row — F9 to F12

### F9 — Keyboard Backlight

**Not available.** This ZBook G9 has no software-controllable keyboard backlight device exposed via `/sys/class/leds/` or the `hp-wmi` platform interface. F9 is left unbound.

### F10 — Insert Key

Requires `wtype` (Wayland key simulator):

```bash
sudo pacman -S wtype
```

```ini
bind = , F10, exec, wtype -k insert
```

### F11 + F12 — Airplane Mode Toggle

Script at `~/.config/hypr/scripts/airplane_mode.sh`:

```bash
#!/bin/bash
if rfkill list wifi | grep -q "Soft blocked: yes"; then
    rfkill unblock all
    notify-send "Airplane Mode" "OFF — Wireless enabled"
else
    rfkill block all
    notify-send "Airplane Mode" "ON — Wireless disabled"
fi
```

```ini
bind = , F11, exec, bash ~/.config/hypr/scripts/airplane_mode.sh
bind = , F12, exec, bash ~/.config/hypr/scripts/airplane_mode.sh
```
