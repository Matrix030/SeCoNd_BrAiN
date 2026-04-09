---
tags: [lld, problem, tree, composite-pattern, recursion]
aliases: [In-Memory File System, FileSystem]
---

# File System

An in-memory file system supporting hierarchical folders and files — no disk, no persistence, just the tree structure and operations.

---

## Requirements

1. Single root directory (Unix-style, e.g. `/home/user/file.txt`)
2. Files store string content
3. Folders contain files and other folders (arbitrary depth)
4. Create and delete files and folders
5. List contents of a folder
6. Resolve absolute paths
7. Rename and move files and folders
8. Retrieve full path from any entry reference
9. Scale to tens of thousands of entries

**Out of scope:** relative paths (`../`), permissions, timestamps, symbolic links, persistence, search

---

## Entities & Relationships

**Path** — not an entity. It's a string input to operations, not something with state. Parse it, don't model it.

**FileSystem** — needed. Without it, path parsing and tree navigation are reimplemented by every caller ([[> LLD Design Principles#DRY]]). It's the Facade that exposes a clean path-based API.

| Entity | Responsibility |
|---|---|
| `FileSystem` | Orchestrator. Owns root, parses paths, exposes all public operations. External code only ever talks to this. |
| `Folder` | Container node. Holds child entries, provides add/remove/lookup. Knows nothing about paths. |
| `File` | Leaf node. Stores content. No children. |

```
FileSystem
 └── root: Folder
       ├── Folder ("home")
       │     └── Folder ("user")
       │           ├── File ("notes.txt")
       │           └── Folder ("docs")
       └── File ("readme.txt")
```

---

## Class Design

### FileSystemEntry — shared abstraction

File and Folder both have `name`, `parent`, and `getPath()`. That's not coincidence — they're both nodes in the same tree. Extract a shared abstract base class.

This is the **Composite pattern**: `File` is the leaf, `Folder` is the composite, `FileSystemEntry` is their shared interface. It also solves the typing problem — Folder's children can be `Map<String, FileSystemEntry>`.

```
abstract class FileSystemEntry:
  - name: string
  - parent: Folder?

  + getName() -> string
  + setName(name)
  + getParent() -> Folder?
  + setParent(Folder?)
  + getPath() -> string          // walks parent chain
  + isDirectory() -> boolean     // abstract
```

**Parent pointer vs stored path:**

| Approach | getPath() | rename/move a folder |
|---|---|---|
| Store path string | O(1) | O(n) — must update every descendant |
| Store parent pointer | O(depth) | O(1) — update one pointer |

Parent pointer wins. File systems rarely exceed 20 levels deep, so O(depth) is negligible. Cascading updates on rename are not.

```
getPath()
    if parent == null → return name    // root returns "/"
    parentPath = parent.getPath()
    if parentPath == "/" → return "/" + name
    return parentPath + "/" + name
```

### File

```
class File extends FileSystemEntry:
  - content: string

  + File(name, content)
  + getContent() -> string
  + setContent(content)
  + isDirectory() -> false
```

### Folder

Children need O(1) lookup by name (path resolution walks the tree one level at a time). A `Map<String, FileSystemEntry>` gives O(1) lookup and enforces unique names for free — the key uniqueness constraint is built into the data structure.

```
class Folder extends FileSystemEntry:
  - children: Map<String, FileSystemEntry>

  + Folder(name)
  + isDirectory() -> true
  + addChild(entry) -> boolean      // sets entry.parent = this
  + removeChild(name) -> FileSystemEntry?   // clears entry.parent
  + getChild(name) -> FileSystemEntry?
  + hasChild(name) -> boolean
  + getChildren() -> List<FileSystemEntry>
```

> [!warning] Bidirectional consistency
> `addChild` must call `entry.setParent(this)`. `removeChild` must call `entry.setParent(null)`. If you forget, `getPath()` silently returns stale paths after a move.

### FileSystem

```
class FileSystem:
  - root: Folder

  + FileSystem()
  + createFile(path, content) -> File
  + createFolder(path) -> Folder
  + delete(path)
  + list(path) -> List<FileSystemEntry>
  + get(path) -> FileSystemEntry
  + rename(path, newName)
  + move(srcPath, destPath)
```

---

## Implementation

### Path helpers (used by all public methods)

```
resolvePath(path)
    if path == "/" → return root
    parts = path.substring(1).split("/")
    current = root
    for part in parts:
        if !current.isDirectory() → throw NotADirectoryException
        child = current.getChild(part)
        if child == null → throw NotFoundException
        current = child
    return current

resolveParent(path)
    if path == "/" → throw InvalidPathException("root has no parent")
    lastSlash = path.lastIndexOf("/")
    parentPath = lastSlash == 0 ? "/" : path.substring(0, lastSlash)
    parent = resolvePath(parentPath)
    if !parent.isDirectory() → throw NotADirectoryException
    return parent

extractName(path)
    return path.substring(path.lastIndexOf("/") + 1)
```

### createFile / createFolder

```
createFile(path, content)
    if path == "/" → throw InvalidPathException
    parent = resolveParent(path)
    name = extractName(path)
    if parent.hasChild(name) → throw AlreadyExistsException
    file = File(name, content)
    parent.addChild(file)        // addChild sets file.parent
    return file
```

`createFolder` is identical — replace `File(name, content)` with `Folder(name)`.

### delete

```
delete(path)
    if path == "/" → throw InvalidPathException("cannot delete root")
    parent = resolveParent(path)
    removed = parent.removeChild(extractName(path))
    if removed == null → throw NotFoundException
```

Deleting a non-empty folder removes the entire subtree — all descendants become unreachable garbage. If you want `rm`-style safety (only delete empty folders), add: `if entry.isDirectory() && !entry.getChildren().isEmpty() → throw`.

### rename

```
rename(path, newName)
    if path == "/" → throw InvalidPathException
    if newName is empty or contains "/" → throw InvalidPathException
    parent = resolveParent(path)
    oldName = extractName(path)
    if !parent.hasChild(oldName) → throw NotFoundException
    if parent.hasChild(newName) → throw AlreadyExistsException
    entry = parent.removeChild(oldName)   // removes from map, clears parent
    entry.setName(newName)
    parent.addChild(entry)                // re-adds under new key, sets parent
```

> [!warning] You can't just call setName() alone
> Children are stored in a map keyed by name. Changing the name without removing and re-adding the entry leaves it orphaned under the old key. Remove → rename → re-add.

### move

```
move(srcPath, destPath)
    if srcPath == "/" → throw InvalidPathException
    srcParent = resolveParent(srcPath)
    srcName = extractName(srcPath)
    entry = srcParent.getChild(srcName)
    if entry == null → throw NotFoundException

    destParent = resolveParent(destPath)
    destName = extractName(destPath)

    // Cycle check: can't move a folder into itself or a descendant
    if entry.isDirectory()
        current = destParent
        while current != null:
            if current == entry → throw InvalidPathException("would create cycle")
            current = current.getParent()

    if destParent.hasChild(destName) → throw AlreadyExistsException
    srcParent.removeChild(srcName)
    entry.setName(destName)
    destParent.addChild(entry)
```

> [!warning] Cycle detection is mandatory
> Moving `/home` into `/home/user/stuff` creates an impossible loop. Walk from `destParent` to root — if you ever hit the entry being moved, reject the operation.

---

## Verification Trace

```
State: root = Folder("/"), empty

createFolder("/home")
  resolveParent → root, name = "home"
  root.addChild(Folder("home")) → home.parent = root

createFolder("/home/user")
  resolveParent → root→home, name = "user"
  home.addChild(Folder("user")) → user.parent = home

createFile("/home/user/notes.txt", "hello")
  resolveParent → home→user, name = "notes.txt"
  user.addChild(File("notes.txt", "hello")) → file.parent = user

file.getPath()
  user.getPath() → home.getPath() → root.getPath() → "/"
  → "/" + "home" = "/home"
  → "/home" + "/" + "user" = "/home/user"
  → "/home/user" + "/" + "notes.txt" = "/home/user/notes.txt" ✓

move("/home/user/notes.txt", "/home/notes.txt")
  entry = file, destParent = home, destName = "notes.txt"
  Cycle check: file is not a directory → skip
  home.hasChild("notes.txt") → false
  user.removeChild("notes.txt") → file.parent = null
  home.addChild(file) → file.parent = home

file.getPath() → "/home/notes.txt" ✓

createFile("/home/notes.txt", "duplicate")
  home.hasChild("notes.txt") → true → AlreadyExistsException ✓

move("/home", "/home/user/stuff")
  entry = home, destParent = user
  Cycle check: user.parent = home = entry → cycle detected ✓
```

---

## Extensibility

**Thread safety** — two options:

- **Coarse-grained lock**: wrap every public method in `synchronized(this)`. Simple and correct. All operations are mutually exclusive.
- **Fine-grained lock**: one lock per Folder, lock only the folder(s) being modified. Better concurrency, but `move` needs to lock two folders simultaneously. Deadlock risk:

```
Thread A: move("/alice/f", "/bob/f") → locks alice, waits for bob
Thread B: move("/bob/g", "/alice/g") → locks bob, waits for alice → deadlock
```

Fix: always acquire folder locks in a consistent order (e.g. by path string alphabetically). Neither thread can form a circular wait.

Read operations (`get`, `list`) can use a `ReadWriteLock` — multiple readers proceed concurrently, writers get exclusive access.

> [!tip] What to say in the interview
> Start with coarse-grained locking. Then say: *"For higher throughput, I'd use per-folder locks — but move touches two folders atomically, so I'd need lock ordering to prevent deadlock."*

**Search by name** — two options:

- **Recursive traversal**: O(n) over all entries. Correct, simple, slow at scale.
- **Name index**: maintain `Map<String, List<FileSystemEntry>>` in FileSystem. Update it on every create/delete/rename. Search becomes O(1). For prefix search, replace the map with a trie.

---

## Related
- [[> LLD Delivery Framework]]
- [[LLD - Entities and Relationships]]
- [[LLD - Class Design]]
- [[> LLD Design Patterns]]
- [[> Concurrency]]
- [[Elevator]]
- [[Amazon Locker]]
