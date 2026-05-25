# Operating System Concepts — Agent Formatting Guide

Book: *Operating System Concepts* — Silberschatz, Galvin, and Gagne (the "Dinosaur Book").

## Workflow

Rishi reads a sub-topic and **drops all his raw notes for it directly into that sub-topic's MOC file** — the `> <Sub-topic>.md` file, written however he likes. So when you open a MOC file and find raw notes instead of an index, that is the signal to process it. The agent's job is to **split that content into sub-sub-topic leaf notes**, then **rewrite the file in place as the MOC**.

The MOC file holding raw notes is the input. The agent:

1. **Read the MOC file** in the sub-topic folder (e.g. `Introduction/What OSs do/> What OSs do.md`). If it contains raw notes rather than an index, process it.
2. **Infer the sub-sub-topic boundaries** from the structure Rishi wrote — his headings, sections, or natural topic breaks signal where one sub-sub-topic ends and the next begins. The content he gives will be structured enough to read these boundaries from; do not invent divisions he didn't imply.
3. **Split** the content into one leaf note per sub-sub-topic, in the same folder, named after that sub-sub-topic (matching the book's TOC casing). Move his words across **verbatim** — no rewriting.
4. **Rewrite the MOC file in place** — replace the raw notes with the MOC index (frontmatter + `> [!info]` + a link to each new leaf note with a one-line description). The raw notes are now fully represented by the leaf notes, so they leave the MOC.
5. **Structure** each leaf note per the steps below (frontmatter, wikilinks, callouts, `## Related`).
6. **Update parent MOCs upward** — append this sub-topic to the **chapter MOC** (`> <Chapter>.md`), and ensure the chapter is listed in the **book MOC** (`> Operating System Concepts.md`). Create either MOC if it doesn't exist yet.

> [!important] Core rule — never rewrite
> The user's words stay exactly as written. Agents only add structure: frontmatter, heading adjustments, wikilinks, callout wrappers, and a `## Related` section. Do not paraphrase, summarise, or expand the user's text. Splitting moves text from the raw MOC file into leaf notes — it never edits the words.

> [!warning] Don't lose content when rewriting the MOC
> The MOC file starts as the raw notes and ends as an index. Before replacing its contents, make sure **every** piece of the raw text has landed in a leaf note. If any part doesn't map cleanly to a sub-sub-topic, stop and ask rather than dropping it.

> [!important] Work one sub-topic at a time — update parent MOCs incrementally
> Content arrives **one sub-topic at a time**, not a whole chapter at once. So the chapter MOC and book MOC are built up gradually:
> - When a new sub-topic is processed, **append** its link to the existing chapter MOC's sub-topic list. Do **not** regenerate the file or touch links already there.
> - If the chapter MOC doesn't exist yet (this is the chapter's first sub-topic), create it with this one entry, then add the chapter to the book MOC.
> - Same rule one level up: a new chapter appends to the book MOC; existing chapter links are left untouched.
>
> Treat every parent-MOC update as an **insert**, never a rewrite. Preserve existing entries, ordering them to match the book's table of contents.

---

## Folder Layout

The book's table of contents is hierarchical (Chapter → Sub-topic → Sub-sub-topic), so the vault mirrors that hierarchy with **nested folders**. This follows the same pattern as `System Design/HLD/` (folder → `> <Name>.md` MOC → leaf notes), just one level deeper.

This is the actual built structure of Chapter 1 — it is the canonical worked example; replicate its shape for every chapter:

```
Books/Operating System Concepts/
├── > Operating System Concepts.md          ← book MOC (lists chapters)
├── CLAUDE.md
└── Introduction/                            ← chapter = folder
    ├── > Introduction.md                    ← chapter MOC (index)
    ├── Introduction.md                      ← chapter preamble (leaf)
    └── What OSs do/                          ← sub-topic = subfolder
        ├── > What OSs do.md                  ← sub-topic MOC (index)
        ├── Computer System Components.md     ← section preamble (leaf)
        ├── User View.md                      ← sub-sub-topic (leaf)
        ├── System View.md                    ← sub-sub-topic (leaf)
        └── Defining Operating Systems.md     ← sub-sub-topic (leaf)
```

**The rule at every level:** anything that has children is a **folder** containing a MOC file named `> <Same Name>.md`. The leaf (a sub-sub-topic with no children) is a plain content note.

| Book hierarchy    | Vault representation                              |
| ----------------- | ------------------------------------------------- |
| `1. Introduction` | folder `Introduction/` + MOC `> Introduction.md`  |
| `1.1 What OSs do` | subfolder `What OSs do/` + MOC `> What OSs do.md` |
| `1.1.1 User View` | leaf note `User View.md`                          |

Naming:
- **Names match the book's table of contents verbatim** — no `Chapter N -` prefix, and preserve the book's own casing (`What OSs do`, not forced Title Case). Plain ASCII only — no emojis, no special characters.
- The `> ` prefix on MOC files makes them sort to the top in Obsidian's file explorer. Keep it.
- A MOC's name matches its folder's name (`What OSs do/` → `> What OSs do.md`).

> [!tip] When a level has no sub-divisions
> If a sub-topic has no sub-sub-topics in the book, it stays a plain leaf note — don't create an empty folder + MOC just for one note.

### Preamble prose — MOCs stay pure index

A chapter or section usually opens with un-numbered intro prose **before** its first numbered sub-topic. **This prose does not stay in the MOC** — MOCs are index-only (frontmatter + one-line `> [!info]` + links), matching the HLD gold standard. Instead, preamble becomes its own **leaf note**, named for what it actually covers — never a generic "Overview":

- **Chapter preamble** → a leaf note named after the chapter (e.g. `Introduction/Introduction.md`, sitting beside the `> Introduction.md` MOC). The chapter MOC links to it under an `## Overview` heading.
- **Section preamble** → a leaf note named for its content (e.g. the "four components of a computer system" intro became `Computer System Components.md`, not `Overview.md`). The sub-topic MOC lists it first among its notes.

So a chapter folder can hold two files that look similar but differ by role:

| File | Role |
|---|---|
| `> Introduction.md` | MOC — index of the chapter |
| `Introduction.md` | leaf — the chapter's intro prose |

Link them with `[[Introduction]]` (leaf) vs `[[> Introduction]]` (MOC) — the `> ` disambiguates.

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

- [[> Introduction]] — what an OS does, its components, and defining operating systems

---

## Related

- [[Home]] — vault dashboard
```

### Chapter MOC — e.g. `Introduction/> Introduction.md`

If the chapter has preamble prose, list its leaf under `## Overview`; list each sub-topic MOC under `## Sub-topics`.

```md
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["Introduction"]
---

# Introduction — Map of Content

> [!info] Chapter 1
> A grand tour of the major components of operating systems and the basic organization of computer systems.

---

## Overview

- [[Introduction]] — chapter intro and objectives

## Sub-topics

- [[> What OSs do]] — user view, system view, and defining operating systems

---

## Related

- [[> Operating System Concepts]] — back to the book MOC
```

### Sub-topic MOC — e.g. `Introduction/What OSs do/> What OSs do.md`

List notes in book order. If a section preamble leaf exists, it comes first.

```md
---
tags: [book, os, operating-systems, book-os-concepts, moc]
aliases: ["What OSs do"]
---

# What OSs do — Map of Content

> [!info] Section 1.1
> What an operating system is — seen from the user's side and the system's side — and how we ultimately define one.

---

## Notes

- [[Computer System Components]] — the four components of a computer system and the OS's role among them
- [[User View]] — the OS from the user's perspective
- [[System View]] — the OS as resource allocator and control program
- [[Defining Operating Systems]] — why OSs exist, and the kernel

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