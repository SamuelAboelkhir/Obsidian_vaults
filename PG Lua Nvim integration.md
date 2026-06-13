---
tags:
- Lua
- Programming-Language
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Lua index|Back to index]]
# Understanding how Lua works with NVIM
## Runtime path
- nvim knows to look for lua plugins in any folder called `lua/` that's in a folder which is in its runtime path, which can be checked inside nvim with `echo nvim_list_runtime_paths()`
- So any new plugin that gets installed by lazy.nvim, or that's created locally, must be placed into one of the available runtime paths (or a new path can be appended to the list too) in order for nvim to be able to find and load it using `require`
## Treesitter
- Hover any element and type `:Inspect` to see what treesitter element it maps to
- `:InspectTree` shows every treesitter element in a file
## LSP
- `:lspconfig-all` shows all the possible LSPs and their configs
- To check if nvim can find a specific LSP server `:echo executable("server")`
- 