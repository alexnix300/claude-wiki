# Agentic Wiki — Claude Code Variant (Reference Implementation)

Full feature parity with the Claude Wiki design. This is the **reference implementation** — everything in the open-source repo targets Claude Code first. The other `for-<agent>/` variants are translations, with known feature gaps documented in each.

## Installation

Easiest: paste the setup prompt (`../../setup-prompt.md`) into Claude Code. It asks a few questions, creates the vault, installs the global config, and installs all 5 skills. Skip the manual steps below.

For manual setup:

1. **Create the vault** by copying the blank template:
   ```
   cp -R ../wiki-template ~/Documents/WIKI
   ```

2. **Install the schema** at the vault root:
   ```
   cp vault-claude-md.md ~/Documents/WIKI/CLAUDE.md
   ```

3. **Install the global config** so the wiki is active in every session:
   ```
   cat global-claude-md.md >> ~/.claude/CLAUDE.md
   ```

4. **Install the slash commands:**
   ```
   cp -R skills/* ~/.claude/skills/
   ```

## What's in here

| File | Purpose |
|------|---------|
| `global-claude-md.md` | Example `~/.claude/CLAUDE.md` — tells Claude Code to read the wiki every session, capture context autonomously, and apply maintenance principles |
| `vault-claude-md.md` | Example `CLAUDE.md` at the vault root — schema, conventions, page format, workflows |
| `skills/ingest/SKILL.md` | `/ingest` — process a source into the wiki |
| `skills/wiki/SKILL.md` | `/wiki <question>` — search wiki and synthesize an answer |
| `skills/wiki-status/SKILL.md` | `/wiki-status` — dashboard of wiki state |
| `skills/lint-wiki/SKILL.md` | `/lint-wiki` — health check |
| `skills/wiki-save/SKILL.md` | `/wiki-save <info>` — save info to the right category |

## Features available only here

Compared to the other variants, Claude Code uniquely supports:

- **Cross-session auto-load from any directory.** `~/.claude/CLAUDE.md` loads at every session start regardless of working directory. Work in unrelated projects can feed the wiki.
- **Autonomous capture.** The agent captures context during normal work without explicit invocation.
- **Proactive skill identification.** The agent watches for recurring workflows and suggests new slash commands.
- **Full personalization trio.** Tools & preferences, agent-personality, user-profile — all three persistent memory layers.

## Docs

- Full design documentation: [`../../docs/`](../../docs/)
- Research paper: [`../../PAPER.md`](../../PAPER.md)
- Main examples README: [`../README.md`](../README.md)
