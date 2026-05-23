---
title: Connect to WiFi in CachyOS
category: linux
tags:
  - linux
  - cachyos
  - networking
  - hyprland
created: 2026-05-23
updated: 2026-05-23
status: active
---

## Overview

CachyOS uses **NetworkManager** by default to handle network connections. In a Hyprland environment, you can manage Wi-Fi connections using Terminal-based User Interfaces (TUI), Command Line Interfaces (CLI), or Graphical User Interfaces (GUI) integrated into the status bar.

Related: [[CachyOS]], [[Linux]], [[Configure Hyprland]]

## 1. Using nmtui (Recommended for TUI)

`nmtui` is a text-based interface for NetworkManager and is the most straightforward way to connect to Wi-Fi from the terminal.

1. Open your terminal (`SUPER + Q`).
2. Type `nmtui` and press Enter.
3. Select **Activate a connection**.
4. Find your Wi-Fi network in the list.
5. Press Enter, type the password, and select **OK**.
6. Back out and quit the application.

## 2. Using nmcli (Command Line)

For pure command-line usage or scripting, use `nmcli`.

- **Scan for networks:**
  ```bash
  nmcli device wifi rescan
  nmcli device wifi list
  ```

- **Connect to a network:**
  ```bash
  nmcli device wifi connect "SSID_NAME" password "YOUR_PASSWORD"
  ```

- **Check connection status:**
  ```bash
  nmcli connection show
  ```

## 3. Using nm-applet (GUI / Tray)

If you prefer a tray icon in your [[Waybar]]:

1. Ensure `network-manager-applet` is installed:
   ```bash
   sudo pacman -S network-manager-applet
   ```
2. Add it to your Hyprland startup config (`~/.config/hypr/hyprland.conf`):
   ```conf
   exec-once = nm-applet --indicator
   ```
3. Restart Hyprland (`SUPER + M` then log back in, or just run `nm-applet &` in terminal).

## 4. Troubleshooting

### Verify NetworkManager Service
If you cannot see any networks, ensure the service is running:

```bash
sudo systemctl status NetworkManager
```

If it's stopped, start and enable it:

```bash
sudo systemctl enable --now NetworkManager
```

### Wi-Fi is Hard-Blocked (RF-Kill)
If the Wi-Fi card is disabled at the hardware level:

```bash
rfkill list
# If blocked, unblock it:
sudo rfkill unblock wifi
```

### Driver Issues
For the **HP ZBook Power G9**, ensure you have the appropriate firmware (usually `linux-firmware` which comes pre-installed in CachyOS).
