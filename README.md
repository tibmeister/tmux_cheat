- [tmux shortcuts & cheatsheet](#tmux-shortcuts--cheatsheet)
  - [Various Tips and Tricks](#various-tips-and-tricks)
  - [Commandline](#commandline)
  - [In-Session](#in-session)
    - [Sessions](#sessions)
    - [Windows (tabs)](#windows-tabs)
    - [Panes (splits)](#panes-splits)
    - [Sync Panes](#sync-panes)
    - [Resizing Panes](#resizing-panes)
    - [Copy mode:](#copy-mode)
    - [Misc](#misc)
    - [Commands/Misc](#commandsmisc)
    - [Configurations Options:](#configurations-options)
- [Resources:](#resources)
# tmux shortcuts & cheatsheet

## Various Tips and Tricks
To start tmux at login without nesting, add the following code to your ~/.bashrc

```bash
case $- in
    *i*)
        if command -v tmux>/dev/null; then
            if [[ ! $TERM =~ screen ]] && [[ -z $TMUX ]]; then
                if tmux ls 2> /dev/null | grep -q -v attached; then
                    exec tmux attach -t $(tmux ls 2> /dev/null | grep -v attached |  head -1 | cut -d : -f 1)
                else
                    exec tmux
                fi
            fi
        fi
    ;;
esac
```
The advantage of this block of code is that when you exit the tmux session, it will also exit you out of your bash session, saving some typing.

## Commandline
start new:

    tmux

start new with session name:

    tmux new -s myname

attach:

    tmux a  #  (or at, or attach)

attach to named:

    tmux a -t myname

list sessions:

    tmux ls

kill session:

    tmux kill-session -t myname

Kill all the tmux sessions:

    tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill

## In-Session
In tmux, hit the prefix `ctrl+b` (this is default, adjust as needed).  All commands below, unless otherwise specified, are keypresses after the prefix.

### Sessions
| Modifier key | Description | 
| :-- | --: | 
| :new<CR> | new session |
| s | list sessions |
| $ | name session |

### Windows (tabs)
| Modifier key | Description |
| :-- | --: |
| c | create window |
| w | list windows |
| n | next window |
| p | previous window |
| f | find window |
| , | name window |
| & | kill window |

### Panes (splits) 
| Modifier key | Description |
| :-- | --: |
| % | vertical split |
| " | horizontal split |
| o | swap panes |
| q | show pane numbers |
| x | kill pane |
| + | break pane into window (e.g. to select text by mouse to copy) |
| - | restore pane from window |
| ⍽ | space - toggle between layouts |
| q | (Show pane numbers, when the numbers show up type the key to goto that pane) |
| { | (Move the current pane left) |
| } | (Move the current pane right) |
| z | toggle pane zoom |

### Sync Panes 

You can do this by switching to the appropriate window, typing your Tmux prefix (commonly Ctrl-B or Ctrl-A) and then a colon to bring up a Tmux command line, and typing:

```
:setw synchronize-panes
```

You can optionally add on or off to specify which state you want; otherwise the option is simply toggled. This option is specific to one window, so it won’t change the way your other sessions or windows operate. When you’re done, toggle it off again by repeating the command. [tip source](http://blog.sanctum.geek.nz/sync-tmux-panes/)


### Resizing Panes

You can also resize panes if you don’t like the layout defaults. I personally rarely need to do this, though it’s handy to know how. Here is the basic syntax to resize panes:

    <prefix>: resize-pane -D (Resizes the current pane down)
    <prefix>: resize-pane -U (Resizes the current pane upward)
    <prefix>: resize-pane -L (Resizes the current pane left)
    <prefix>: resize-pane -R (Resizes the current pane right)
    <prefix>: resize-pane -D 20 (Resizes the current pane down by 20 cells)
    <prefix>: resize-pane -U 20 (Resizes the current pane upward by 20 cells)
    <prefix>: resize-pane -L 20 (Resizes the current pane left by 20 cells)
    <prefix>: resize-pane -R 20 (Resizes the current pane right by 20 cells)
    <prefix>: resize-pane -t 2 20 (Resizes the pane with the id of 2 down by 20 cells)
    <prefix>: resize-pane -t -L 20 (Resizes the pane with the id of 2 left by 20 cells)
    
    
### Copy mode:

Pressing PREFIX [ places us in Copy mode. We can then use our movement keys to move our cursor around the screen. By default, the arrow keys work. we set our configuration file to use Vim keys for moving between windows and resizing panes so we wouldn’t have to take our hands off the home row. tmux has a vi mode for working with the buffer as well. To enable it, add this line to .tmux.conf:

    setw -g mode-keys vi

With this option set, we can use h, j, k, and l to move around our buffer.

To get out of Copy mode, we just press the ENTER key. Moving around one character at a time isn’t very efficient. Since we enabled vi mode, we can also use some other visible shortcuts to move around the buffer.

For example, we can use "w" to jump to the next word and "b" to jump back one word. And we can use "f", followed by any character, to jump to that character on the same line, and "F" to jump backwards on the line.

| Function | vi | emacs |
| :-- | :--: | --: |
| Back to indentation | ^ | M-m |
| Clear selection | Escape | C-g |
| Copy selection | Enter | M-w |
| Cursor down | j | Down |
| Cursor left |  h | Left |
| Cursor right | l | Right |
| Cursor to bottom line | L ||
| Cursor to middle line | M | M-r |
| Cursor to top line | H | M-R |
| Cursor up | k | Up |
| Delete entire line | d | C-u |
| Delete to end of line | D | C-k |
| End of line | $ | C-e |
| Goto line | : | g |
| Half page down | C-d | M-Down |
| Half page up | C-u | M-Up |
| Next page | C-f | Page down |
| Next word | w | M-f |
| Paste buffer | p | C-y |
| Previous page | C-b | Page up |
| Previous word | b | M-b |
| Quit mode | q | Escape |
| Scroll down | C-Down or J | C-Down |
| Scroll up | C-Up or K | C-Up |
| Search again | n | n |
| Search backward | ? | C-r |
| Search forward | / | C-s |
| Start of line | 0 | C-a |
| Start selection | Space | C-Space |
| Transpose chars | | C-t |

### Misc
| Modifier key | Description |
| :-- | --: |
| d | detach |
| t | big clock |
| ? | list shortcuts |
| : | prompt/command |

### Commands/Misc
    PREFIX : join-pane -s 2 -t 1 (Joins two windows into one as seperate panes with -s being Source and -t being Target)

### Configurations Options:

    # Mouse support - set to on if you want to use the mouse
    * :setw -g mouse [on|off]

    # Set the default terminal mode to 256color mode
    set -g default-terminal "screen-256color"

    # enable activity alerts
    setw -g monitor-activity on
    set -g visual-activity on

    # Center the window list
    set -g status-justify centre

# Resources:

* [tmux: Productive Mouse-Free Development](http://pragprog.com/book/bhtmux/tmux)
* [How to reorder windows](http://superuser.com/questions/343572/tmux-how-do-i-reorder-my-windows)
