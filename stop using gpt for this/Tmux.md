You can rename a **tmux window** in a couple of ways:

### 1. From inside tmux

Run:

```bash
Ctrl+b , 
```

(hold `Ctrl`, press `b`, then press the comma `,`)  
This opens a prompt at the bottom where you can type the new window name. Press **Enter** to confirm.

---

### 2. With a tmux command

From inside tmux:

```bash
tmux rename-window new_name
```

Or target a specific window (e.g., window 2):

```bash
tmux rename-window -t 2 new_name
```

---

### 3. From outside tmux (if you know the session name)

```bash
tmux rename-window -t session_name:window_index new_name
```

---

Do you want me to also show you how to make windows **auto-rename** based on the running program (like `vim`, `htop`, etc.), or do you prefer keeping custom names?

# In tmux, you can open a new pane with these shortcuts:

- **Split vertically (side-by-side):**
	Ctrl+b %

Split horizontally (top/bottom):
Ctrl+b "

Switch to next pane (cycle):
Ctrl+b o

**Switch to a specific direction:**

- Left: `Ctrl+b ←`
    
- Right: `Ctrl+b →`
    
- Up: `Ctrl+b ↑`
    
- Down: `Ctrl+b ↓`

Swap the current pane with the next one:
Ctrl+b {

Swap the current pane with the previous one:
Ctrl+b }


# Switch window number:
In **tmux**, windows are numbered automatically starting from 0. If you want to move a window from index **1** to index **2**, you can use the `move-window` command.

Here are two ways:

### 1. From inside tmux command prompt

Press:

```
Ctrl+b :
```

then type:

```
move-window -s 1 -t 2
```

This moves the window currently at index 1 into position 2.

### 2. From the shell

If you’re outside or scripting:

```bash
tmux move-window -s 1 -t 2
```

### Notes

- `-s` = source window index
    
- `-t` = target window index
    
- If a window already exists at the target index, tmux will swap them unless you add `-a` or `-b` for “after/before” behavior.
    
- To renumber automatically after closing/moving windows, you can add this to your `~/.tmux.conf`:
    
    ```tmux
    set -g renumber-windows on
    ```
    

Do you want me to also show you how to **swap** instead of move (so windows exchange positions)?

# Swap window number:
If by _switch_ you mean **swap two windows’ indices** (so they exchange positions), tmux has a built-in command:

### From inside tmux

Press:

```
Ctrl+b :
```

then type:

```
swap-window -s 1 -t 2
```

This swaps the window at index `1` with the window at index `2`.

### From the shell

```bash
tmux swap-window -s 1 -t 2
```

### Handy shortcut

If you want to swap the **current window** with another one (say index 2), you can do:

```bash
swap-window -t 2
```

---
Difference recap:

- `move-window` = move a window to a new index (replacing if needed).
    
- `swap-window` = exchange positions of two windows.
    

Do you want me to also give you a binding (like `Ctrl+b S`) so you can quickly swap windows without typing commands?sflix