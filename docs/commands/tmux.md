# tmux

## Sessions

- Start a new session with a name:
    - `tmux`
    - `tmux new`
    - `tmux new -s <name>`
    - `:new`
    - `:new -s <name>`
- List sessions:
    - `tmux ls`
    - `tmux list-sessions`
    - `:ls`
    - `:list-sessions`
- Attach to a session:
    - `tmux a`
    - `tmux a -t <name>`
    - `tmux attach`
    - `tmux attach -t <name>`
    - `:a`
    - `:a -t <name>`
    - `:attach`
    - `:attach -t <name>`
- Detach from a session:
    - `tmux detach`
    - `:detach`
    - `ctrl-b` `d`
- Kill a session:
    - `tmux kill-session`
    - `tmux kill-session -t <name>`
    - `:kill-session`
    - `:kill-session -t <name>`
- Rename a session:
    - `tmux rename-session -t <old> <new>`
    - `:rename-session -t <old> <new>`
    - `ctrl-b` `$`
- Kill/delete all session but mysession:
    - `tmux kill-session -a -t mysession`
    - `:kill-session -a -t mysession`
- Move to next session:
    - `ctrl-b` `(`
- Move to previous session:
    - `ctrl-b` `)`
- Move to last session:
    - `ctrl-b` `l`
- Move to session by name:
    - `ctrl-b` `s`
- Move to session by number:
    - `ctrl-b` `w`
- Session and windows preview:
    - `ctrl-b` `f`


## Windows

- start a new session with the name mysession and window mywindow
    - `tmux new -s mysession -n mywindow`
- Create window
    - `ctrl+b` `c`
- Rename current window
    - `Ctrl + b` `,`
- Close current window
    - `Ctrl + b` `&`
- List windows
    - `Ctrl + b` `w`
- Previous window
    - `Ctrl + b` `p`
- Next window
    - `Ctrl + b` `n`
- Switch/select window by number
    - `Ctrl + b` `0` ... `9`
- Toggle last active window
    - `Ctrl + b` `l`
- Reorder window, swap window number 2(src) and 1(dst)
    - `:swap-window -s 2 -t 1`
- Move current window to the left by one position
    - `:swap-window -t -1`


## Panes

- Toggle last active pane
    - `Ctrl + b` `;`
- Split pane with horizontal layout
    - `Ctrl + b` `%`
- Split pane with vertical layout
    - `Ctrl + b` `"`
- Move the current pane left
    - `Ctrl + b` `{`
- Move the current pane right
    - `Ctrl + b` `}`
- Switch to pane to the direction
    - `Ctrl + b` `<arrow-key>`
- Show pane numbers
    - `Ctrl + b` `q`
- Switch/select pane by number
    - `Ctrl + b` `q` `0` ... `9`
- Toggle pane zoom
    - `Ctrl + b` `z`
- Convert pane into a window
    - `Ctrl + b` `!`
- Resize current pane height(holding second key is optional)
    - `Ctrl + b + <arrow-up-key>`
    - `Ctrl + b` `Ctrl + <arrow-up-key>`
    - `Ctrl + b + <arrow-down-key>`
    - `Ctrl + b` `Ctrl + <arrow-down-key>`
- Resize current pane width(holding second key is optional)
    - `Ctrl + b + <arrow-right-key>`
    - `Ctrl + b` `Ctrl + <arrow-right-key>`
    - `Ctrl + b + <arrow-left-key>`
    - `Ctrl + b` `Ctrl + <arrow-left-key>`
- Close current pane
    - `Ctrl + b` `x`
- Toggle synchronize-panes(send command to all panes)
    - `:setw synchronize-panes`
- Toggle between pane layouts
    - `Ctrl + b` `Spacebar`
- Switch to next pane
    - `Ctrl + b` `o`

## Copy Mode

- use vi keys in buffer
    - `:setw -g mode-keys vi`
- Enter copy mode
    - `Ctrl + b` `[`
- Enter copy mode and scroll one page up
    - `Ctrl + b` `PgUp`
- Quit mode
    - `q`
- Go to top line
    - `g`
- Go to bottom line
    - `G`
- Scroll down
    - `<arrow-down-key>`
- Scroll up
    - `<arrow-up-key>`
- Move cursor left
    - `h`
- Move cursor down
    - `j`
- Move cursor up
    - `k`
- Move cursor right
    - `l`
- Move cursor forward one word at a time
    - `w`
- Move cursor backward one word at a time
    - `b`
- Search forward
    - `/`
- Search backward
    - `?`
- Next keyword occurance
    - `n`
- Previous keyword occurance
    - `N`
- Start selection
    - `Spacebar`
- Clear selection
    - `Esc`
- Copy selection
    - `Enter`
- Paste contents of buffer_0
    - `Ctrl + b` `]`
- display buffer_0 contents
    - `show-buffer`
- Copy entire visible contents of pane to a buffer
    - `capture-pane`
- Show all buffers
    - `list-buffers`
- Show all buffers and paste selected
    - `choose-buffer`
- Save buffer contents to buf.txt
    - `save-buffer buf.txt`
- Delete buffer_1
    - `delete-buffer -b 1`

## Misc

- Enter command mode
    - `Ctrl + b` `:`
- Set OPTION for all sessions
    - `:set -g OPTION`
- Set OPTION for all windows
    - `:setw -g OPTION`
- Enable mouse mode
    - `:set mouse on`


## Help

- List key bindings(shortcuts)
    - `tmux list-keys`
    - `list-keys`
    - `Ctrl + b` `?`
- Show every session, window, pane, etc...
    - `tmux info`
