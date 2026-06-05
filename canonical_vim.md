# Canonical Shortcuts Reference

> **Canonical keymap:** VS Code  
> **Covered IDEs:** VS Code, PyCharm / JetBrains  
> Mac uses `Cmd`, Linux/Windows uses `Ctrl` in place of `Cmd` unless noted.

### Config files

| File | Purpose |
|------|---------|
| `.ideavimrc` | PyCharm only — all Vim remaps + IDE action mappings via IdeaVim |
| `vscode-settings.json` | VS Code only — base Vim remaps via VSCodeVim settings |
| `vscode-keybindings.json` | VS Code only — IDE action mappings (leader shortcuts, tab nav) |

> Base Vim remaps must be kept in sync manually between `.ideavimrc` and `vscode-settings.json`.
> IDE action mappings must be kept in sync between `.ideavimrc` and `vscode-keybindings.json`.

### VSCodeVim known limitations

VSCodeVim has a bug where remapping an uppercase letter (e.g. `H`, `L`, `G`, `N`) also breaks
its lowercase counterpart (`h`, `l`, `g`, `n`). There is no known workaround within VSCodeVim.

As a result, the following remaps from `.ideavimrc` are **intentionally absent** from `vscode-settings.json`:

| Remap | Works in PyCharm | Works in VS Code |
|-------|-----------------|-----------------|
| `H` → `^` (line start) | ✓ | ✗ use `^` directly |
| `L` → `$` (line end) | ✓ | ✗ use `$` directly |
| `G` → `Gzz` (center after jump) | ✓ | ✗ use `G` then `zz` |
| `j` → `gj` (visual line) | ✓ | ✗ use `gj` directly |
| `N` → `Nzz` (center after search) | ✓ | ✗ use `N` then `zz` |
| `J` → `mzJ\`z` (keep cursor on join) | ✓ | ✗ use `J` directly |

---

## Layer 1 — Vim Motions

Motions and operators are built into both plugins and need no config.
Remaps are configured separately in `.ideavimrc` (PyCharm) and `vscode-settings.json` (VS Code).
See VSCodeVim limitations above for remaps that only work in PyCharm.

### Modes
| Key | Action |
|-----|--------|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `I` | Insert at start of line |
| `A` | Insert at end of line |
| `o` | New line below, insert |
| `O` | New line above, insert |
| `v` | Visual (character) mode |
| `V` | Visual line mode |
| `Ctrl+v` | Visual block mode |
| `Esc` / `jk` | Back to Normal mode |

### Navigation — character & line
| Key | Action |
|-----|--------|
| `h j k l` | Left / down / up / right |
| `0` | Start of line (including whitespace) |
| `^` / `H` ⚠️ | First non-blank character of line (`H` only works in PyCharm) |
| `$` / `L` ⚠️ | End of line (`L` only works in PyCharm) |
| `gg` | Top of file |
| `G` | Bottom of file |
| `:{n}` | Go to line n |

### Navigation — word
| Key | Action |
|-----|--------|
| `w` | Next word start |
| `W` | Next WORD start (space-separated) |
| `b` | Previous word start |
| `e` | Next word end |
| `ge` | Previous word end |

### Navigation — screen & file
| Key | Action |
|-----|--------|
| `Ctrl+d` | Scroll half page down (centered) |
| `Ctrl+u` | Scroll half page up (centered) |
| `Ctrl+f` | Scroll full page down |
| `Ctrl+b` | Scroll full page up |
| `zz` | Center current line on screen |
| `zt` | Current line to top |
| `zb` | Current line to bottom |
| `%` | Jump to matching bracket / paren |
| `*` | Search forward for word under cursor |
| `#` | Search backward for word under cursor |

### Navigation — search
| Key | Action |
|-----|--------|
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next match (centered) |
| `N` | Previous match (centered) |
| `//` (visual) | Search for selected text |

### Operators (combine with motions)

> **Clipboard rule:** this config routes all yanks and deletes to the system clipboard.
> Every `d`, `c`, `x`, `y` operation overwrites it — unless you use `<leader>d` (black hole register).

| Key | Action | Clipboard |
|-----|--------|-----------|
| `d{motion}` | Delete | ✗ overwrites |
| `c{motion}` | Change (delete + insert) | ✗ overwrites |
| `y{motion}` | Yank (copy) | ✓ writes |
| `<leader>d{motion}` | Delete without touching clipboard | ✓ preserved |
| `>{motion}` | Indent | — |
| `<{motion}` | Dedent | — |
| `={motion}` | Auto-indent | — |
| `gU{motion}` | Uppercase | — |
| `gu{motion}` | Lowercase | — |
| `gc{motion}` | Comment (vim-commentary) | — |

### Common operator + motion combos
| Key | Action | Clipboard |
|-----|--------|-----------|
| `dd` | Delete line | ✗ overwrites |
| `<leader>dd` | Delete line without touching clipboard | ✓ preserved |
| `yy` | Yank line | ✓ writes |
| `cc` | Change line | ✗ overwrites |
| `dw` | Delete word forward | ✗ overwrites |
| `diw` | Delete inner word | ✗ overwrites |
| `ciw` | Change inner word | ✗ overwrites |
| `ci"` | Change inside double quotes | ✗ overwrites |
| `ci'` | Change inside single quotes | ✗ overwrites |
| `ci(` | Change inside parentheses | ✗ overwrites |
| `ci{` | Change inside curly braces | ✗ overwrites |
| `ca"` | Change around double quotes (includes quotes) | ✗ overwrites |
| `dit` | Delete inside HTML/XML tag | ✗ overwrites |
| `D` | Delete to end of line | ✗ overwrites |
| `C` | Change to end of line | ✗ overwrites |
| `Y` | Yank to end of line | ✓ writes |
| `x` | Delete character under cursor | ✗ overwrites |
| `J` | Join line below | — |

### Visual mode — selection & deletion
| Key | Action | Clipboard |
|-----|--------|-----------|
| `v` + motion | Select character by character | — |
| `V` + motion | Select whole lines | — |
| `Ctrl+v` + motion | Select rectangular block | — |
| `ggVG` | Select entire file | — |
| `vi"` | Select inside quotes | — |
| `va(` | Select including parentheses | — |
| `d` | Delete selection | ✗ overwrites |
| `<leader>d` | Delete selection without touching clipboard | ✓ preserved |
| `y` | Yank selection | ✓ writes |
| `ggyG` | Yank entire file (no visual mode needed) | ✓ writes |
| `p` | Paste over selection — clipboard **not** overwritten | ✓ preserved |

### Multi-cursor — same word
| Key | Action |
|-----|--------|
| `gb` | Add cursor at next occurrence of word under cursor (repeatable) |

> Press `gb` multiple times to select more occurrences, then type to replace all at once.
> This is the Vim equivalent of `Cmd+D` in VS Code.

### Multi-cursor — visual block
Visual block mode lets you place a cursor on the same column across multiple lines.

| Step | Key | Action |
|------|-----|--------|
| 1 | `Ctrl+v` | Enter visual block mode |
| 2 | `j` / `k` | Extend block down / up |
| 3a | `I` | Insert at the **start** of each line in the block |
| 3b | `A` | Insert at the **end** of each line in the block |
| 4 | type, then `Esc` | Change applies to all lines simultaneously |

> Example: add `//` at the start of 3 lines — position cursor, `Ctrl+v`, `2j`, `I`, type `//`, `Esc`.

### Clipboard & registers
| Key | Action |
|-----|--------|
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `"ayy` | Yank line into register `a` |
| `"ap` | Paste from register `a` |
| `"+y` | Yank to system clipboard explicitly |
| `"+p` | Paste from system clipboard explicitly |

### Text objects
| Key | Action |
|-----|--------|
| `iw` | Inner word |
| `aw` | A word (includes space) |
| `i"` `i'` `` i` `` | Inner quotes |
| `i(` `i)` | Inner parentheses |
| `i{` `i}` | Inner curly braces |
| `i[` `i]` | Inner square brackets |
| `it` | Inner tag |
| `ia` / `aa` | Inner / around function argument (argtextobj) |

### Surround (vim-surround)
| Key | Action |
|-----|--------|
| `cs"'` | Change surrounding `"` to `'` |
| `ds"` | Delete surrounding `"` |
| `ysiw"` | Surround word with `"` |
| `yss)` | Surround entire line with `()` |
| `S"` (visual) | Surround selection with `"` |

### Marks & jumps
| Key | Action |
|-----|--------|
| `ma` | Set mark `a` at cursor |
| `` `a `` | Jump to exact position of mark `a` |
| `'a` | Jump to line of mark `a` |
| `Ctrl+o` | Jump back (previous location) |
| `Ctrl+i` | Jump forward |

### Macros
| Key | Action |
|-----|--------|
| `qa` | Start recording macro into register `a` |
| `q` | Stop recording |
| `@a` | Play macro `a` |
| `@@` | Repeat last macro |
| `10@a` | Play macro `a` 10 times |

### Miscellaneous
| Key | Action |
|-----|--------|
| `.` | Repeat last change |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `~` | Toggle case of character |
| `r{char}` | Replace character under cursor |
| `>>` | Indent line |
| `<<` | Dedent line |
| `Alt+j/k` | Move line / selection up or down |
| `<` / `>` (visual) | Indent / dedent, stay in visual mode |

### EasyMotion
| Key | Action |
|-----|--------|
| `<leader><leader>w` | Jump to any word start on screen |
| `<leader><leader>b` | Jump to any word start (backward) |
| `<leader><leader>s` | Jump to any character on screen |

---


## Vim Leader Shortcuts (`.ideavimrc` + `vscode-keybindings.json`)

These mappings are defined in `vscode-keybindings.json` and must also be mirrored in `.ideavimrc`.
Note: `.ideavimrc` is not tracked in this repo — keep it in sync manually when adding new mappings.

### Code actions
| Key | Action |
|-----|--------|
| `<leader>gd` | Go to definition |
| `<leader>gi` | Go to implementation |
| `<leader>gr` | Find usages / references |
| `<leader>rn` | Rename symbol |
| `<leader>k` | Show hover documentation |
| `<leader>f` | Format file |
| `<leader>o` | File structure / outline |
| `<leader>e` | Recent files |
| `<leader>a` | Quick fix / intention action |
| `<leader>b` | Toggle breakpoint |
| `<leader>rr` | Run |
| `<leader>h` | Clear search highlight |
| `<leader>uc` | Transform to lowercase (VS Code only — use `gu{motion}` in PyCharm) |
| `<leader>uw` | Transform to uppercase (VS Code only — use `gU{motion}` in PyCharm) |
| `]e` / `[e` | Next / previous error |
| `]c` / `[c` | Next / previous git change hunk |
| `gt` / `gT` | Next / previous editor tab |
| `<leader><leader>w` | EasyMotion — jump to word |

### Panel toggles (`t` = toggle)
> These replace conflicting native shortcuts like `Cmd+B`.

| Key | Action |
|-----|--------|
| `<leader>te` | Toggle file explorer / sidebar |
| `<leader>tt` | Open terminal from the editor (see terminal note below) |
| `<leader>tp` | Toggle bottom panel (problems) |
| `<leader>tm` | Toggle maximize editor / hide all panels |
| `Cmd+J` (VS Code only) | Universal terminal toggle — also closes it **from inside the terminal** |

> **Terminal toggling in VS Code.** The leader is `Space`, and inside the terminal a
> space must reach the shell — so `<leader>tt` can only *open* the terminal (from the
> editor). To close it while focused in the terminal, use `Cmd+J`, which is bound to a
> non-`Space` key precisely so it works from any context. `Cmd+J` opens it too, so it
> doubles as a one-key universal toggle. In PyCharm this isn't needed: IdeaVim's
> `<leader>tt` toggles the terminal tool window in both directions.
>
> **Why every VS Code leader binding requires `editorTextFocus`.** `vim.mode` stays
> `Normal` even when focus is in the terminal, so a leader binding without a focus guard
> fires there and swallows the `Space` before the shell sees it (the "space doesn't type
> in the terminal" bug). Every entry in `vscode-keybindings.json` is therefore guarded
> with `editorTextFocus && vim.active && vim.mode == 'Normal'`. Keep that guard on any
> new `Space`-prefixed binding.

---

## VS Code: config files

Base Vim remaps live in `vscode-settings.json` via VSCodeVim's key binding arrays.
IDE action mappings live in `vscode-keybindings.json`.
Both files symlink to `~/Library/Application Support/Code/User/` — see Dotfiles Layout below.

---

## Dotfiles Layout

```
dotfiles/
  .ideavimrc                        ← symlink to ~/
  keyboards/
    canonical.md                    ← this file
    vscode-keybindings.json         ← symlink to ~/Library/Application Support/Code/User/
    vscode-settings.json            ← symlink to ~/Library/Application Support/Code/User/
    jetbrains-keymap.xml            ← symlink to ~/.config/JetBrains/<product>/keymaps/
```

### Symlink commands (macOS/Linux)
```bash
# .ideavimrc
ln -sf ~/dotfiles/.ideavimrc ~/.ideavimrc

# VS Code keybindings
ln -sf ~/dotfiles/keyboards/vscode-keybindings.json \
  "$HOME/Library/Application Support/Code/User/keybindings.json"

# VS Code settings
ln -sf ~/dotfiles/keyboards/vscode-settings.json \
  "$HOME/Library/Application Support/Code/User/settings.json"

# PyCharm (adjust product folder name as needed)
ln -sf ~/dotfiles/keyboards/jetbrains-keymap.xml \
  "$HOME/.config/JetBrains/PyCharm2024.3/keymaps/my-keymap.xml"
```