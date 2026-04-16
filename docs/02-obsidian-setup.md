---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, obsidian, research]
---

# Step 2 — Obsidian Vault & Directory Structure

## Context

Obsidian is the "IDE" for browsing the wiki. The LLM is the "programmer." The wiki files are the "codebase." You view the results in Obsidian while the LLM makes edits via the terminal.

## Prerequisites

- Obsidian installed ([obsidian.md](https://obsidian.md))
- A parent directory chosen (e.g. `~/Documents`)

## Process

### 2.1 — Create the vault

In Obsidian: Open another vault → Create new vault → choose a name (e.g. `WIKI`) and location (e.g. `~/Documents`).

This creates a folder with an `.obsidian/` config directory inside.

Alternatively, create the folder manually and open it as a vault:

```bash
mkdir -p ~/Documents/WIKI
```

### 2.2 — Create the directory structure

The agent creates this structure. If setting up manually:

```bash
cd ~/Documents/WIKI
mkdir -p raw raw/assets
mkdir -p wiki/{sources,entities,concepts,topics,analyses,projects,personal,research,meta}
```

**Directory purposes:**

| Directory | Purpose | Who writes |
|-----------|---------|------------|
| `raw/` | Immutable source documents (articles, PDFs, transcripts) | Human |
| `raw/assets/` | Downloaded images and attachments | Human / Obsidian |
| `wiki/sources/` | One summary page per ingested source | LLM |
| `wiki/entities/` | People, orgs, clients, systems | LLM |
| `wiki/concepts/` | Ideas, frameworks, patterns | LLM |
| `wiki/topics/` | Thematic synthesis across sources | LLM |
| `wiki/analyses/` | Filed query results, investigations | LLM |
| `wiki/projects/` | Project pages (personal and client) | LLM |
| `wiki/personal/` | Routines, goals, habits, life context | LLM |
| `wiki/research/` | Tech exploration, learning notes | LLM |
| `wiki/meta/` | Setup docs, instructions, this guide | LLM / Human |

### 2.3 — Initialize git

```bash
cd ~/Documents/WIKI
git init
```

Create `.gitignore`:

```
.DS_Store
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
```

### 2.4 — Create index and log

Two special files the LLM uses for navigation:

**`wiki/index.md`** — master catalog of all pages, organized by type. The LLM reads this first when answering queries.

**`wiki/log.md`** — append-only chronological record of operations. Each entry prefixed with date and operation type for parseability.

## Comment — Why Obsidian

Obsidian is ideal because:
- It's just a folder of markdown files — no lock-in, no database
- `[[wikilinks]]` provide native cross-referencing
- Graph view shows the shape of your knowledge base visually
- It's local-first — the LLM reads/writes files directly on disk
- Rich plugin ecosystem (Dataview, Marp, Web Clipper, etc.)
- Free for personal use

Any markdown editor works, but you lose the graph view and wikilink navigation. Obsidian is the best viewer for this pattern.

## Prompt used

After pasting the seed prompt (Step 1), the agent asked:

> "What's the primary use case for your wiki?"
> Options: Personal, Research, Reading, Business/work

> "Can you briefly describe the specific topic or domain?"
> Options: I'll describe it, General purpose

> "Should I initialize a git repo for version history?"
> Options: Yes (Recommended), No

Answers given: Work/projects/tasks/knowledgebase, General purpose, Yes.

The agent then created the directory structure, git repo, and initial files automatically.
