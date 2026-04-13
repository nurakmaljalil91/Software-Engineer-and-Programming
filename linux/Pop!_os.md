---
title: Pop!_os
category: linux
tags:
  - linux
  - pop_os
created: 2026-04-11
updated: 2026-04-13
status: active
---
## Installing PopOS 

- Choose PopOS LTS with NVIDIA
- Flash using Rufus

## Upgrade and Update

```bash
sudo apt updated
```

```bash
sudo apt upgrade
```

- Reboot your computer

## Check NVIDIA

```bash
nvidia-smi
```
## Set up SSH for GitHub

- Open terminal and run

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- Press enter to save to default location and skip the passphrase
-  Add to your SSH agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

- Add key to GitHub account
- Copy your public key to your clipboard

```bash
cat ~/.ssh/id_ed25519.pub
```

- Go to GitHub Settings -> SSH and GPG keys -> New SSH Key
- Paste your key there and save

## Installing GitHub CLI

```bash
sudo apt install gh
```

## Installing WezTerm

```bash
# 1. Add the GPG key
curl -fsSL https://apt.fury.io/wez/gpg.key | sudo gpg --yes --dearmor -o /usr/share/keyrings/wezterm-fury.gpg

# 2. Add the repository to your sources list
echo 'deb [signed-by=/usr/share/keyrings/wezterm-fury.gpg] https://apt.fury.io/wez/ * *' | sudo tee /etc/apt/sources.list.d/wezterm.list

# 3. Update and install
sudo apt update
sudo apt install wezterm
```

### Setup JetBrains Mono Nerd Font

- Download the font from [Nerd Font](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/JetBrainsMono.zip)
- Extract the files to `~/.local/share/fonts`
- Refresh your font cache `fc-cache -fv`

### Customize WezTerm (The Lua Config)

- Create file : `nano ~/.wezterm.lua`

```lua
local wezterm = require 'wezterm'
local config = {}

-- Use the config builder for better error messages
if wezterm.config_builder then
  config = wezterm.config_builder()
end

-- --- FONT & LIGATURES ---
config.font = wezterm.font('JetBrainsMono Nerd Font', { weight = 'Regular' })
config.font_size = 12.0
config.line_height = 1.1

-- --- APPEARANCE ---
config.color_scheme = 'Catppuccin Mocha' -- One of the best for dev work
config.window_background_opacity = 0.90   -- Slightly transparent for that Linux look
config.hide_tab_bar_if_only_one_tab = true

-- --- PERFORMANCE ---
config.front_end = "WebGpu" -- Fastest rendering for Pop!_OS

return config
```

### Add Oh My Posh (Folder & Icons)

- Install the binary

```bash
curl -s https://ohmyposh.dev/install.sh | bash -s
```

- Add to your shell

```bash
nano ~/.bashrc
```

- Add this line at the very bottom

```bash
export PATH=$PATH:$HOME/.local/bin

eval "$(oh-my-posh init bash --config 'https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/atomic.omp.json')"
```

- Enable nerd font

```lua
-- Set to true if you have a Nerd Font installed and selected in the terminal
vim.g.have_nerd_font = true  -- Change this from false to true
```

## Install Neovim

```bash
sudo add-apt-repository ppa:neovim-ppa/unstable
sudo apt update
sudo apt install neovim
```

### Install Kickstart.nvim

```bash
sudo apt update
```

```bash
sudo apt install -y ripgrep fd-find fzf unzip curl git xclip
```

- Install tree-sitter

```bash
cargo install tree-sitter-cli
```

- Install Build Tool (for Treesitter)

```bash
sudo apt install -y build-essential
```

```bash
# Back up existing config if necessary
mv ~/.config/nvim ~/.config/nvim.bak 2>/dev/null

# Clone Kickstart
git clone https://github.com/nvim-lua/kickstart.nvim.git ~/.config/nvim
```

- To allow copy

```bash
sudo apt update && sudo apt install -y xclip
```

```lua
vim.opt.clipboard = 'unnamedplus'
```

- Change theme to Visual Studio Code

```
nvim ~/.config/nvim/init.lua
```

```lua
{
    'Mofiqul/vscode.nvim',
    priority = 1000, -- Load this before other plugins
    config = function()
      require('vscode').setup({
        -- Optional: Customize the style (dark, light, or modern)
        style = 'dark', 
        -- Enable italic comments
        italic_comments = true,
        -- Disable background so it matches WezTerm's transparency
        transparent = false, 
      })
      -- Activate the theme
      vim.cmd.colorscheme 'vscode'
    end,
  },
```

### Install Neo-tree in Kickstart

```bash
nvim ~/.config/nvim/init.lua
```

- Search for where your plugin are defined. Add this block to your plugin list:

```lua
{
  "nvim-neo-tree/neo-tree.nvim",
  branch = "v3.x",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- Requires Nerd Fonts
    "MunifTanjim/nui.nvim",
  },
  config = function()
    require("neo-tree").setup({
      filesystem = {
        filtered_items = {
          visible = true, -- Show hidden files (dotfiles)
          hide_dotfiles = false,
          hide_gitignored = false,
        },
        follow_current_file = { enabled = true }, -- Focus the file you're currently editing
      },
      window = {
        width = 30,
        mappings = {
          ["<space>"] = "none", -- Disable space so it doesn't conflict with your leader key
        },
      },
    })
    -- Shortcut to toggle the tree
    vim.keymap.set('n', '<leader>e', ':Neotree toggle<CR>', { desc = 'Toggle [E]xplorer' })
  end,
},
```
## Install Fnm & Node

```bash
curl -fsSL https://fnm.vercel.app/install | bash
```

- Configure in Shell

```bash
nvim /.bashrc
```

- Go to bottom of the file and add these lines:

```bash
# fnm
export PATH="$HOME/.local/share/fnm:$PATH"
eval "`fnm env --use-on-cd`"
```

- Refresh Shell

```bash
source ~/.bashrc
```

### Install Node.js

```bash
# Install the LTS version
fnm install --lts

# Tell fnm to use it as the default
fnm default lts-latest
```

- Verify

```bash
node -v
npm -v
```

## C++ Essentials

- Install Core Toolchain

```bash
sudo apt update
sudo apt install build-essential gdb
```

- Install CMake and Ninja

```bash
sudo apt install cmake ninja-build
```

- Install Clang/LLVM

```bash
sudo apt install clang clangd
```


## Install Rust

- Install Dependencies

```bash
sudo apt update
sudo apt install build-essential -y
```

- Install Rust via Rustup

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

- Configure your Shell

```bash
source "$HOME/.cargo/env"
```

```
## Install Gemini CLI

```bash
# Install globally via npm
npm install -g @google/gemini-cli

# Run it to authenticate
gemini
```

## Install GitHub Copilot CLI

```bash
# 1. Install GitHub CLI (if not already installed)
sudo apt install gh

# 2. Authenticate your GitHub account
gh auth login

# 3. Install the Copilot extension
gh extension install github/gh-copilot
```

## Install OpenAI Codex CLI

```bash
# Install globally via npm
npm install -g @openai/codex

# Set your API Key
export OPENAI_API_KEY="your-key-here"
```

## Install .NET

```bash
# 1. Update your package list
sudo apt update

# 2. Install the .NET 10 SDK (Includes Runtime)
sudo apt install -y dotnet-sdk-10.0

# 3. (Optional) If you build web apps, install the ASP.NET Core runtime
sudo apt install -y aspnetcore-runtime-10.0
```

## Install Tmux

```bash
sudo apt update
sudo apt install tmux
```

## Install Docker

- The clean setup (Official Repository)

```bash
# Update and install pre-requisites
sudo apt update
sudo apt install ca-certificates curl gnupg

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine & Compose
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- The "Non-Root" fix

```bash
# Create the docker group (if it doesn't exist)
sudo groupadd docker

# Add your user to the group
sudo usermod -aG docker $USER

# Apply the group changes without logging out
newgrp docker
```

- Verification

```bash
docker run hello-world
```

## Install CopyQ

```bash
sudo apt install copyq
```

## Install Btop

```bash
sudo apt update
sudo apt install btop
```

## Install Git Delta

```bash
sudo apt install git-delta
```
- run this

```bash
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global delta.dark true  # or `delta.light true`, or omit for auto-detection
git config --global merge.conflictStyle zdiff3
```
- For side by side view

```toml
[delta]
    side-by-side = true
```
- Full configuration

```toml
[delta]
    features = side-by-side line-numbers decorations
    syntax-theme = Monokai Extended Bright
    plus-style = syntax "#003800"
    minus-style = syntax "#3f0001"

[delta "decorations"]
    commit-decoration-style = bold yellow box ul
    file-style = bold yellow ul
    file-decoration-style = none
```

## Install LazyGit

```bash
# For Lazygit (using the official personal package archive)
sudo add-apt-repository ppa:lazygit-team/release
sudo apt update
sudo apt install lazygit
```

```bash
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit /usr/local/bin
```
- verify

```bash
lazygit --version
```
- cleanup

```bash
rm lazygit.tar.gz lazygit
```
- remove the broken PPA

```bash
sudo add-apt-repository --remove ppa:lazygit-team/release
sudo apt update
```
- Integration with Lazygit

```yaml
git:
  paging:
    colorArg: always
    pager: delta --dark --paging=never
```

## Software Installed

- Chrome
- [[Obsidian]]
- GitHub Desktop
- Steam Installer
- VLC
- Visual Studio Code
