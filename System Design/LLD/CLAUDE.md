# System Design - Obsidian Vault

This is an Obsidian vault dedicated to system design learning notes.

## Rules

- All notes are written in Markdown (`.md`) files inside this directory
- Notes should be **summarized and concise** — not copy-pasted walls of text
- Never co-author any git commits
- Stay within the `System Design/` directory only — do not touch other vault folders
- When the user says they made changes to a note, read the updated file, understand their writing style and structural changes, and adopt that style for all future notes

## Obsidian Features — Use Them Well

- **Wikilinks**: Use `[[Note Name]]` to link related concepts. Every note should link out to relevant topics so the graph view stays rich and navigable.
- **Tags**: Use inline tags (e.g. `#scaling`, `#database`, `#caching`, `#tradeoff`) to categorize concepts. Place them near the top of the note or inline where relevant.
- **Aliases**: When a concept has multiple names (e.g. "Consistent Hashing" / "Hash Ring"), add aliases in YAML frontmatter so wikilinks work regardless of which name is used:
  ```yaml
  ---
  aliases: [Hash Ring]
  ---
  ```
- **Callouts**: Use Obsidian callouts to highlight important trade-offs, warnings, or tips:
  ```md
  > [!tip] Why this matters
  > Brief explanation

  > [!warning] Trade-off
  > What you give up
  ```
- **Embeds**: Use `![[Note Name]]` to embed one note's content inside another when it avoids duplication.
- **Frontmatter**: Every note should have YAML frontmatter with at least `tags` and optionally `aliases`.

## Note Structure

When writing a new note, follow this format:

```md
---
tags: [relevant, tags]
aliases: [alternate names if any]
---

# Topic Title

Brief one-liner summary of the concept.

---

## Key Points
- Concise bullet points

## Details
- Explanations, trade-offs, diagrams (if needed)
- Use callouts for important trade-offs or tips

## Related
- [[Link to related notes]]
```

## Conventions

- Entry/root notes for a topic group are prefixed with `> ` (e.g. `> LLD Delivery Framework.md`) so they sort to the top in the file explorer
- Sub-topic file names should match the topic title (e.g. `Load Balancing.md`)
- Group subtopics using folders if a topic grows large (e.g. `Databases/`)
- Keep each note focused on one concept — link out to related ones instead of repeating content
- Prefer linking and embedding over duplicating content across notes
