---
title: Hyprland NVIDIA GPU Acceleration
category: linux
tags:
  - linux
  - cachyos
  - hyprland
  - nvidia
  - gpu
  - chrome
  - helium
created: 2026-05-20
updated: 2026-05-20
status: active
---

## Overview

Fixes for FPS drops, missing hardware GPU acceleration, and Chrome YouTube freezes on **HP ZBook Power G9** running CachyOS with Hyprland and a hybrid Intel Iris Xe + NVIDIA RTX A1000 GPU setup.

Related: [[CachyOS]], [[Configure Hyprland]]

## Symptoms

- FPS drops when watching YouTube in Chrome or Helium
- `chrome://gpu` shows all features as "Software only"
- High CPU usage during video playback
- Chrome freezes or shows no video when opening YouTube

## Root Causes

1. **Wrong VA-API device for browser video** — forcing `LIBVA_DRIVER_NAME=nvidia` and `/dev/dri/renderD128` sends Chrome YouTube decode through NVIDIA VA-API, which can freeze on this hybrid Wayland setup
2. **Stale Chrome flags** — `--use-gl=egl` was removed in Chrome 128+, causing the GPU process to crash on every startup
3. **Vulkan + Wayland incompatibility** — Chrome/Helium enable Vulkan by default on NVIDIA, but it is incompatible with `--ozone-platform=wayland`, causing crash loops
4. **No-op `AQ_DRM_NO_ATOMIC=1`** — disables atomic modesetting in Hyprland's Aquamarine backend, degrading frame sync during video
5. **Forced VA-API blocklist bypass** — `VaapiIgnoreDriverChecks` and `--ignore-gpu-blocklist` can push Chrome onto an unstable decode path

## Fix

### 1. Confirm Render Node Mapping

On this HP ZBook Power G9:

| Render node | GPU | Vendor |
|---|---|---|
| `/dev/dri/renderD128` | NVIDIA RTX A1000 | `0x10de` |
| `/dev/dri/renderD129` | Intel Iris Xe | `0x8086` |

Check:

```bash
for f in /sys/class/drm/renderD*/device/vendor /sys/class/drm/renderD*/device/device; do
  printf '%s: ' "$f"
  cat "$f"
done
```

Use Intel VA-API for browser video decode. Keep NVIDIA for explicit offload or GL/Vulkan workloads, not as the global browser decode target.

### 2. Install Intel VA-API Driver

```bash
sudo pacman -S intel-media-driver libva-utils
```

Verify:

```bash
LIBVA_DRIVER_NAME=iHD LIBVA_DRM_DEVICE=/dev/dri/renderD129 vainfo
```

Expected output: entries for `VAProfileH264`, `VAProfileVP9Profile0`, `VAProfileAV1Profile0`, etc.

### 3. Update Hyprland Config

File: `~/.config/hypr/hyprland.conf`

Use Intel media decode globally:

```conf
# Use Intel media decode for browsers/video. renderD128 is NVIDIA on this laptop;
# renderD129 is the Intel iGPU, which is the stable VA-API path for YouTube.
env = LIBVA_DRIVER_NAME,iHD
env = LIBVA_DRM_DEVICE,/dev/dri/renderD129
# env = NVD_BACKEND,direct
```

Avoid this for Chrome/YouTube on this laptop:

```conf
env = LIBVA_DRIVER_NAME,nvidia
env = LIBVA_DRM_DEVICE,/dev/dri/renderD128
env = NVD_BACKEND,direct
```

Remove this line if present (it disables atomic modesetting):

```conf
env = AQ_DRM_NO_ATOMIC,1
```

Do **not** set `__EGL_VENDOR_LIBRARY_FILENAMES` globally — CachyOS's `/etc/profile.d/nvidia-rtd3-workaround.sh` manages this automatically for the hybrid GPU setup.

### 4. Configure Google Chrome

File: `~/.config/chrome-flags.conf`

```text
--ozone-platform=wayland
--use-vulkan=disabled
--enable-features=WaylandWindowDecorations
--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE,VaapiVideoDecoder,VaapiVideoDecodeLinuxGL,VaapiVideoEncoder
```

Key flags explained:

| Flag | Reason |
|---|---|
| `--use-vulkan=disabled` | Disables Vulkan at GPU process level — Vulkan is incompatible with Wayland on NVIDIA |
| `--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE` | Disables Vulkan at the Chrome feature layer |
| `WaylandWindowDecorations` | Keeps Chrome native on Wayland with window decorations |
| `--disable-features=...,VaapiVideoDecoder,...` | Avoids Chrome freezing on YouTube by not forcing the unstable VA-API decode path |
| Do **not** use `--use-gl=egl` | Removed in Chrome 128+, causes GPU process crash loop |
| Do **not** use `--enable-zero-copy` | Triggers DMA-BUF `eglCreateImage` failure on NVIDIA Wayland |
| Do **not** use `VaapiIgnoreDriverChecks` or `--ignore-gpu-blocklist` | Can force Chrome onto unstable NVIDIA VA-API decode |

### 5. Configure Helium Browser

File: `~/.config/helium-browser-flags.conf`

```text
--ozone-platform=wayland
--use-vulkan=disabled
--enable-features=WaylandWindowDecorations
--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE,VaapiVideoDecoder,VaapiVideoDecodeLinuxGL,VaapiVideoEncoder
```

Helium reads flags from `~/.config/helium-browser-flags.conf` via its wrapper script at `/opt/helium-browser-bin/helium-wrapper`. System-wide flags can also be placed in `/etc/helium-browser-flags.conf`.

### 6. Clear Chrome GPU Crash State (if needed)

If Chrome previously disabled GPU due to crash loops, clear the cached state:

```bash
pkill -f chrome
python3 - <<'EOF'
import json
path = '/home/amal/.config/google-chrome/Local State'
with open(path, 'r') as f:
    data = json.load(f)
removed = [k for k in list(data.keys()) if any(x in k.lower() for x in ['gpu', 'crash', 'hardware_accel'])]
for k in removed:
    del data[k]
with open(path, 'w') as f:
    json.dump(data, f, separators=(',', ':'))
print("Cleared:", removed)
EOF
```

## Verification

After restarting Chrome or Helium, go to `chrome://gpu` and confirm:

| Feature | Expected |
|---|---|
| Canvas | Hardware accelerated |
| Compositing | Hardware accelerated |
| OpenGL | Enabled |
| Rasterization | Hardware accelerated on all pages |
| Video Decode | Software only or hardware accelerated on Intel; either is acceptable if YouTube plays without freezing |
| WebGL | Hardware accelerated |
| Video Encode | Software only is acceptable |

Verify Intel VA-API is available:

```bash
LIBVA_DRIVER_NAME=iHD LIBVA_DRM_DEVICE=/dev/dri/renderD129 vainfo
```

Expected driver: `Intel iHD driver for Intel(R) Gen Graphics`.

Restart Chrome completely after changing flags:

```bash
pkill -f chrome
google-chrome-stable
```

## Hyprland Config Reference

Relevant section of `~/.config/hypr/hyprland.conf` for this setup:

```conf
env = LIBVA_DRIVER_NAME,iHD
env = LIBVA_DRM_DEVICE,/dev/dri/renderD129
# env = NVD_BACKEND,direct
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
```
