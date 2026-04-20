# Agentic Wiki — Cursor Variant

[Cursor](https://cursor.com) variant using project-level rules.

## Installation

1. **Set up the wiki vault** as a Cursor project. Open the vault directory in Cursor.

2. **Copy the rule into the vault:**
   ```
   cp -R .cursor /path/to/your/wiki/vault/
   ```

   This places `.cursor/rules/agentic-wiki.mdc` at the root of your vault. Cursor reads it automatically when you open the project.

3. (Optional) Commit `.cursor/` if you share the vault across team or devices.

## What you get

The rule has `alwaysApply: true`, so it's active in every Cursor chat session in the vault project. It defines:

- The vault layout (directories, page types)
- Page format (YAML frontmatter + Obsidian `[[wikilinks]]`)
- Five core operations (ingest / query / lint / save / autonomous capture) triggered by natural-language intents
- Four maintenance principles (Read Before Edit, Minimal Page Surface, Surgical Updates, Verify After Edit)
- Index and log conventions

## How to invoke operations

Cursor does not have true slash commands. Use natural-language intents:

| Intent | Example phrasing |
|--------|------------------|
| Ingest | "Ingest this file" / "Process this article" |
| Query | "What do we know about X?" / "Search the wiki for Y" |
| Save | "Save this to the wiki" / "File this info" |
| Lint | "Lint the wiki" / "Health check the wiki" |
| Status | "Show me the wiki status" / "What's in the wiki right now?" |

Cursor will interpret these and follow the rule's workflow.

## Known limitations vs. Claude Code reference implementation

- **No true slash commands.** Natural-language intents work but require the user to phrase requests. Claude Code's `/ingest`, `/wiki`, etc. are more convenient.
- **No cross-session persistence outside the vault.** The rule applies when Cursor is working inside the vault project. If you open Cursor on an unrelated codebase, wiki context does not load. For cross-project behavior, configure user-level rules via Cursor Settings → Rules (UI only, not file-based — cannot be committed to this repo).
- **No autonomous-capture across all sessions.** Autonomous capture happens only when Cursor is working in the vault. Work in other projects doesn't feed the wiki without explicit action.
- **No proactive skill identification.**

For full feature parity, use the `for-claude-code/` variant.

## Docs

- Universal schema: [`../AGENTS.md`](../AGENTS.md) (Cursor also supports `AGENTS.md` as an alternative to `.cursor/rules/`)
- Design decisions: [`../../docs/`](../../docs/)
- Main README with comparison matrix: [`../README.md`](../README.md)
