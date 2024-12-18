# Editor Support

## Tree-sitter Highlighting

If your editor supports Tree-sitter, you can enable syntax highlighting for `.noti` files by adding our Tree-sitter grammar:

[**Tree-sitter Noti**](https://github.com/noti-rs/tree-sitter-noti)

## Neovim Plugin

For Neovim users, we offer a [plugin](https://github.com/noti-rs/noti.nvim) that integrates Tree-sitter to enable syntax highlighting with minimal effort.

To install, use your favorite plugin manager, such as `lazy.nvim`:

```lua
{
    "noti-rs/noti.nvim",
    dependencies = {
      "nvim-treesitter/nvim-treesitter",
    },
    build = "python3 clone_queries.py",
    opts = {},
}
```

After adding the plugin to your configuration, run:

```vim
:TSInstall noti
```
