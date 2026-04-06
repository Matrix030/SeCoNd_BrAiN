# SeCoNd_BrAiN — Obsidian Vault

This is Rishi's personal Obsidian knowledge vault. It covers AI/ML, software engineering, security, cloud, and dev fundamentals — primarily from courses, books, and hands-on projects.

---

## Vault Structure

```
SeCoNd_BrAiN/
├── 🏠 Home.md                        ← Start here — master dashboard linking everything
├── AI Agent Course/                  ← HuggingFace AI Agents course (Units 1–2)
├── Machine_Learning/
│   ├── Udemy_ML/                     ← Udemy ML course (regression, classification)
│   ├── Coursera/                     ← Andrew Ng Coursera ML (supervised learning, NNs)
│   └── Google_Machine_Learning/      ← Google ML Crash Course
├── Algorithms/                       ← DSA notes (has its own CLAUDE.md with rules)
├── System Design/                    ← LLD: OOP, design principles, patterns, problems
├── Books/
│   ├── Machine Learning/             ← Hands-On ML with Scikit-Learn (Géron)
│   ├── Essential Scrum/              ← Essential Scrum (Rubin)
│   └── Programming Massively Parallel Processors/  ← CUDA/GPU programming
├── OWASP Top 10/                     ← Web security vulnerabilities
├── dev_basics/
│   ├── GOLANG/                       ← Go language theory + HTTP projects
│   ├── C for Memory Management/      ← Pointers, stack/heap, GC
│   └── Linux/                        ← File systems, permissions, I/O, shells
└── Coursera/
    └── AWS/                          ← AWS Fundamentals (EC2, IAM, S3, networking)
```

---

## Navigation Conventions

### MOC / Index Notes (`> ` prefix)
Notes prefixed with `> ` (e.g. `> Introduction to Agents.md`) are **Maps of Content** — index notes that link to all sub-topics within that section. They sort to the top of the file explorer. Always start with these when entering a new topic.

**Key entry points:**

| Section | MOC Note |
|---------|----------|
| Full vault | `🏠 Home.md` |
| AI Agent Course | `AI Agent Course/> AI Agent Course (Main Unit Connector).md` |
| Unit 1 — Agents | `AI Agent Course/Unit 1.../> Introduction to Agents (Unit 1 - Connector).md` |
| Unit 2 — smolagents | `AI Agent Course/Unit 2.../> Unit 2 - Connector.md` |
| Machine Learning (Udemy) | `Machine_Learning/Udemy_ML/Udemy Machine Learning.md` |
| System Design (LLD) | `System Design/> LLD Delivery Framework.md` |
| Go / Golang | `dev_basics/GOLANG/> GOLANG CONNECTOR.md` |
| OWASP Top 10 | `OWASP Top 10/> OWASP TOP 10.md` |
| AWS | `Coursera/AWS/> AWS.md` |
| Algorithms | `Algorithms/` (see `Algorithms/CLAUDE.md`) |

### Sub-topic Notes
Regular content notes live inside topic folders alongside or under their MOC. File names match the topic title.

---

## Obsidian Features in Use

### Frontmatter (YAML)
Every note has frontmatter with at least `tags`, and `aliases` when the concept has multiple common names:
```yaml
---
tags: [ai-agents, course, unit-1, llm]
aliases: ["Large Language Model", "LLM"]
---
```

### Tag Taxonomy
Tags are derived from the folder hierarchy and topic content. Core tags:

| Tag | Covers |
|-----|--------|
| `ai-agents` | All AI Agent Course notes |
| `ml` | All machine learning notes |
| `algorithms` / `dsa` | Algorithms & data structures |
| `system-design` / `lld` | Low-level design, OOP, patterns |
| `golang` | Go language notes |
| `security` / `owasp` | Security notes |
| `aws` / `cloud` | AWS / cloud notes |
| `book` | Book notes |
| `course` | Course notes |
| `moc` | Map of Content / index notes |
| `regression` | Regression algorithms |
| `neural-network` | Neural network notes |
| `smolagents` | smolagents framework |
| `react-pattern` | ReAct reasoning pattern |

### Wikilinks
Use `[[Note Name]]` to link between notes. Aliases mean you can link via any known name:
- `[[LLM]]` resolves to the "What is a Large Language Model?" note
- `[[Unit 1]]` resolves to the Unit 1 connector

### Callouts
Used in MOC and content notes to highlight key information:
```md
> [!info] Context or background
> [!tip] Practical advice or mental model
> [!warning] Common pitfall or gotcha
> [!note] Side note or clarification
```

### Embeds
`![[Note Name]]` embeds another note's content inline — used to avoid duplication.

---

## Note Structure Templates

### Content Note (general)
```md
---
tags: [relevant, tags]
aliases: ["Alternate Name"]
---

# Topic Title

One-liner summary.

---

## Key Points
- Concise bullets

## How It Works
- Step-by-step explanation
- Use callouts for tips/warnings

## Related
- [[Link to related note]]
```

### Algorithm Note (see `Algorithms/CLAUDE.md` for full rules)
```md
---
tags: [algorithms, dsa, sorting]
aliases: ["Alternate Name"]
---

# Algorithm Name

One-liner.

## Complexity
| | Best | Average | Worst |
|---|---|---|---|
| Time | O(...) | O(...) | O(...) |
| Space | | | O(...) |

## Key Points / How It Works
## When to Use
## Related
```

### MOC / Index Note
```md
---
tags: [relevant-tags, moc]
aliases: ["Short Name"]
---

# Section Title — Map of Content

> [!info] Goal or purpose of this section

---

## Sub-topics
[[> Sub-topic 1]] — one-line description
[[> Sub-topic 2]] — one-line description

> [!tip] Mental model or key takeaway
```

---

## Rules for Agents

- **Read the MOC note first** before diving into sub-notes in any section
- **Do not overwrite existing frontmatter** — only add to it if a field is missing
- **Preserve the `> ` prefix** on index/MOC files — it controls sort order in the explorer
- **Match writing style** — notes are concise and direct, not copy-pasted walls of text
- **Link liberally** — every note should have at least one `[[wikilink]]` to a related concept
- **Do not touch** `Algorithms/` without reading `Algorithms/CLAUDE.md` first — it has its own strict rules
- **Do not modify** System Design notes that already have complete frontmatter, callouts, and structure — they are the gold standard for this vault
- **Scope changes to the relevant folder** — don't reorganize across top-level sections unless explicitly asked
- **Never co-author git commits** (no `Co-Authored-By` lines)

---

## What "Good" Looks Like

The **System Design** section is the best-organized part of this vault. When in doubt, look at a note like `System Design/> LLD OOP Concepts.md` as the reference for formatting, frontmatter quality, callout usage, and wikilink density.
