---
name: advanced-memory
description: Persistent memory protocol across sessions. Use at the start of every task (recall), whenever the user states a preference, decision, configuration, architecture, solved bug or next step (save), when something saved changes (update), and when the user says "remember", "recuerda", "olvida" or asks what you know about a project.
---

# Advanced Memory

Backend: the hosted Claude Code memory MCP (`mcp__memory__memory_list/read/write/str_replace/append/delete`). It persists across sessions and containers. Do NOT create memory files inside repositories and never write a MEMORY.md index (the server keeps it).

## Categories (file-name prefix → `type`)
| Category | File prefix | type |
|---|---|---|
| Active projects | `proj-active-<repo>.md` | project |
| Finished projects | `proj-done-<repo>.md` | project |
| Coding preferences | `pref-<topic>.md` | feedback / user |
| Architectures | `arch-<repo>.md` | project |
| Technical config | `config-<topic>.md` | reference |
| Solved errors | `bugfix-<topic>.md` | project |
| Important decisions | `decision-<topic>.md` | project |
| Next tasks | `todo-<repo>.md` | project |

Frontmatter: `name` (kebab slug), `description` (one line), `type`, `scope` (repo|person). Body: the fact; for project/feedback add **Why:** and **How to apply:**. Link related notes with `[[slug]]`. Use absolute dates (YYYY-MM-DD).

## Protocol
1. **Recall (start of task):** if an index block is in context, read only the notes that bear on the task; otherwise `memory_list include_preview=true` once, then `memory_read` relevant notes (batch up to 20 paths in one call).
2. **Verify:** a note is a snapshot. Check files/flags it names still exist before acting on it. If stale, update the note.
3. **Save only what is non-obvious and durable:** not what git/code/README already records, not one-off chat details.
4. **Deduplicate:** before `memory_write`, check for an existing note on the same topic and `memory_str_replace`/`memory_append` it instead.
5. **Update on change:** moving a project from active to done = new `proj-done-` note + mark the active one, or edit in place.
6. **Forget:** only on the user's typed request; remove exactly what they named.

## Never store
Passwords, API keys, tokens, connection strings, `.env` contents, personal IDs, payment data. Store *where* config lives conceptually (e.g. "Supabase keys are Railway env vars"), never values.

## Limits
Memory is per repository folder (the one linked to the session). Notes about another repo go in that repo's session or are scoped `person`.
