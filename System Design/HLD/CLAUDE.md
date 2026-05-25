# HLD Notes — Agent Formatting Guide

This file tells agents how to format raw HLD notes written by Rishi. The words are his — agents only add structure.

> [!important] Read `STYLE.md` too
> This file covers **structure** (frontmatter, splitting into folders, MOC pattern, wikilinks, `## Related`). The companion `STYLE.md` covers **style recognition** — how to detect when raw text should become a callout, a numbered list, a bold term, a table, etc. Agents must read both before formatting raw text.

---

## Core Rule

**Never rewrite the user's words.** The text stays exactly as written. Agents only add:
- YAML frontmatter
- Heading level adjustments (when splitting into standalone files)
- Wikilinks within existing text where relevant
- A `## Related` section at the bottom of each note
- A MOC (`> FileName.md`) when splitting a large note into a folder
- Markdown wrappers (callouts, lists, bold, tables) where the raw text matches recognizable patterns — see `STYLE.md`

---

## Step 1 — Add Frontmatter

Every note needs YAML frontmatter. If it's missing, add it. If it already exists, do not overwrite it.

```yaml
---
tags: [system-design, hld, <topic-specific-tags>]
aliases: ["Alternate Name", "Short Name"]
---
```

**Tag conventions for HLD notes:**

| Tag                  | When to use                         |
| -------------------- | ----------------------------------- |
| `system-design`      | Every HLD note                      |
| `hld`                | Every HLD note                      |
| `networking`         | Networking section notes            |
| `tcp`, `udp`, `http` | Protocol-specific notes             |
| `load-balancing`     | Load balancing notes                |
| `scaling`            | Scaling, CDN, regional notes        |
| `fault-tolerance`    | Retries, circuit breakers, timeouts |
| `api`                | REST, GraphQL, gRPC notes           |
| `real-time`          | SSE, WebSockets, WebRTC notes       |
| `moc`                | MOC / index notes only              |

---

## Step 2 — Splitting a Monolithic Note into a Folder

When a note covers multiple distinct topics and is too long to navigate easily, split it into per-topic files inside a new subfolder.

### Folder and file naming

- Create a subfolder named after the topic (e.g., `Networking/`)
- Create `> TopicName.md` inside the folder as the MOC — the `> ` prefix makes it sort to the top in Obsidian's file explorer
- Each sub-topic gets its own file named after the topic (e.g., `TCP.md`, `Load Balancing.md`)
- File names use Title Case, plain ASCII — no emojis, no special characters

### What goes in each sub-note

Take the relevant section of the original note verbatim and put it in the sub-note. Then:

1. **Promote the top-level heading** — if the section was an `##` or `###` in the original, make it `#` in the standalone file (relative sub-headings stay the same)
2. **Add frontmatter** (tags + aliases)
3. **Add a `## Related` section** at the bottom with wikilinks to the MOC and 2–4 closely related notes

### What goes in the MOC (`> TopicName.md`)

The MOC is an index, not a content note. It should have:
- Frontmatter with `moc` tag
- A one-line `> [!info]` callout describing the section's purpose
- A grouped list of `[[wikilinks]]` — one per sub-note — each with a short one-line description
- Optionally a `> [!tip]` at the bottom with an interview mental model or key takeaway

```md
---
tags: [system-design, hld, <topic>, moc]
aliases: ["Topic Name"]
---

# Topic Name — Map of Content

> [!info] About this section
> One-line purpose statement.

---

## Group 1
- [[Sub-note A]] — one-line description
- [[Sub-note B]] — one-line description

## Group 2
- [[Sub-note C]] — one-line description

> [!tip] Key takeaway or interview mental model
```

### What to do with the original monolithic file

After splitting, replace the original file's content with a thin pointer that embeds the MOC:

```md
---
tags: [system-design, hld, <topic>, moc]
aliases: ["Topic Name"]
---

# Topic Name

See [[> Topic Name]] for the full map of content and all sub-notes.

![[> Topic Name]]
```

This preserves any existing `[[wikilinks]]` pointing to the original file while redirecting readers to the new structure.

---

## Step 3 — Add Wikilinks

After adding frontmatter and splitting (if needed), add wikilinks within the text where relevant:

- Link to other notes in the vault: `[[TCP]]`, `[[Load Balancing]]`
- Link to headings within the same note: `[[#Section Name]]`
- Link to headings in other notes: `[[Load Balancing#Layer 4 Load Balancers|L4]]`
- Do **not** add links in code blocks or image embeds

Do not change the surrounding words — only wrap the relevant term in `[[brackets]]`.

---

## Step 4 — Add `## Related` Section

Every standalone note (not MOC) ends with a `## Related` section. Keep it to 3–5 links:

```md
## Related

- [[> Networking Essentials]] — back to the section MOC
- [[TCP]] — the reliable alternative
- [[Load Balancing]] — how traffic gets distributed across servers
```

The first link should always go back to the parent MOC. The rest link to concepts that a reader of this note would logically want next.

---

## What Agents Must NOT Do

- **Do not rewrite, paraphrase, or "improve" the user's text** — structure only
- **Do not add explanatory comments** like "This section covers..." unless the user wrote them
- **Do not create new content sections** beyond frontmatter and `## Related`
- **Do not change heading words** — only heading levels (`###` → `#`)
- **Do not use emojis** in file names or note content
- **Do not touch files in other vault sections** (ML, Algorithms, etc.)
- **Do not overwrite existing frontmatter** — only add missing fields
- **Never co-author git commits**

---

## Reference Example

The `Networking/` folder in `HLD/Core Concepts/` is the canonical example of this workflow done correctly. When in doubt, look there.

```
HLD/Core Concepts/Networking/
├── > Networking Essentials.md   ← MOC
├── TCP.md
├── UDP.md
├── Transport Layer Protocols.md
├── HTTP and HTTPS.md
├── REST.md
├── GraphQL.md
├── gRPC.md
├── Server-Sent Events.md
├── WebSockets.md
├── WebRTC.md
├── Load Balancing.md
├── Regionalization and Latency.md
├── Timeouts Retries and Backoff.md
└── Circuit Breakers.md
```
