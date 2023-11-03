# Neovim DevOps IDE configuration

STOPPED.

I changed to [LazyVim](https://www.lazyvim.org).

"Effortless Efficiency, Unleash IDE Power with Ease".

---

The purpose of this Neovim configuration is to make an IDE for DevOps engineers. It is forked from [kickstart.nvim](nvim-lua/kickstart.nvim).

### Introduction

#### Requirements for Neovim DevOps IDE

| Subject | Description |
| :------ | :---------- |
| Goal | Stay ascetic, the fewer plugins, the more core Neovim functionality only, the better. Compatibility, stability, speed, simplicity, maintainability above all. |
| Support | Bash, Docker (Dockerfile), Git, Jenkins (Jenkinsfile, Pipeline Groovy), JSON, Kubernetes, Lua, Markdown, Python, TOML, XML, YAML, CSV, etc. |
| Configuration | Have a Neovim-compatible Lua configuration. |
| Plugins | If there is an alternative Lua plugin, use it, if not, use the vimscript plugin. Go for the better one, not the all-cost Lua one. |
| LSP | Have linters, formatters and intellisense auto completion. |

Devops.nvim targets only the latest ['stable'](https://github.com/neovim/neovim/releases/tag/stable) and latest ['nightly'](https://github.com/neovim/neovim/releases/tag/nightly) of Neovim. If you are experiencing issues, please make sure you have the latest versions.

### Installation

> **NOTE** 
> [Backup](#FAQ) your previous configuration (if any exists)

```bash
mv ~/.config/nvim{,.bak}
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}
```

or simply remove them, if you already have a backup:

```bash
rm -fr ~/.config/nvim; rm -fr ~/.local/share/nvim; rm -fr ~/.local/state/nvim; rm -fr ~/.cache/nvim
```

Requirements:
* Make sure to review the readmes of the plugins if you are experiencing errors. In particular:
  * [ripgrep](https://github.com/BurntSushi/ripgrep#installation) is required for multiple [telescope](https://github.com/nvim-telescope/telescope.nvim#suggested-dependencies) pickers.
* See [Windows Installation](#Windows-Installation) if you have trouble with `telescope-fzf-native`

Neovim's configurations are located under the following paths, depending on your OS:

| OS | PATH |
| :- | :--- |
| Linux | `$XDG_CONFIG_HOME/nvim`, `~/.config/nvim` |
| MacOS | `$XDG_CONFIG_HOME/nvim`, `~/.config/nvim` |
| Windows | `%userprofile%\AppData\Local\nvim\` |

Clone devops.nvim:

```sh
# on Linux and Mac
git clone https://github.com/Imolai/devops.nvim.git "${XDG_CONFIG_HOME:-$HOME/.config}"/nvim
```

```
# on Windows
git clone https://github.com/Imolai/devops.nvim.git %userprofile%\AppData\Local\nvim\ 
```

### Post Installation

Start Neovim:

```sh
nvim
```

The `Lazy` plugin manager will start automatically on the first run and install the configured plugins. After the installation is complete you can press `q` to close the `Lazy` UI and **you are ready to go**! Next time you run nvim `Lazy` will no longer show up.

If you would prefer to hide this step and run the plugin sync from the command line, you can use:

```sh
nvim --headless "+Lazy! sync" +qa
```

### Uninstallation

To uninstall `devops.nvim`, you need to remove the following files and directories:

- config: `~/.config/nvim`
- data: `~/.local/share/nvim`
- state: `~/.local/state/nvim`
- cache: `~/.cache/nvim`

### Recommended Steps

[FORK](https://docs.github.com/en/get-started/quickstart/fork-a-repo) this repo (so that you have your own copy that you can modify) and then installing you can install to your machine using the methods above.

> **NOTE**  
> Your fork's url will be something like this: `https://github.com/<your_github_username>/devops.nvim.git`

### Configuration And Extension

* Inside of your copy, feel free to modify any file you like! It's your copy!
* Feel free to change any of the default options in `init.lua` to better suit your needs.
* For adding plugins, there are 3 primary options:
  * Add new configuration in `lua/custom/plugins/*` files, which will be auto sourced using `lazy.nvim` (uncomment the line importing the `custom/plugins` directory in the `init.lua` file to enable this)
  * Modify `init.lua` with additional plugins.
  * Include the `lua/devops/plugins/*` files in your configuration.

You can also merge updates/changes from the repo back into your fork, to keep up-to-date with any changes for the default configuration.

#### Example: Adding an autopairs plugin

In the file: `lua/custom/plugins/autopairs.lua`, add:

```lua
-- File: lua/custom/plugins/autopairs.lua

return {
  "windwp/nvim-autopairs",
  -- Optional dependency
  dependencies = { 'hrsh7th/nvim-cmp' },
  config = function()
    require("nvim-autopairs").setup {}
    -- If you want to automatically add `(` after selecting a function or method
    local cmp_autopairs = require('nvim-autopairs.completion.cmp')
    local cmp = require('cmp')
    cmp.event:on(
      'confirm_done',
      cmp_autopairs.on_confirm_done()
    )
  end,
}
```

This will automatically install [windwp/nvim-autopairs](https://github.com/windwp/nvim-autopairs) and enable it on startup. For more information, see documentation for [lazy.nvim](https://github.com/folke/lazy.nvim).

#### Example: Adding a file tree plugin

In the file: `lua/custom/plugins/filetree.lua`, add:

```lua
-- Unless you are still migrating, remove the deprecated commands from v1.x
vim.cmd([[ let g:neo_tree_remove_legacy_commands = 1 ]])

return {
  "nvim-neo-tree/neo-tree.nvim",
  version = "*",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- not strictly required, but recommended
    "MunifTanjim/nui.nvim",
  },
  config = function ()
    require('neo-tree').setup {}
  end,
}
```

This will install the tree plugin and add the command `:Neotree` for you. You can explore the documentation at [neo-tree.nvim](https://github.com/nvim-neo-tree/neo-tree.nvim) for more information.

### Contribution

Pull-requests are welcome.
Don't forget the goal: stay ascetic, the fewer plugins, the more core Neovim functionality only, the better. Compatibility, stability, speed, simplicity, maintainability above all.
Each PR, especially those which increase the line count, should have a description as to why the PR is really necessary.

### FAQ

* What should I do if I already have a pre-existing neovim configuration?
  * You should back it up, then delete all files associated with it.
  * This includes your existing init.lua and the neovim files in `~/.local` which can be deleted with `rm -rf ~/.local/share/nvim/`
  * You may also want to look at the [migration guide for lazy.nvim](https://github.com/folke/lazy.nvim#-migration-guide)
* Can I keep my existing configuration in parallel to devops?
  * Yes! You can use [NVIM_APPNAME](https://neovim.io/doc/user/starting.html#%24NVIM_APPNAME)`=nvim-NAME` to maintain multiple configurations. For example you can install the devops configuration in `~/.config/nvim-devops` and create a script `~/bin/nvim-devops`:

    ```bash
    #!/bin/bash
    exec env NVIM_APPNAME=nvim-devops nvim "$@"
    ```

    When you run Neovim with `nvim-devops` it will use the alternative config directory and the matching local directory: `~/.local/share/nvim-devops`. You can apply this approach to any Neovim distribution that you would like to try out.

### Windows Installation

Installation may require installing build tools, and updating the run command for `telescope-fzf-native`

See `telescope-fzf-native` documentation for [more details](https://github.com/nvim-telescope/telescope-fzf-native.nvim#installation)

This requires:

- Install CMake, and the Microsoft C++ Build Tools on Windows

```lua
{'nvim-telescope/telescope-fzf-native.nvim', build = 'cmake -S. -Bbuild -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release && cmake --install build --prefix build' }
```
