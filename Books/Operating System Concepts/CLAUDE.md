# Operating System Concepts — Agent Formatting Guide

Book: *Operating System Concepts* — Silberschatz, Galvin, and Gagne (the "Dinosaur Book").

## Workflow

Rishi drops in raw content — passages, highlights, or ideas from the book that he finds interesting or wants to remember. **The agent's job is to organise that content neatly into the knowledge base.** This means:

1. **Identify** where the content sits in the book hierarchy — which chapter, sub-topic, and (if applicable) sub-sub-topic.
2. **Route** it to the right leaf note, creating any missing folders + MOCs along the path (chapter folder → sub-topic subfolder → leaf note). See **Folder Layout** below.
3. **Structure** it using the steps below (frontmatter, wikilinks, callouts, `## Related`).
4. **Update every MOC on the path** — the book MOC, the chapter MOC, and the sub-topic MOC — whenever a new folder or note is created so the new entry is linked from its parent.

> [!important] Core rule — never rewrite
> The user's words stay exactly as written. Agents only add structure: frontmatter, heading adjustments, wikilinks, callout wrappers, and a `## Related` section. Do not paraphrase, summarise, or expand the user's text.

---

## Folder Layout

The book's table of contents is hierarchical (Chapter → Sub-topic → Sub-sub-topic), so the vault mirrors that hierarchy with **nested folders**. This follows the same pattern as `System Design/HLD/` (folder → `> <Name>.md` MOC → leaf notes), just one level deeper.

```
Books/Operating System Concepts/
├── > Operating System Concepts.md     ← book MOC (lists chapters)
├── CLAUDE.md
└── Introduction/                       ← chapter = folder
    ├── > Introduction.md               ← chapter MOC (lists sub-topics)
    └── What OSs do/                     ← sub-topic = subfolder
        ├── > What OSs do.md             ← sub-topic MOC (lists sub-sub-topics)
        ├── User View.md                 ← sub-sub-topic = leaf note
        └── System View.md               ← sub-sub-topic = leaf note
```

**The rule at every level:** anything that has children is a **folder** containing a MOC file named `> <Same Name>.md`. The leaf (a sub-sub-topic with no children) is a plain content note.

| Book hierarchy | Vault representation |
|---|---|
| `1. Introduction` | folder `Introduction/` + MOC `> Introduction.md` |
| `1.1 What OSs do` | subfolder `What OSs do/` + MOC `> What OSs do.md` |
| `1.1.1 User View` | leaf note `User View.md` |

Naming:
- **Names match the book's table of contents verbatim** — no `Chapter N -` prefix, and preserve the book's own casing (`What OSs do`, not forced Title Case). Plain ASCII only — no emojis, no special characters.
- The `> ` prefix on MOC files makes them sort to the top in Obsidian's file explorer. Keep it.
- A MOC's name matches its folder's name (`What OSs do/` → `> What OSs do.md`).

> [!tip] When a level has no sub-divisions
> If a sub-topic has no sub-sub-topics in the book, it stays a plain leaf note — don't create an empty folder + MOC just for one note.

---

## Step 1 — Frontmatter

Every note needs YAML frontmatter. Add it if missing; do not overwrite if it already exists.

### Chapter / content note
```yaml
---
tags: [book, os, operating-systems, book-os-concepts, <topic-tags>]
aliases: ["Alternate name if useful"]
---
```

### MOC note
```yaml
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["OS Concepts MOC", "Dinosaur Book"]
---
```

### OS topic tag conventions

| Tag | When to use |
|---|---|
| `os` | Every note in this folder |
| `operating-systems` | Every note in this folder |
| `book-os-concepts` | Every note in this folder (book-scoped) |
| `processes` | Processes, threads, context switching |
| `scheduling` | CPU scheduling algorithms |
| `memory` | Memory management, paging, segmentation |
| `virtual-memory` | Virtual memory, page replacement |
| `file-systems` | File systems, directories, storage |
| `concurrency` | Synchronisation, deadlocks, semaphores, monitors |
| `io` | I/O systems, device drivers |
| `security` | Protection, security chapter notes |
| `networking` | Distributed systems, networking notes |
| `moc` | MOC / index notes only |

---

## Step 2 — MOCs (one per folder)

There is a MOC at **every level that has children** — the book root, each chapter, and each sub-topic. A MOC is an index only — no content, just links to its direct children, each with a one-line description. Whenever a new folder or note is created, link it from its parent MOC.

### Book MOC — `> Operating System Concepts.md`
```md
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["OS Concepts MOC", "Dinosaur Book"]
---

# Operating System Concepts — Map of Content

> [!info] Book
> *Operating System Concepts* — Silberschatz, Galvin, and Gagne. Chapter notes and key concepts.

---

## Chapters

- [[> Introduction]] — what an OS does, computer-system organisation, structure

---

## Related

- [[> LLD Delivery Framework]] — system design section MOC
- [[> AWS]] — cloud / OS-adjacent topics
```

### Chapter MOC — e.g. `Introduction/> Introduction.md`
```md
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["Introduction"]
---

# Introduction — Map of Content

> [!info] Chapter 1
> Sub-topics covered in the Introduction chapter.

---

## Sub-topics

- [[> What OSs do]] — user view, system view, defining an OS

---

## Related

- [[> Operating System Concepts]] — back to the book MOC
```

### Sub-topic MOC — e.g. `Introduction/What OSs do/> What OSs do.md`
```md
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["What OSs do"]
---

# What OSs do — Map of Content

> [!info] Section 1.1
> Sub-sub-topics under "What Operating Systems Do".

---

## Notes

- [[User View]] — the OS from the user's perspective
- [[System View]] — the OS as a resource allocator / control program

---

## Related

- [[> Introduction]] — back to the chapter MOC
```

Each MOC's `## Related` links **up** to its parent MOC. Leaf notes (Step 5) link back to their sub-topic MOC.

---

## Step 3 — Wikilinks

Add wikilinks inside the user's text where a concept appears:

- Link to sibling or related notes: `[[System View]]`, `[[> What OSs do]]`
- Link to other vault notes where naturally relevant: `[[TCP]]`, `[[Load Balancing]]`
- Link to headings within the same note: `[[#Section Name]]`
- Do **not** add links inside code blocks or image embeds

Do not change surrounding words — only wrap the relevant term in `[[brackets]]`.

---

## Step 4 — Callout Wrappers

When raw text clearly matches one of these patterns, wrap it — do not invent callouts:

| Pattern in raw text | Callout to use |
|---|---|
| Background / "what is X" context | `> [!info]` |
| A tip, trick, or mental model | `> [!tip]` |
| A common mistake or pitfall | `> [!warning]` |
| A side note or clarification | `> [!note]` |

---

## Step 5 — `## Related` Section

Every leaf note (not a MOC) ends with a `## Related` section, 3–5 links:

```md
## Related

- [[> What OSs do]] — back to the sub-topic MOC
- [[System View]] — the sibling note
- [[Processes]] — logically related concept elsewhere in the book
```

The first link always points back to the note's **immediate parent MOC** (the sub-topic MOC it lives under).

---

## What Agents Must NOT Do

- **Do not rewrite, paraphrase, or "improve" the user's text** — structure only
- **Do not create new content** beyond frontmatter, callout wrappers, and `## Related`
- **Do not change heading words** — only heading levels when splitting (`##` → `#`)
- **Do not use emojis** in file names or note content
- **Do not overwrite existing frontmatter** — only add missing fields
- **Do not touch files outside `Books/Operating System Concepts/`**
- **Never co-author git commits**

---

## Reference

Look at `Books/Essential Scrum/` as a working example of the chapter-note + MOC pattern used in this vault.
