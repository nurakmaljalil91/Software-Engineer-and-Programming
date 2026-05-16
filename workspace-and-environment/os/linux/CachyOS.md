---
title: CachyOS
category: linux
tags:
  - linux
  - cachyos
  - arch
  - hyprland
created: 2026-05-14
updated: 2026-05-16
status: active
---

## Overview

CachyOS is an Arch-based Linux distribution optimized for performance using the BORE scheduler and PGO/LTO-optimized packages. This note covers installation and post-install setup on the **HP ZBook Power G9** with GRUB as the bootloader and Hyprland as the desktop.

Related: [[Linux]], [[Pop!_os]], [[Configure Hyprland]], [[Pacman Basic Usage]]

## Installing CachyOS

### 1. Create Bootable USB

- Download the latest ISO from [cachyos.org](https://cachyos.org)
- Flash using **Ventoy** or **dd**:

```bash
sudo dd if=cachyos-*.iso of=/dev/sdX bs=4M status=progress oflag=sync
```

### 2. Boot and Install

- Boot from USB → select **CachyOS Hello** installer
- Choose **Online Install** (recommended, pulls latest packages)
- Partitioning: use **Erase disk** or manual partition with:
  - `/boot/efi` — 512 MB, FAT32, EFI System Partition
  - `/` — remaining space, ext4 or btrfs (btrfs recommended)
  - swap — 8–16 GB (or use zram post-install)

### 3. Bootloader — GRUB

- In the installer, select **GRUB** as the bootloader
- After install, verify GRUB is installed:

```bash
grub-install --version
```

- Edit GRUB config if needed:

```bash
sudo nvim /etc/default/grub
```

- Regenerate GRUB config after changes:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 4. Select Hyprland Desktop

- In the CachyOS installer, select **Hyprland** from the desktop environment list
- CachyOS ships a pre-configured Hyprland setup with Waybar, wofi, and swaync

## System Information

```bash
fastfetch
```

```bash
hostnamectl
```

## Update System

```bash
sudo pacman -Syu
```

## AUR Helper — paru

CachyOS ships with `paru` pre-installed. To verify or install:

```bash
paru --version
```

Alternative you can install `yay`

```bash
# If not present
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay && makepkg -si
```

## Hyprland Configuration

Config lives at `~/.config/hypr/hyprland.conf`. For a detailed breakdown of the configuration settings, see [[Configure Hyprland]].

```bash
nvim ~/.config/hypr/hyprland.conf
```

### Key Bindings (defaults)

| Keybind | Action |
|---|---|
| `SUPER + Q` | Open terminal |
| `SUPER + C` | Close window |
| `SUPER + M` | Exit Hyprland |
| `SUPER + E` | File manager |
| `SUPER + V` | Toggle floating |
| `SUPER + R` | App launcher (wofi) |
| `SUPER + 1-9` | Switch workspace |
| `SUPER + SHIFT + 1-9` | Move window to workspace |

### Monitor Setup

```bash
# In hyprland.conf
monitor=,preferred,auto,1
```

```bash
# Example for explicit resolution
monitor=eDP-1,2560x1600@165,0x0,1.5
```

## Install Rofi

```bash
sudo pacman -S rofi-wayland
```

## Install Wezterm (main terminal)

```bash
paru -S wezterm-git
```

## Configure Hyprland to  use Wezterm and Rofi

```bash
$mainMod = SUPER
$terminal = kitty
$fileManager = dolphin
$menu = rofi -show drun
```

## Install Zsh Shell

```bash
sudo pacman -S zsh
```

## Install GitHub CLI

```bash
sudo pacman -S github-cli
gh auth login
```

## Install Neovim

```bash
sudo pacman -S neovim
```

### Install Kickstart.nvim

```bash
sudo pacman -S ripgrep fd fzf unzip curl git
```

```bash
cargo install tree-sitter-cli
sudo pacman -S base-devel
```

```bash
# Back up existing config if necessary
mv ~/.config/nvim ~/.config/nvim.bak 2>/dev/null

# Clone Kickstart
git clone https://github.com/nvim-lua/kickstart.nvim.git ~/.config/nvim
```

- Enable clipboard (Wayland):

```bash
sudo pacman -S wl-clipboard
```

```lua
vim.opt.clipboard = 'unnamedplus'
```

## Install JetBrains Mono Nerd Font

```bash
sudo pacman -S ttf-jetbrains-mono-nerd
```

## Install Waybar

```bash
sudo pacman -S waybar ttf-font-awesome
```

## Install Grim for Screenshot

```bash
sudo pacman -S grim
```

## Install Swaybg

```bash
sudo pacman -S swaybg
```

## Install Helium Browser

```bash
sudo pacman -S helium-browser-bin
```

### Configure Wayland for Helium

Create the configuration file to enable native Wayland support:

```bash
nvim ~/.config/helium-browser-flags.conf
```

Add the following flags:

```text
--enable-features=UseOzonePlatform
--ozone-platform=wayland
```

## Install Fish Shell

```bash
sudo pacman -S fish
```

- Set Fish as default shell:

```bash
chsh -s $(which fish)
```

- Restart terminal and configure:

```bash
nvim ~/.config/fish/config.fish
```

## Set up SSH for GitHub

```bash
ssh-keygen -t ed25519 -C "nurakmaljalil91@gmail.com"
eval (ssh-agent -c)
ssh-add ~/.ssh/id_ed25519
```

- Copy public key to clipboard:

```bash
cat ~/.ssh/id_ed25519.pub | wl-copy
```

- Go to **GitHub Settings → SSH and GPG keys → New SSH Key**, paste and save.


## Install Ghostty

```bash
sudo pacman -S ghostty
```

### Setup JetBrains Mono Nerd Font

```bash
sudo pacman -S ttf-jetbrains-mono-nerd
fc-cache -fv
```

### Add Oh My Posh

```bash
yay -S oh-my-posh-bin
```

- Add to `~/.config/fish/config.fish`:

```fish
oh-my-posh init fish --config 'https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/atomic.omp.json' | source
```

## Install Zellij

```bash
sudo pacman -S zellij
```

## Install Fnm & Node

```bash
curl -fsSL https://fnm.vercel.app/install | bash
```

- Add to `~/.config/fish/config.fish`:

```fish
fnm env --use-on-cd | source
```

### Install Node.js

```bash
fnm install --lts
fnm default lts-latest
node -v && npm -v
```


## Install Tmux

```bash
sudo pacman -Syu tmux
```
## Install Rust

- Remove the existing [[Rust]]

```bash
sudo pacman -Rnd rust
```

- Check if [[Rust]] cargo still exists

```bash
which cargo
```

- Install [[Rust]]

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- Refresh `zhs` environment

```bash
source $HOME/.cargo/env
```

- Add to `~/.config/fish/config.fish`:

```fish
source "$HOME/.cargo/env.fish"
```

## C++ Essentials

```bash
sudo pacman -S base-devel gdb cmake ninja clang
```

## Install .NET

```bash
sudo pacman -S dotnet-sdk aspnet-runtime
```

- Verify:

```bash
dotnet --version
```

## Install Docker

```bash
# install docker and docker-compose
sudo pacman -S docker docker-compose
# Start the docker
sudo systemctl start docker.service
# To enable the boot
sudo systemctl enable docker.service
# create a docker group
sudo groupadd docker

sudo usermod -aG docker $USER
newgrp docker
```

- Verify:

```bash
docker run hello-world
```

## Install Git Delta

```bash
sudo pacman -S git-delta
```

```bash
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global delta.dark true
git config --global merge.conflictStyle zdiff3
```

## Install LazyGit

```bash
sudo pacman -S lazygit
```

## Install Lazydocker

```bash
yay -S lazydocker-bin
```

## Install Btop

```bash
sudo pacman -S btop
```

## Install Fastfetch

```bash
sudo pacman -S fastfetch
```

## Install Yazi

```bash
sudo pacman -S yazi ffmpeg p7zip jq poppler fd ripgrep fzf zoxide imagemagick
```

## Install Atuin

```bash
sudo pacman -S atuin
```

- Add to `~/.config/fish/config.fish`:

```fish
atuin init fish | source
```

## Install Stow

```bash
sudo pacman -S stow
stow --version
```

## Install Visual Studio Code

```bash
yay -S visual-studio-code-bin
```

## Install Google Chrome

```bash
yay -S google-chrome
```

### Configure Wayland for Google Chrome

Create the configuration file to enable native Wayland support:

```bash
nvim ~/.config/chrome-flags.conf
```

Add the following flags:

```text
--ozone-platform-hint=auto
--enable-features=WaylandWindowDecorations
```


## Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

## Install Gemini CLI

```bash
npm install -g @google/gemini-cli
gemini
```

## Install GitHub Copilot CLI

```bash
gh extension install github/gh-copilot
```

## Install OpenAI Codex CLI

```bash
npm install -g @openai/codex
export OPENAI_API_KEY="your-key-here"
```

## Install Multipass

```bash
sudo snap install multipass
```

## NVIDIA Setup (if using dedicated GPU)

```bash
# CachyOS ships NVIDIA drivers in the installer — verify they are active
nvidia-smi
```

```bash
# If reinstalling or switching
sudo pacman -S nvidia-dkms nvidia-utils lib32-nvidia-utils
```

- For Hyprland, add to `~/.config/hypr/hyprland.conf`:

```conf
env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = WLR_NO_HARDWARE_CURSORS,1
```

## Zram (Swap Alternative)

CachyOS enables zram by default. To check:

```bash
zramctl
```

## Check System Info

```bash
# Full hardware info
fastfetch

# Kernel version
uname -r

# OS details
hostnamectl
```

## Give Permission to Development Directory

```bash
sudo chown -R $USER:$USER /home/amal/Developments
```

## Software Installed

- [[Obsidian]]
- [[GitHub]] Desktop — `yay -S github-desktop`
- Steam — `sudo pacman -S steam`
- VLC — `sudo pacman -S vlc`
- [[JetBrains]] Toolbox — `yay -S jetbrains-toolbox`
  - WebStorm
  - Rider
  - RustRover
  - CLion
