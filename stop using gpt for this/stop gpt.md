#reset-gitignore
reset gitignore  --cache

git rm --cached -r .
git add .
git commit -m 'ahahahhahahhaha''

#create-checkboxes
- [ ]  like this

vim end of line:
$
vim start of  line:
0
vim white - space skip:
 ^
 jump to new line insert mode:
 o

jump to previous line insert mode:
O


diagnostics:
control + w  + d

delete the words you just typed (insert mode):
control + w

shift codeblock (visual mode):
shift + >


In tmux, you can open a new pane with these shortcuts:

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

##  1. Delete until a character (exclusive)

Use `d` with **t** (till) or **f** (find):

- `dtx` → delete **up to (but not including)** the first occurrence of `x`.
- `dfx` → delete **through (including)** the firstc occurrence of `x`.


Comment selected text (visual mode):
gc

comment cursor text (normal mode):
gcc

check what a function does (normal mode):
K  (while cursor is on the function)

## Tmux
2. Step Resize with Prefix + Ctrl + Arrow
rename a window:
control + b + ,