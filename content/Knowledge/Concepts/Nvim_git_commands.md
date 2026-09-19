Done! Your Neovim now has full Git integration. Here's what's available:

| Plugin | Keybinding | Description |
|--------|------------|-------------|
| vim-fugitive | `<leader>gg` | Git status (full Fugitive interface) |
| | `<leader>gb` | Git blame |
| | `<leader>gd` | Diff split |
| | `<leader>gc` | Git commit |
| | `<leader>gp` | Git push |
| | `<leader>gP` | Git pull |
| lazygit.nvim | `<leader>lg` | Full TUI Git interface (like terminal lazygit) |
| diffview.nvim | `<leader>gv` | Open diff view |
| | `<leader>gh` | File history |
| gitsigns.nvim | `]h` / `[h` | Next/prev hunk |
| | `<leader>hs` | Stage hunk |
| | `<leader>hr` | Reset hunk |
| | `<leader>hp` | Preview hunk |
| | `<leader>hb` | Blame line |
| advanced-git-search | `<leader>gs` | Telescope-based git search |

**Usage:** Open any git repo in nvim (`nvim .` or `nvim /path/to/repo`) and use the keys above.

**Pro tip:** `<leader>lg` opens LazyGit inside Neovim - you get the full terminal UI without leaving your editor.