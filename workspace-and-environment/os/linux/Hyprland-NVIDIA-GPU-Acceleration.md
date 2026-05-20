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

Fixes for FPS drops and missing hardware GPU acceleration on **HP ZBook Power G9** running CachyOS with Hyprland and a hybrid Intel Iris Xe + NVIDIA RTX A1000 GPU setup.

Related: [[CachyOS]], [[Configure Hyprland]]

## Symptoms

- FPS drops when watching YouTube in Chrome or Helium
- `chrome://gpu` shows all features as "Software only"
- High CPU usage during video playback

## Root Causes

1. **Missing VA-API driver** — `libva-nvidia-driver` not installed, so Chrome/Helium decode video in software on the CPU
2. **Stale Chrome flags** — `--use-gl=egl` was removed in Chrome 128+, causing the GPU process to crash on every startup
3. **Vulkan + Wayland incompatibility** — Chrome/Helium enable Vulkan by default on NVIDIA, but it is incompatible with `--ozone-platform=wayland`, causing crash loops
4. **No-op `AQ_DRM_NO_ATOMIC=1`** — disables atomic modesetting in Hyprland's Aquamarine backend, degrading frame sync during video

## Fix

### 1. Install NVIDIA VA-API Driver

```bash
sudo pacman -S libva-nvidia-driver libva-utils
```

Verify:

```bash
vainfo
```

Expected output: entries for `VAProfileH264`, `VAProfileVP9Profile0`, `VAProfileAV1Profile0`, etc.

### 2. Update Hyprland Config

File: `~/.config/hypr/hyprland.conf`

Remove this line if present (it disables atomic modesetting):

```conf
env = AQ_DRM_NO_ATOMIC,1
```

Do **not** set `__EGL_VENDOR_LIBRARY_FILENAMES` globally — CachyOS's `/etc/profile.d/nvidia-rtd3-workaround.sh` manages this automatically for the hybrid GPU setup.

### 3. Configure Google Chrome

File: `~/.config/chrome-flags.conf`

```text
--ozone-platform=wayland
--use-vulkan=disabled
--enable-features=VaapiVideoDecoder,VaapiVideoDecodeLinuxGL,VaapiVideoEncoder,VaapiIgnoreDriverChecks,WaylandWindowDecorations
--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE
--enable-gpu-rasterization
--ignore-gpu-blocklist
```

Key flags explained:

| Flag | Reason |
|---|---|
| `--use-vulkan=disabled` | Disables Vulkan at GPU process level — Vulkan is incompatible with Wayland on NVIDIA |
| `--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE` | Disables Vulkan at the Chrome feature layer |
| `VaapiVideoDecoder` | Enables VA-API hardware video decode |
| `VaapiVideoDecodeLinuxGL` | Required since Chrome 107+ for VA-API when using OpenGL/EGL backend |
| `VaapiIgnoreDriverChecks` | Bypasses Chrome's driver version checks for NVIDIA VA-API |
| `--ignore-gpu-blocklist` | Overrides Chrome's internal GPU blocklist for NVIDIA |
| Do **not** use `--use-gl=egl` | Removed in Chrome 128+, causes GPU process crash loop |
| Do **not** use `--enable-zero-copy` | Triggers DMA-BUF `eglCreateImage` failure on NVIDIA Wayland |

### 4. Configure Helium Browser

File: `~/.config/helium-browser-flags.conf`

```text
--ozone-platform=wayland
--use-vulkan=disabled
--enable-features=VaapiVideoDecoder,VaapiVideoDecodeLinuxGL,VaapiVideoEncoder,VaapiIgnoreDriverChecks,WaylandWindowDecorations
--disable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE
--enable-gpu-rasterization
--ignore-gpu-blocklist
```

Helium reads flags from `~/.config/helium-browser-flags.conf` via its wrapper script at `/opt/helium-browser-bin/helium-wrapper`. System-wide flags can also be placed in `/etc/helium-browser-flags.conf`.

### 5. Clear Chrome GPU Crash State (if needed)

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
| Video Decode | Hardware accelerated |
| WebGL | Hardware accelerated |
| Video Encode | Software only (expected — NVIDIA VA-API encode not supported in Chrome on Linux) |

Monitor GPU decoder usage during YouTube playback:

```bash
nvidia-smi dmon -s u
```

The `dec` column should show non-zero activity while a video is playing.

## Hyprland Config Reference

Relevant section of `~/.config/hypr/hyprland.conf` for this setup:

```conf
env = LIBVA_DRIVER_NAME,nvidia
env = LIBVA_DRM_DEVICE,/dev/dri/renderD128
env = NVD_BACKEND,direct
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
```
