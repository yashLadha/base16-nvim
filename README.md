# base16-nvim

Personal fork of [wincent/base16-nvim](https://github.com/wincent/base16-nvim), kept so the Neovim colorschemes can be customised independently of upstream. The schemes are generated from the [tinted-theming](https://github.com/tinted-theming/schemes) base16 and base24 scheme collections.

Originally, this was a Lua port of [chriskempson/base16-vim](https://github.com/chriskempson/base16-vim) for [Neovim](https://github.com/neovim/neovim). As base16-vim now appears to be abandonware, the new upstream is considered to be [tinted-vim](https://github.com/tinted-theming/tinted-vim) (itself, a fork of base16-vim).

## Installation

With [lazy.nvim](https://github.com/folke/lazy.nvim):

```lua
{ "yashLadha/base16-nvim", lazy = false, priority = 1000 }
```

Then pick a scheme:

```lua
vim.cmd.colorscheme("default-dark")
```

Every file in [`colors/`](colors) is a standalone colorscheme, so there is no `setup()` to call. Scheme names are the tinted-theming slugs without a `base16-` or `base24-` prefix, for example `default-dark`, `tokyo-night-dark`, `kanagawa-dragon` or `gruvbox-dark-hard`. Use `:colorscheme <Tab>` to list them. Schemes do not set `background`, so set it yourself before loading a light scheme.

## Customising

All highlight groups come from [`templates/default.mustache`](templates/default.mustache), and every file in `colors/` is generated from it. Changes that should apply to every scheme belong in the template, followed by regenerating `colors/`. A one-off tweak to a single scheme can be made directly in its `colors/<scheme>.lua`, but it is overwritten the next time the schemes are regenerated.

[`templates/config.yaml`](templates/config.yaml) renders the template for both the base16 and base24 systems. base16 schemes fill the base24 slots (`base10` to `base17`) from their closest base16 colors, so every scheme exposes the same palette.

### Regenerating

Install [tinted-builder-rust](https://github.com/tinted-theming/tinted-builder-rust):

```sh
cargo install --locked tinted-builder-rust
```

Then run:

```sh
bin/update-themes
```

The script pulls the latest [tinted-theming schemes](https://github.com/tinted-theming/schemes), renders the template, and replaces `colors/` with the result. When a scheme exists in both systems under the same name, the base24 version is kept. Schemes removed upstream are removed from `colors/` as well.

## Why?

These schemes are implemented in pure Lua, using the lowest-level API for creating highlight groups ([`vim.api.nvim_set_hl`](https://neovim.io/doc/user/api.html#nvim_set_hl())), so as to have the minimum possible impact on startup time.

## base16 or base24?

Originally, this was a [base16](https://github.com/tinted-theming/home/blob/main/styling.md) theme, but is now a [base24](https://github.com/tinted-theming/base24/blob/main/styling.md) theme (ie. with seven additional colors corresponding to the ANSI "bright" colors). For now, there are no plans to change the name.
