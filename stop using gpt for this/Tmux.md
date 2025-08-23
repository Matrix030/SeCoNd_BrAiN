You can rename a **tmux window** in a couple of ways:

### 1. From inside tmux

Run:

```bash
Ctrl+b , 
```

(hold `Ctrl`, press `b`, then press the comma `,`)  
👉 This opens a prompt at the bottom where you can type the new window name. Press **Enter** to confirm.

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
