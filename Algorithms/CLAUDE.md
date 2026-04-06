# Algorithms - Obsidian Vault

This is an Obsidian vault dedicated to algorithms and data structures learning notes.

## Rules

- All notes are written in Markdown (`.md`) files inside this directory
- Notes should be **summarized and concise** — not copy-pasted walls of text
- Never co-author any git commits
- Stay within the `Algorithms/` directory only — do not touch other vault folders
- When the user says they made changes to a note, read the updated file, understand their writing style and structural changes, and adopt that style for all future notes

## Obsidian Features — Use Them Well

- **Wikilinks**: Use `[[Note Name]]` to link related concepts. Every note should link out to relevant topics so the graph view stays rich and navigable.
- **Tags**: Use inline tags (e.g. `#sorting`, `#graph`, `#dp`, `#greedy`, `#tradeoff`) to categorize concepts. Place them near the top of the note or inline where relevant.
- **Aliases**: When a concept has multiple names (e.g. "BFS" / "Breadth-First Search"), add aliases in YAML frontmatter so wikilinks work regardless of which name is used:
  ```yaml
  ---
  aliases: [Breadth-First Search]
  ---
  ```
- **Callouts**: Use Obsidian callouts to highlight important complexity, warnings, or tips:
  ```md
  > [!tip] When to use this
  > Brief explanation

  > [!warning] Watch out
  > Common pitfall or edge case

  > [!info] Complexity
  > Time: O(...) | Space: O(...)
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

# Algorithm/Topic Title

Brief one-liner summary of the concept.

---

## Complexity
| | Best | Average | Worst |
|---|---|---|---|
| Time | O(...) | O(...) | O(...) |
| Space | | | O(...) |

## Key Points
- Concise bullet points

## How It Works
- Step-by-step intuition (not pseudocode walls — keep it tight)
- Use callouts for complexity, tips, or pitfalls

## When to Use
- Conditions or problem patterns where this algorithm shines

## Related
- [[Link to related notes]]
```

## Conventions

- Entry/root notes for a topic group are prefixed with `> ` (e.g. `> Sorting Algorithms.md`) so they sort to the top in the file explorer
- Sub-topic file names should match the topic title (e.g. `Binary Search.md`, `Dijkstra.md`)
- Group subtopics using folders if a topic grows large (e.g. `Graphs/`, `Dynamic Programming/`)
- Keep each note focused on one algorithm or concept — link out to related ones instead of repeating content
- Prefer linking and embedding over duplicating content across notes
- For problems/patterns (e.g. Sliding Window, Two Pointers), follow the same note structure but skip the complexity table if not applicable
