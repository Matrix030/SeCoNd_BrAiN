# HLD Notes — Writing Style Recognition Guide

Companion to `CLAUDE.md`. While `CLAUDE.md` tells agents **how to structure** a note (frontmatter, splitting, wikilinks, Related), this file tells agents **how to recognize the shape** of Rishi's raw text so the right markdown gets applied without rewriting any words.

> [!warning] Core rule still applies
> Agents do **not** rewrite, paraphrase, summarize, or "improve" the words. They detect intent and apply the right markdown wrapper. If the raw text doesn't match a pattern below, leave it as prose — never invent structure.

---

## 1. Title Style

The user's titles fall into two shapes:

- **Plain topic**: `# Load Balancing`, `# Cache Eviction Policies`, `# What is Sharding?`
- **Topic + tagline (colon form)**: `# TCP: Reliable but with Overhead`, `# UDP: Fast but Unreliable`

If the raw text opens with a punchy one-liner that describes the topic ("UDP is the machinegun of protocols"), the colon-tagline form is appropriate. Otherwise use the plain topic. **Never invent a tagline** — only promote one if the user already wrote it.

---

## 2. Heading Hierarchy

Within a standalone note (post-split), use this ladder:

| Level | Use for |
|---|---|
| `#` | Note title (exactly one per file) |
| `##` | Major sections (e.g., `## Cache-Aside`, `## CDN`) |
| `###` | Sub-sections within a major section (e.g., `### When to Choose Consistency`) |
| `####` | "Key Characteristics" / sub-topic blocks (e.g., `#### Client-Side Load Balancing`) |
| `#####` | Examples or deeper sub-topics (e.g., `##### Example: Redis Cluster`) |
| `######` | Reserved for nested "Where to Use It" or similar leaves |

When promoting a split section from a monolith: bump every heading in that section up by the same delta so `##` becomes `#`, `###` becomes `##`, etc. **Relative depth stays the same.**

---

## 3. List Conventions — Preserve What the User Wrote

The user uses three list styles and they are NOT interchangeable:

### `1)` `2)` `3)` — sequential prose points
Used for **explaining a concept across several connected sentences**. Example:
```
1) TCP is the workhorse of the internet.
2) It provides reliable, ordered, and error-checked delivery.
3) It establishes a connection through a three-way handshake.
```

### `1.` `2.` `3.` — formal numbered lists / steps
Used for **discrete steps or enumerated items**. Often appears under a "How it works:" intro:
```
How it works:
1. Application checks the cache.
2. If the data is there, return it.
3. If not, fetch from the database, store it in the cache, and return it.
```

### `-` bullet — parallel, non-sequential items
Used when items are **at the same level and order doesn't matter**:
```
- Configuration values
- Feature flags
- Small reference datasets
```

**Rule for agents**: if the raw text already uses one of these styles, preserve it exactly. Do not convert between styles. If the raw text is prose with no markers, leave it as prose — do not impose a list.

---

## 4. Callouts — Detection Patterns

The user wraps important asides in Obsidian callouts. Convert raw text to a callout only when the pattern is clearly recognizable.

| Callout | When to apply |
|---|---|
| `> [!info]` | Background context, side notes, parenthetical asides, "by the way" content. Triggers in raw text: "Note that...", "Interestingly...", explanations of related but tangential concepts. |
| `> [!tip]` | Interview advice, mental models, "the default answer is...", key takeaways. Triggers: "In an interview...", "In system design interviews...", "The safe default is...", "If you only remember one thing...". |
| `> [!warning]` | Common mistakes, gotchas, things that look right but aren't. Triggers: "A common mistake is...", "Don't...", "Be careful...", "The biggest mistake with X is...". |
| `> [!note]` | Clarifications, distinctions between similar terms. Use sparingly — `[!info]` covers most cases. |

**Do not invent callouts** for plain prose. Only convert when the raw text contains one of the trigger phrases above, OR the user has explicitly written `Tip:`, `Note:`, `Warning:`, etc. inline.

Callouts can carry a short title after the type: `> [!tip] Default answer` or `> [!info] About this section`.

---

## 5. Bold Conventions

The user bolds aggressively but with intent:

- **Bold the first phrase of a bullet** when comparing options:
  ```
  - **Connection-oriented**: Establishes a dedicated connection before data transfer
  - **Reliable delivery**: Guarantees that data arrives in order and without errors
  ```
- **Bold key terms inline** on first meaningful use: `**stateful connection**`, `**spray and pray**`, `**shard key**`
- **Bold the punchline** of a decision sentence: "the most common pattern for scaling you'll see is **horizontal scaling**"
- **Bold question phrasings** the user wants to highlight: "**Do you prioritize consistency or availability when a network partition occurs?**"

If the raw text already uses `**...**`, preserve it. If the raw text uses other emphasis (CAPS, "quotes"), do not auto-convert to bold unless the term is being introduced as a definition.

---

## 6. Recurring Sub-Section Headings

When raw text has a section that fits one of these recurring shapes, use the exact heading name the user has used elsewhere (consistency across the vault matters):

- `## Key Characteristics` or `#### Key Characteristics of <X>`
- `## How it works` (often followed by a `1.` `2.` `3.` list)
- `## Where to Use It` / `### Where to Use It`
- `## When to Use` / `### When to Choose <X>`
- `## Real-World Examples` / `## Real-World Implementations`
- `## Common Mistakes` (often with `### <Mistake name>` sub-headings)
- `## Cheat Sheet` (usually contains a markdown table)
- `## Key Takeaways` (bullet list at the end, before `## Related`)
- `## Tradeoffs`
- `## Advanced <X> Considerations` (often introduced with a `> [!warning]` that this is senior-level content)
- `## Conclusion` (only if the user wrote a clear wrap-up paragraph — do not invent)

---

## 7. Tables

Use markdown tables when raw text compares **3+ items across the same dimensions** (e.g., metrics × components, options × tradeoffs). Pattern:

```md
| Component | Key Metrics | Scale Triggers |
|---|---|---|
| **Caching** | ~1ms latency | Hit rate < 80% |
```

- Bold the row label in the first column when it's a category/component.
- Use `<br>` inside cells for multi-line content (matches existing Numbers To Know style).
- Do not convert prose comparisons to tables — only convert if the raw text is already in a parallel-comparison shape.

---

## 8. Code Blocks

- Preserve language hints where obvious: ```` ```sql ````, ```` ```yaml ````, ```` ```json ````, ```` ```md ````.
- If the user wrote a SQL/JSON/code snippet without a fence, add the fence. Do not change the code itself.
- Never add wikilinks inside code blocks.

---

## 9. Image Embeds

Preserve `![[Pasted image YYYYMMDDHHMMSS.png]]` references **exactly where the user placed them** in the raw text. Images sit between content blocks — they're often the visual explanation of the paragraph immediately above. Do not move them around when splitting files.

Image embeds with a size suffix like `![[Pasted image ...png|646]]` keep the suffix.

---

## 10. Wikilinks — When to Add

Add wikilinks for:

- **First meaningful mention** of any concept that has its own note: `[[Redis]]`, `[[LRU]]`, `[[TTL]]`, `[[Sharding]]`, `[[CAP Theorem]]`.
- **Section anchors** when pointing into another note: `[[Load Balancing#Layer 4 Load Balancers|L4]]`.
- **Aliases** are fine: `[[LLM]]` works because the target note has it as an alias.

Do NOT:
- Wikilink every occurrence — first mention per section is enough.
- Wikilink inside code blocks, image embeds, or table headers.
- Wikilink generic words ("database", "system") unless they map to a specific concept note.

---

## 11. External Links

The user uses inline external links (`[Memcached](https://memcached.org/)`) for tools, AWS instance pages, and reference docs. Preserve any URL the user provides. **Do not invent URLs.**

---

## 12. Voice & Tone — How to Recognize It

The user's voice is:

- **Second person, direct**: "You'll default to HTTP over TCP", "Your app is taking off", "If you find yourself in an interview..."
- **Conversational with punchy analogies**: "the machinegun of protocols", "spray and pray", "Taylor Swift's shard getting hammered"
- **Interview-framed**: many notes have a "in an interview, you might say..." or "interviewers want to see..." paragraph
- **Honest about tradeoffs**: nearly every concept has a "but..." or "the tradeoff is..." paragraph

Agents should **never rewrite to match this voice** — they should only recognize when a chunk of raw text is already in this voice and apply structure (callouts, bolds, lists) appropriately.

---

## 13. The `## Related` Section

Every standalone note (not MOC) ends with `## Related`. Conventions:

- 3–5 links, never more.
- **First link is always back to the parent MOC**: `[[> Caching]] — back to the Caching MOC`
- Each link has an em-dash followed by a **one-line description** (lowercase start, no period).
- Order: MOC → most closely related → broader context.

Example:
```md
## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[UDP]] — the fast, unreliable alternative
- [[Transport Layer Protocols]] — when to choose TCP vs UDP
- [[WebSockets]] — persistent TCP connections for bidirectional comms
```

---

## 14. The MOC Note Style (`> Topic.md`)

The MOC is an index, never a content note. Pattern:

```md
---
tags: [system-design, hld, <topic>, moc]
aliases: ["Topic"]
---

# Topic — Map of Content

> [!info] About this section
> One-line purpose statement.

---

## <Group 1>
- [[Sub-note A]] — one-line description
- [[Sub-note B]] — one-line description

## <Group 2>
- [[Sub-note C]] — one-line description

> [!tip] Interview mental model
> One-line takeaway or decision heuristic.
```

Group sub-notes by sub-topic ("Fundamentals", "How to Shard", "Challenges", "Reference"), not by file order. Each sub-note gets one em-dash description of what's inside.

---

## 15. What Agents MUST NOT Do (style edition)

In addition to the structural rules in `CLAUDE.md`:

- **Do not invent callouts** from plain prose. Only wrap when trigger phrases are present.
- **Do not invent taglines** in titles. Only use colon-form if the user wrote a tagline.
- **Do not convert list styles** between `1)`, `1.`, and `-`. Preserve what the user wrote.
- **Do not add transitional sentences** ("Let's dive into...", "In summary...", "Now we'll cover..."). The user writes these or doesn't.
- **Do not summarize sections** at the top or bottom unless the user wrote a summary.
- **Do not add example code** the user didn't write.
- **Do not move image embeds** away from the paragraph they accompany.
- **Do not auto-bold** terms the user didn't bold, unless wrapping the first-phrase-of-bullet pattern in a parallel-comparison list.
- **Do not add "Conclusion" sections** unless the raw text has a clear closing paragraph.

---

## 16. Quick Decision Tree for Raw Text Chunks

When an agent encounters a chunk of raw text:

1. **Is it a heading-shaped phrase?** (short, no verb, capitalized) → make it a heading at the appropriate level.
2. **Is it a sequence of connected statements explaining one idea?** → wrap in `1)` `2)` `3)` if the user already started one; otherwise leave as prose.
3. **Is it discrete steps?** → `1.` `2.` `3.` numbered list.
4. **Is it parallel items at the same level?** → `-` bullets.
5. **Does it start with a callout trigger phrase** (see §4)? → wrap in matching callout.
6. **Is it a comparison across 3+ items × dimensions?** → markdown table.
7. **Is it code, config, or schema?** → fenced code block with language hint.
8. **Is it an inline term being introduced?** → `**bold**` it.
9. **Is it a concept that has its own note in the vault?** → wikilink the first mention.
10. **None of the above?** → leave as plain paragraph prose.

---

## 17. Reference Example

The gold-standard end-to-end examples in this vault:

- `HLD/Core Concepts/Networking/` — full split with MOC + 14 sub-notes
- `HLD/Core Concepts/Caching/` — clean split with tip-heavy callouts
- `HLD/Core Concepts/Numbers To Know/Numbers To Know.md` — table-heavy reference style
- `HLD/Core Concepts/CAP Theorem/CAP Theorem.md` — long single-file note with nested headings, callouts, and an "Advanced Considerations" section

When in doubt, open one of these and match the shape.
