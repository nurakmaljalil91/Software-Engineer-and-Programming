---
id: Neovim as Main Integrated Development Environment (IDE)
aliases: 
tags:
  - Neovim
---

## Overview

[[Neovim]] is a keyboard-based text editor. 
## Getting started

Read how to install [[Neovim]] from [here](https://github.com/neovim/neovim/blob/master/INSTALL.md). Since we use Windows, download the `.msi` and install it. Once installed, you can open it from Windows terminal by typing `nvim` or run the program from Windows start menu.

```bash
$ nvim
```

![[neovim-first-opening.png]]

Learn some basic on [[Neovim]] by typing `:Tutor`. Master moving the cursor by pressing `h`, `j`, `k`, `l`. Use `:help` to get all the help
## Easy beginner setup

Use [kicstart.nvim](https://github.com/nvim-lua/kickstart.nvim) to easily setup [[Neovim]] configuration. If you're using `powershell.exe` clone the [kicstart.nvim](https://github.com/nvim-lua/kickstart.nvim)to the configuration path.

```bash
git clone https://github.com/nvim-lua/kickstart.nvim.git $env:USERPROFILE\AppData\Local\nvim\
```

The other way to do this is by go to [[Neovim]] configuration folder and copy the content of `init.lua`

```bash
cd $env:USERPROFILE\AppData\Local\nvim\
touch init.lua
```

Start  [[Neovim]] by command `nvim` and [kicstart.nvim](https://github.com/nvim-lua/kickstart.nvim) will install all the plugins once it started.

Learn more about [kicstart.nvim](https://github.com/nvim-lua/kickstart.nvim) by pressing `space` + `s` + `h`. Choose topic to learn.

```
<space> sh
```

To search the file use `space` + `s` + `f` and write the name of the file and click enter.

```
<space> sf
```

To split the screen `Ctrl w` + `v`. 

```
Wv
```

To go to directory use `:Ex` command.

```
:Ex
```

## Installing plugins

[[Neovim]] use [LazyVim](https://www.lazyvim.org/).Open the menu using `:Lazy` command.

```
:Lazy
```

To install the custom plugin, go to configuration file and open the `init.lua` file.

```
cd $env:USERPROFILR\AppData\Local\nvim\
nvim init.lua
```

Once inside the `init.lua` file go to section to add custom custom plugin. For example to install [Obsidian.nvim](https://github.com/epwalsh/obsidian.nvim), add the [GitHub](https://github.com/) package name to the `init.lua` file. For example, add `{ "epwalsh/obsidian.nvim" }` to the file.

```lua
require("lazy").setup({
	-- NOTE: Plugins can be added with a link (or for a github repo: 'owner/repo' link).

	-- My custom plugins
	-- Install vscode colorscheme
	"Mofiqul/vscode.nvim",
	-- Install Obsidian.nvim
	{
		"epwalsh/obsidian.nvim",
		version = "*", -- recommended, use latest release instead of latest commit
		lazy = true,
		ft = "markdown",
		-- Replace the above line with this if you only want to load obsidian.nvim for markdown files in your vault:
		-- event = {
		--   -- If you want to use the home shortcut '~' here you need to call 'vim.fn.expand'.
		--   -- E.g. "BufReadPre " .. vim.fn.expand "~" .. "/my-vault/**.md"
		--   "BufReadPre path/to/my-vault/**.md",
		--   "BufNewFile path/to/my-vault/**.md",
		-- },
		dependencies = {
			-- Required.
			"nvim-lua/plenary.nvim",

			-- see below for full list of optional dependencies 👇
		},
		opts = {
			workspaces = {
				{
					name = "personal",
					path = "~/OneDrive/Obsidian Vault",
				},
				{
					name = "work",
					path = "~/OneDrive/Software Engineer and Programming",
				},
			},

			-- see below for full list of options 👇
		},
	},
	"tpope/vim-sleuth", -- Detect tabstop and shiftwidth automatically
})
```

## Change Color Scheme

Change the color scheme to [Visual Studio Code](https://code.visualstudio.com/) theme by add `{ "Mofiqul/vscode.nvim", }` to the `init.lua` file.

```lua
require("lazy").setup({
	-- NOTE: Plugins can be added with a link (or for a github repo: 'owner/repo' link).

	-- My custom plugins
	-- Install vscode colorscheme
	"Mofiqul/vscode.nvim",
})
```

To actually use it, modify the `vim.cmd.colorscheme()` to `vscode`.

```lua
{ 
        -- You can easily change to a different colorscheme.
		-- Change the name of the colorscheme plugin below, and then
		-- change the command in the config to whatever the name of that colorscheme is.
		--
		-- If you want to see what colorschemes are already installed, you can use `:Telescope colorscheme`.
		"folke/tokyonight.nvim",
		priority = 1000, -- Make sure to load this before all the other start plugins.
		init = function()
			-- Load the colorscheme here.
			-- Like many other themes, this one has different styles, and you could load
			-- any other, such as 'tokyonight-storm', 'tokyonight-moon', or 'tokyonight-day'.
			vim.cmd.colorscheme("vscode")

			-- You can configure highlights by doing something like:
			vim.cmd.hi("Comment gui=none")
		end,
	},
```

## Installing File Explorer in Neovim

Add the following snippet to your `init.lua` file to install and configure `nvim-tree.lua` using lazy.nvim:

```lua
 -- install nvim-tree
{
	"nvim-tree/nvim-tree.lua",
	version = "*",
	lazy = false,
	dependencies = {
		"nvim-tree/nvim-web-devicons",
	},
	config = function()
		require("nvim-tree").setup({})
	end,
}
```
## Installing GitHub Copilot in Neovim

[GitHub Copilot](https://github.com/features/copilot) is an AI Tool for helping developer to code. Install [GitHub Copilot](https://github.com/features/copilot) using `zbirenbaum/copilot.lua` plugin.It is better than the officail plugin because 100% written in Lua and have integrations with CMP, which better than virtual text. Read here [[CMP versus Virtual Text in Neovim]] for more detail.

Add this code snippet in the `init.lua` file under custom plugin section.

```lua
{
  // Rest of your configuration
  {
      "zbirenbaum/copilot.lua",
      cmd = "Copilot",
      event = "InsertEnter",
      config = function()
	requre("copilot").setup({})
      end,
  },
}
```

Save the file and reload [[Neovim]].

The plugin should be installed with the default configurations. Login to [GitHub Copilot](https://github.com/features/copilot) using the `:Copilot auth` command.

```bash
:Copilot auth
```

At this point, [GitHub Copilot](https://github.com/features/copilot) running and ready to use. Use Copilot CMP for better visualization.

Install `nvim-cmp` first (already install if using `LazyVim`) and then install `copilot-cmp`.

```lua
{
      "zbirenbaum/copilot-cmp",
      config = function()
          require("copilot_cmp").setup()
      end,
},
```

Configure CMP to work with `copilot-cmp` by adding `copilot` to `cmp` configuration .

```lua
cmp.setup {
  sources = {
    -- Copilot Source
    { name = "copilot", group_index = 2 },
    -- Other Sources
    { name = "nvim_lsp", group_index = 2 },
    { name = "path", group_index = 2 },
    { name = "luasnip", group_index = 2 },
  },
}
```

Add icon to indicate suggestion from Copilot.

```lua
-- cmp.lua
cmp.setup {
  formatting = {
    format = lspkind.cmp_format({
      mode = "symbol",
      max_width = 50,
      symbol_map = { Copilot = "" }
    })
  }
}
```

It is also recommended to disable `copilot.lua` suggestions and panel modules, as they can interfere with completions properly appearing in copilot-cmp. To do so, simply place the following in the `copilot.lua` config:

```lua
require("copilot").setup({
  suggestion = { enabled = false },
  panel = { enabled = false },
})
```
## References

-  [The Only Video You Need to Get Started with Neovim by TJ DeVries](https://www.youtube.com/watch?v=m8C0Cq9Uv9o&t=703s)
- [Setting up Copiloy in Neovim with Sane Settings article by tamerlan.dev](https://tamerlan.dev/setting-up-copilot-in-neovim-with-sane-settings/)


