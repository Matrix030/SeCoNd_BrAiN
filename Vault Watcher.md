---
tags: [project, automation, obsidian, ollama, python]
aliases: [Vault Watcher, Obsidian Auto-Formatter]
---

# Vault Watcher

A background Python script that watches the Obsidian vault for file changes and automatically formats new/edited notes using a local LLM via Ollama.

---

## Goal

Seamless note formatting — write a note, save it, and within a few seconds it comes back properly structured with frontmatter, callouts, and wikilinks. No manual Claude Code invocation needed.

---

## Setup

| Component | Location |
|---|---|
| Vault | this machine — `/home/rgmatr1x/Documents/SeCoNd_BrAiN` |
| Ollama + model | home computer (RTX 4080 Super, 14GB VRAM loaded) |
| Script | this machine — `~/scripts/vault_watcher.py` |
| Model | `gemma4:26b` (verify exact tag with `ollama list` on home machine) |

> [!warning] Ollama must accept remote connections
> By default Ollama only listens on `127.0.0.1`. On the home machine, start Ollama with:
> `OLLAMA_HOST=0.0.0.0 ollama serve`
> Or set `Environment=OLLAMA_HOST=0.0.0.0` in the systemd service file.

---

## Architecture

```
[vault file changes]
        ↓
  watchdog (Python) — monitors vault recursively
        ↓
  debounce 4s — waits for user to stop typing
        ↓
  build vault index — all note names + tags + first line
        ↓
  Ollama API call → gemma4:26b on home machine
  prompt = CLAUDE.md + vault index + changed file content
        ↓
  formatted markdown returned
        ↓
  write back to file (mark as script-written to avoid re-trigger loop)
```

---

## What the LLM Does Per Note

1. Add/fix YAML frontmatter (`tags`, `aliases`)
2. Ensure `# Title` heading exists
3. Fix broken callout syntax (all content lines need `> ` prefix)
4. Add `[[wikilinks]]` inline where relevant vault notes exist
5. Add `## Related` section at the bottom with top relevant links
6. Preserve all original content — no summarizing or removal

---

## Key Implementation Details

### File Watcher
- Library: `watchdog` (`pip install watchdog`)
- Watch path: `VAULT_PATH` recursively
- Trigger on: `on_modified` and `on_created` events

### Debounce
- 4 second timer — resets on each new event for the same file
- Prevents processing while the user is mid-sentence

### Loop Prevention
- After writing a formatted file, add its path to `SCRIPT_WRITTEN` set
- On next event for that path, skip processing and remove from set

### Files to Ignore
```python
IGNORE_DIRS = {'.obsidian', '.git', '.claude'}
IGNORE_FILES = {'CLAUDE.md', 'MEMORY.md'}
```

### Vault Index (for wikilink context)
- Rebuild on every processing call (vault is small enough)
- Format: `[[Note Name]] [tag1, tag2] — first content line`
- Cap at ~150 notes to keep prompt size reasonable
- Exclude the file currently being formatted

### Ollama Call
```python
POST http://<HOME_IP>:11434/api/generate
{
  "model": "gemma4:26b",
  "prompt": "...",
  "stream": false,
  "options": { "temperature": 0.1, "num_ctx": 8192 }
}
```
- `temperature: 0.1` — deterministic formatting, not creative
- `num_ctx: 8192` — enough for style guide + vault index + note

---

## Files to Create

```
~/scripts/
├── vault_watcher.py      ← main script
└── requirements.txt      ← watchdog, requests

~/.config/systemd/user/
└── vault-watcher.service ← auto-start on login
```

### `requirements.txt`
```
watchdog
requests
```

### Systemd Service (auto-start on login)
```ini
[Unit]
Description=Obsidian Vault Watcher
After=network.target

[Service]
ExecStart=/usr/bin/python3 /home/rgmatr1x/scripts/vault_watcher.py
Restart=on-failure
RestartSec=5

[Install]
WantedBy=default.target
```

Enable with:
```bash
systemctl --user enable vault-watcher
systemctl --user start vault-watcher
```

---

## Open Questions

- [ ] What is the exact Ollama model tag? Run `ollama list` on the home machine.
- [ ] What is the home machine's local IP? Run `hostname -I` on the home machine.
- [ ] Should notes with existing frontmatter be re-processed when content is added? (Current plan: yes — the LLM returns unchanged content if nothing needs fixing, so no write happens.)
- [ ] Context window size — if the vault grows large, the index may need to be trimmed or summarized.

---

## Future: Hierarchical Session Architecture

> [!note] Shelved idea — do not implement in v1
> The vault watcher formats individual notes. The idea below is a separate, more ambitious system.

Each directory could have its own LLM session with focused context on just that subtopic. Sessions are summarized bottom-up into a `home.md` per directory, which the parent session uses as context for its own `home.md`. This recurses all the way up to the root `Home.md`.

```
leaf notes → dir/home.md → parent/home.md → ... → Home.md
```

**Key constraint:** the existing `> ` prefix MOC notes (e.g. `> LLD OOP Concepts.md`) already serve this purpose and are manually maintained. The auto-generated `home.md` layer would be a parallel system for LLM context only — not a replacement for MOC notes.

**Trigger cascade when a note changes:**
```
note.md edited
→ re-generate its directory's home.md
→ re-generate parent directory's home.md
→ ... up to root Home.md
```

---

## Related

- [[Home]]
