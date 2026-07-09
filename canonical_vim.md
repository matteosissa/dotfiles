# Canonical Shortcuts Reference

> **Canonical keymap:** VS Code
> **Covered IDEs:** VS Code, PyCharm / JetBrains

The keymap has **two layers**, and every shortcut belongs to exactly one:

- **Layer 1 — Editor (Vim motions + leader code actions).** Text editing,
  code navigation, code intelligence. Fires **only when a text editor is
  focused**. Uses Vim motions and the `Space` leader.
- **Layer 2 — IDE management (`Ctrl+Shift`).** Panels, terminal, file/project
  search. Fires from **any focus** (editor, terminal, project tree). Uses
  `Ctrl+Shift+<letter>`, kept as literal `Ctrl` on every OS.

### Config files

| File | Layer | Purpose |
|------|-------|---------|
| `.ideavimrc` | 1 | JetBrains — base Vim remaps + leader code actions (IdeaVim) |
| `vscode-settings.json` | 1 | VS Code — base Vim remaps (VSCodeVim) |
| `vscode-keybindings.json` | 1 + 2 | VS Code — §1 leader code actions, §2 `Ctrl+Shift` IDE management |
| `jetbrains-keymap.xml` | 2 | JetBrains — native `Ctrl+Shift` IDE-management bindings |

> **Sync rules**
> - Base Vim remaps: keep `.ideavimrc` (§2) and `vscode-settings.json` in sync.
> - Leader code actions: keep `.ideavimrc` (§4) and `vscode-keybindings.json` §1 in sync.
> - IDE management: keep `jetbrains-keymap.xml` and `vscode-keybindings.json` §2 in sync.
>
> All four files are tracked in this repo (`.ideavimrc` is committed as `ideavimrc`
> and symlinked to `~/.ideavimrc`).

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

# Layer 1 — Editor (Vim motions + text editing)

Motions and operators are built into both plugins and need no config.
Remaps are configured in `.ideavimrc` (PyCharm) and `vscode-settings.json` (VS Code).
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
| `%` | Jump to matching bracket / parent |
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
| `daw` | Delete a word |  ✗ overwrites |
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

> Notice: prepending `<leader>` to the delete commands will avoid overwriting to the system clipboard.

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

## Layer 1 — Editor code actions (leader)

Native IDE actions, **editor focus only**. Defined in `vscode-keybindings.json` §1
and mirrored in `.ideavimrc` §4. The leader is `Space`.

> Every VS Code leader binding is guarded with
> `editorTextFocus && vim.active && vim.mode == 'Normal'`. Without the guard the
> binding fires outside the editor and swallows the `Space` before the focused
> widget (terminal, tree) sees it. Keep that guard on any new `Space` binding.

### Code navigation & intelligence
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
| `<leader>uc` | Transform to lowercase (VS Code only — use `gu{motion}` in PyCharm) |
| `<leader>uw` | Transform to uppercase (VS Code only — use `gU{motion}` in PyCharm) |

### Diagnostics, VCS, run
| Key | Action |
|-----|--------|
| `<leader>en` / `<leader>ep` | Next / previous error |
| `<leader>cn` / `<leader>cp` | Next / previous git change hunk |
| `<leader>b` | Toggle breakpoint |
| `<leader>rr` | Run |
| `<leader>h` | Clear search highlight |
| `gt` / `gT` | Next / previous editor tab |
| `<leader><leader>w` | EasyMotion — jump to word |

> **Moved out:** *Go to file* and *Find in project* used to live here as
> `<leader>gf` / `<leader>sp`. They are file/project search, so they now belong
> to Layer 2 (`Ctrl+Shift+O` / `Ctrl+Shift+F`) and work from any focus.

> **Italian keyboard layout:** `]`/`[` bindings are unavailable in VSCodeVim on
> Italian keyboards (AltGr chords are not recognized). All bracket-based bindings
> are remapped to leader sequences. IdeaVim is unaffected.

---

# Layer 2 — IDE Management (global, `Ctrl+Shift`)

Panels, terminal, and file/project search. One chord family, identical in both
IDEs and on every OS.

> **Why `Ctrl+Shift+<letter>`**
> - **No Vim conflict.** Vim's motions are plain `Ctrl+<letter>` (`Ctrl+d`, `Ctrl+u`,
>   `Ctrl+o`, your `Ctrl+hjkl`…). Neither Vim plugin binds `Ctrl+Shift+<letter>`, so
>   it passes straight through to the IDE even inside the editor.
> - **Works in any focus.** A single modifier chord fires from the editor, terminal,
>   or project tree. A `Space` leader can't — the focused widget eats the leading
>   `Space` (the same reason the old terminal toggle needed a separate key).
> - **Portable.** Kept as literal `Ctrl` on every OS (never swapped to `Cmd`) and
>   limited to letters (no layout-dependent punctuation), so one binding is identical
>   on macOS / Linux / Windows and across IDEs.
>
> **Where it lives:** VS Code → `vscode-keybindings.json` §2 (global, no `when`).
> JetBrains → `jetbrains-keymap.xml` (the **native** keymap, *not* `.ideavimrc`,
> because IdeaVim only fires in the editor and could never toggle a panel from the
> terminal or tree).

| Chord | Action | VS Code command | JetBrains action |
|-------|--------|-----------------|------------------|
| `Ctrl+Shift+E` | Explorer / project tree | `workbench.view.explorer` | `ActivateProjectToolWindow` |
| `Ctrl+Shift+F` | Find in project | `workbench.action.findInFiles` | `FindInPath` |
| `Ctrl+Shift+G` | Source control / Git | `workbench.view.scm` | `ActivateVersionControlToolWindow` |
| `Ctrl+Shift+D` | Run / debug | `workbench.view.debug` | `ActivateDebugToolWindow` |
| `Ctrl+Shift+M` | Problems / diagnostics | `workbench.actions.view.problems` | `ActivateProblemsViewToolWindow` |
| `Ctrl+Shift+T` | Terminal | `workbench.action.terminal.toggleTerminal` | `ActivateTerminalToolWindow` |
| `Ctrl+Shift+O` | Go to file (by name) | `workbench.action.quickOpen` | `GotoFile` |
| `Ctrl+Shift+A` | Find action / command palette | `workbench.action.showCommands` | `GotoAction` |
| `Ctrl+Shift+B` | Toggle sidebar | `workbench.action.toggleSidebarVisibility` | — (native toggle already works) |
| `Ctrl+Shift+W` | Close current file | `workbench.action.closeActiveEditor` | `CloseContent` |
| `Ctrl+Shift+Q` | Close all files | `workbench.action.closeAllEditors` | `CloseAllEditors` |
| `Ctrl+Shift+N` | New terminal tab | `workbench.action.terminal.new` | `Terminal.OpenInTerminal` |
| `Ctrl+Shift+K` | Close terminal tab | `workbench.action.terminal.kill` | `CloseContent` |
| `Ctrl+Shift+H` | Move tab to split (left) | `workbench.action.moveEditorToLeftGroup` | `MoveTabLeft` |
| `Ctrl+Shift+L` | Move tab to split (right) | `workbench.action.moveEditorToRightGroup` | `MoveTabRight` |
> Notice: to close the current file, you can simply use the Vim-native command `:q`
 
> **Behaviour.** Being single modifier chords, all eight work from inside the
> terminal and the project tree too — so the old "can't close the panel from the
> tree" and "`Space` reaches the shell" problems no longer apply. The tool-window
> toggles (`E` `G` `D` `M`) reveal-and-focus when hidden and hide when already
> focused; `Ctrl+Shift+T` toggles the terminal from any context (it replaces the
> old `Cmd+J` workaround).


---

## Dotfiles Layout

```
dotfiles/
  .ideavimrc                        ← committed as `ideavimrc`; symlink to ~/.ideavimrc
  keyboards/
    canonical.md                    ← this file
    vscode-keybindings.json         ← symlink to ~/Library/Application Support/Code/User/
    vscode-settings.json            ← symlink to ~/Library/Application Support/Code/User/
    jetbrains-keymap.xml            ← symlink to ~/.config/JetBrains/<product>/keymaps/
```

### Symlink commands (macOS/Linux)
```bash
# .ideavimrc
ln -sf ~/dotfiles/ideavimrc ~/.ideavimrc

# VS Code keybindings (Layer 1 §1 + Layer 2 §2)
ln -sf ~/dotfiles/keyboards/vscode-keybindings.json \
  "$HOME/Library/Application Support/Code/User/keybindings.json"

# VS Code settings (Layer 1 base Vim remaps)
ln -sf ~/dotfiles/keyboards/vscode-settings.json \
  "$HOME/Library/Application Support/Code/User/settings.json"

# PyCharm native keymap (Layer 2) — adjust product folder name as needed,
# then select "Canonical (Vim + Ctrl+Shift)" in Settings -> Keymap
ln -sf ~/dotfiles/keyboards/jetbrains-keymap.xml \
  "$HOME/.config/JetBrains/PyCharm2024.3/keymaps/my-keymap.xml"
```
