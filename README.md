# openclaw-memory-manager

> A structured long-term memory management skill for OpenClaw Agents — automatically captures, organizes, retrieves, and prunes persistent memory across conversations.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)]()

---

## Overview

OpenClaw agents lose context between sessions by default. The **memory-manager** skill solves this by giving your agent a layered, self-maintaining long-term memory system. It automatically detects what's worth remembering, stores it in a structured `MEMORY.md` file, trims stale entries when the file grows too large, and reloads the relevant context at the start of every new session.

Designed for OpenClaw Agent environments, this skill requires no external database — everything lives as plain Markdown files in your workspace.

---

## Features

- **Automatic memory capture** — Triggers on corrections, expressed preferences, decisions, completed complex tasks, business insights, held tasks, and explicit "remember" requests
- **Hierarchical storage** — Six memory layers with different retention rules (permanent, capped, time-limited)
- **Correction tracking** — Detects 6 correction signals; promotes repeated corrections to permanent rules after 2 occurrences
- **Auto-trimming** — Keeps `MEMORY.md` under 5 KB using priority-based deletion with archival rather than hard deletion
- **Privacy isolation** — Strict separation between group-chat and private-chat memories
- **Daily logging** — Auto-generates `memory/YYYY-MM-DD.md` logs for historical reference
- **On-demand search** — `memory_search` tool for retrieving history, decisions, preferences, or date-specific entries

---

## Memory Architecture

```
MEMORY.md  (≤5KB, auto-loaded each session)
├── User Preferences         [permanent, never deleted]
├── Role Definition          [permanent, never deleted]
├── Business Experience      [capped at 15 items, oldest merged when exceeded]
├── Active Tasks             [temporary, removed on completion]
├── Correction Records       [capped at 10 items, promoted to rules when recurring]
└── Conversation Summary     [7-day TTL, then consolidated/archived]

memory/YYYY-MM-DD.md         ← Daily logs
memory/archive/              ← Archived older memories
.learnings/corrections.md    ← Full correction records with root-cause analysis
```

---

## Installation

**Via ClawhHub (recommended):**

```bash
clawhub install memory-manager
```

**Manual:**

Download the `.skill` file and extract it to your workspace's `skills/` directory:

```
workspace/
└── skills/
    └── memory-manager/
```

---

## Usage

Once installed, the skill runs automatically. At the start of each session the agent loads `MEMORY.md` and performs at least two `memory_search` calls to surface relevant context.

### Automatic triggers (no user action required)

| Trigger | Example |
|--------|---------|
| User correction | "That's wrong, it should be X" |
| Expressed preference | "I prefer concise answers" |
| Decision made | "We decided to use PostgreSQL" |
| Complex task completed | Multi-step implementation finished |
| Business insight | Discovered domain-specific rule |
| Task put on hold | "Let's pause this for now" |
| Explicit request | "Remember that…" |

### Manual search

Ask the agent to search memory directly:

```
What do you remember about our database decisions?
Search memory for my preferences on code style.
```

---

## Configuration

Advanced parameters can be adjusted via `references/advanced-config.md`:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `MEMORY_MAX_SIZE` | 5 KB | Maximum size of `MEMORY.md` before trimming |
| `MAX_EXPERIENCES` | 15 | Max business experience entries |
| `MAX_CORRECTIONS` | 10 | Max correction records |
| `SUMMARY_TTL_DAYS` | 7 | Days before conversation summaries are archived |
| `CORRECTION_PROMOTE_COUNT` | 2 | Occurrences before a correction becomes a permanent rule |

---

## Auto-Trimming Priority

When `MEMORY.md` exceeds the size limit, entries are pruned in this order:

1. Completed tasks
2. Old conversation summaries
3. Promoted (already-archived) correction records
4. Merged business experiences (oldest first)
5. Archived to `memory/archive/` (not permanently deleted)

---

## Integration with Other OpenClaw Skills

| Skill | Interaction |
|-------|-------------|
| **Self-Improving / Proactive Self-Improving** | Captures errors; memory-manager stores the learnings |
| **Lossless-Claw (LCM)** | LCM handles conversation-level persistence (SQLite); memory-manager handles knowledge-level persistence |
| **High Agency / PUA** | Ensures corrections are recorded and persisted across sessions |

---

## Requirements

- OpenClaw Agent framework
- A writable workspace directory for `MEMORY.md` and `memory/` folder

No external dependencies or databases required.

---

## License

MIT — see [LICENSE](LICENSE) for details.
