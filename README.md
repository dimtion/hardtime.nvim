<!-- panvimdoc-ignore-start -->

<h1 align="center">
hardtime.nvim
</h1>

<div align="center">
  <div>Establish good command workflow and quit bad habit.</div><br />
  <img src="https://github.com/m4xshen/hardtime.nvim/assets/74842863/117a8d30-64ba-4ca9-8414-5c493cbe8a70" width="700" />
</div>

<!-- panvimdoc-ignore-end -->

## ✨ Features

- Block repeated keys within a period of time
- Print hints about better commands eg: Use `ci"` instead of `di"i`
- Customizable options for restricted keys, disabled keys, etc.
- Get report for your most common bad habits for improvement

Recommended workflow:

1. Avoid using the mouse and arrow keys if they are not at the home row of your keyboard.
2. Use relative jump (eg: `5j` `12-`) for vertical movement within the screen.
3. Use `CTRL-U` `CTRL-D` `CTRL-B` `CTRL-F` `gg` `G` for vertical movement outside the screen.
4. Use word-motion (`w` `W` `b` `B` `e` `E` `ge` `gE`) for short-distance horizontal movement.
5. Use `f` `F` `t` `T` `,` `;` `0` `^` `$` for medium to long-distance horizontal movement.
6. Use operator + motion/text-object (eg: `ci{` `y5j` `dap`) whenever possible.
7. Use `%` and square bracket commands (see `:h [`) to jump between brackets.

Learn more in this [blog post](https://m4xshen.dev/posts/vim-command-workflow/)

## ⚡ Requirements

- Neovim >= [v0.10.0](https://github.com/neovim/neovim/releases/tag/v0.10.0)

## 📦 Installation

1. Install via your favorite package manager.

```lua
-- lazy.nvim
{
   "m4xshen/hardtime.nvim",
   dependencies = { "MunifTanjim/nui.nvim" },
   opts = {}
},
```

2. Setup the plugin in your `init.lua`. This step is not needed with lazy.nvim if `opts` is set as above.

```lua
require("hardtime").setup()
```

If you want to see the hint messages in insert and visual mode, set the `'showmode'` to false.

But if you want to see both the hint message and current mode you can setup with one of the following methods:

- Display the mode on status line and set `'showmode'` to false. You can do this with some statusline plugin such as lualine.nvim.
- Set the `'cmdheight'` to 2 so that the hint message won't be replaced by mode message.
- Use nvim-notify to display hint messages on the right top corner instead of commandline.

## 🚀 Usage

hardtime.nvim is enabled by default. You can change its state with the following commands:

- `:Hardtime enable` enable hardtime.nvim
- `:Hardtime disable` disable hardtime.nvim
- `:Hardtime toggle` toggle hardtime.nvim

You can view the most frequently seen hints with `:Hardtime report`.

Your log file is at `~/.local/state/nvim/hardtime.nvim.log`.

## 🔧 Configuration

You can pass your config table into the `setup()` function or `opts` if you use lazy.nvim.

If the option is a table (`key = value` pair), your `value` will overwrite the default if the `key` exists, and the pair will be appended to the default configuration if the `key` doesn't exist.

Examples:

```lua
-- Remove <Up> keys and append <Space> to the disabled_keys
disabled_keys = {
   ["<Up>"] = {},
   ["<Space>"] = { "n", "x" },
},
```

```lua
-- Enable hardtime in "lazy" and "dapui" filetypes
disabled_filetypes = { 
   lazy = false,
   ["dapui*"] = false,
},
```

### Options

| Option Name              | Type                         | Default Value                                                                           | Meaning                                                                                                                                                                       |
| ------------------------ | ---------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `max_time`               | number                       | `1000`                                                                                   | Maximum time (in milliseconds) to consider key presses as repeated.                                                                                                           |
| `max_count`              | number                       | `3`                                                                                      | Maximum count of repeated key presses allowed within the `max_time` period.                                                                                                   |
| `disable_mouse`          | boolean                      | `true`                                                                                   | Disable mouse support.                                                                                                                                                        |
| `hint`                   | boolean                      | `true`                                                                                   | Enable hint messages for better commands.                                                                                                                                     |
| `notification`           | boolean                      | `true`                                                                                   | Enable notification messages for restricted and disabled keys.                                                                                                                |
| `timeout`                | number or boolean            | `3000`                                                                                   | Time to show notification in milliseconds, set to `false` to disable timeout.                                                                                                 |
| `allow_different_key`    | boolean                      | `true`                                                                                   | Allow different keys to reset the count.                                                                                                                                      |
| `enabled`                | boolean                      | `true`                                                                                   | Whether the plugin is enabled by default or not.                                                                                                                              |
| `resetting_keys`         | table of strings/table pair  | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | Keys in what modes that reset the count.                                                                                                                                      |
| `restricted_keys`        | table of strings/table pair  | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | Keys in what modes triggering the count mechanism.                                                                                                                            |
| `restriction_mode`       | string (`"block" or "hint"`) | `"block"`                                                                                | The behavior when `restricted_keys` trigger count mechanism.                                                                                                                  |
| `disabled_keys`          | table of strings/table pair  | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | Keys in what modes are disabled.                                                                                                                                              |
| `disabled_filetypes`     | table of strings/boolean pair  | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | `hardtime.nvim` is disabled under these filetypes.                                                                                                                            |
| `hints`                  | table                        | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | `key` is a string pattern you want to match, `value` is a table of hint message and pattern length. Learn more about [Lua string pattern](https://www.lua.org/pil/20.2.html). |
| `callback`               | function(text)               | `vim.notify`                                                                             | `callback` function can be used to override the default notification behavior.                                                                                                |
| `force_exit_insert_mode` | boolean                      | `false`                                                                                  | Enable forcing exit Insert mode if user is inactive in Insert mode.                                                                                                           |
| `max_insert_idle_ms`     | number                       | `5000`                                                                                   | Maximum amount of idle time, in milliseconds, allowed in Insert mode.                                                                                                         |
| `ui`                     | table of strings/table pair  | [See Config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua) | An option to customize the popup for the `Hardtime report`.

### `hints` example

These are two default hints:

```lua
hints = {
   ["k%^"] = {
      message = function()
         return "Use - instead of k^" -- return the hint message you want to display
      end,
      length = 2, -- the length of actual key strokes that matches this pattern
   },
   ["d[tTfF].i"] = { -- this matches d + {t/T/f/F} + {any character} + i
      message = function(keys) -- keys is a string of key strokes that matches the pattern
         return "Use " .. "c" .. keys:sub(2, 3) .. " instead of " .. keys
         -- example: Use ct( instead of dt(i
      end,
      length = 4,
   },
}
```

Check out some examples of custom hint in [discussion](https://github.com/m4xshen/hardtime.nvim/discussions/categories/custom-hints)!

### [Default config](https://github.com/m4xshen/hardtime.nvim/blob/main/lua/hardtime/config.lua)

<!-- panvimdoc-ignore-start -->

## 🦾 Contributing

Please read [CONTRIBUTING.md](https://github.com/m4xshen/hardtime.nvim/blob/main/CONTRIBUTING.md).

## 👥 Contributors

<a href="https://github.com/m4xshen/hardtime.nvim/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=m4xshen/hardtime.nvim" />
</a>

<!-- panvimdoc-ignore-end -->
