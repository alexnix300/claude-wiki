# Agentic Wiki — OpenCode Variant

[OpenCode](https://opencode.ai) variant of the Agentic Wiki skills.

## Installation

1. **Set up the wiki vault** using the universal schema. Place `../AGENTS.md` at the root of your wiki vault directory:
   ```
   cp ../AGENTS.md ~/Documents/WIKI/AGENTS.md
   ```

2. **Install the slash commands** globally so they're available in every OpenCode session:
   ```
   mkdir -p ~/.config/opencode/commands
   cp commands/*.md ~/.config/opencode/commands/
   ```

3. Launch `opencode` from inside your wiki vault. The commands below are now available.

## Available commands

| Command | Purpose |
|---------|---------|
| `/ingest <file>` | Process a source document into the wiki |
| `/wiki <question>` | Search the wiki and synthesize an answer with citations |
| `/wiki-status` | Dashboard: page counts, recent activity, pending sources |
| `/lint-wiki` | Health check with suggestions for orphans, contradictions, stale pages |
| `/wiki-save <info>` | Save a piece of information to the appropriate category |

OpenCode commands use `$ARGUMENTS`, `$1`, `$2` for argument binding.

## Known limitations vs. Claude Code reference implementation

- **No automatic cross-session persistence.** OpenCode reads `AGENTS.md` when working inside a project directory. If you open OpenCode from an unrelated directory (e.g. a different codebase), the wiki context does not load automatically. Workaround: place a minimal `AGENTS.md` in other projects that references your wiki vault, or always launch OpenCode from inside the wiki.
- **No session hooks.** The "autonomous capture" behavior (agent files decisions from unrelated work into the wiki) depends on always being in the vault directory. OpenCode has no equivalent to Claude Code's `~/.claude/CLAUDE.md` that loads regardless of working directory.
- **No proactive skill identification.** Works when the user prompts; no automatic pattern recognition across sessions.

For full feature parity, use the `for-claude-code/` variant. For portability across the ~15 agents supporting AGENTS.md, this OpenCode setup + the universal schema is sufficient.

## Docs

- Universal schema: [`../AGENTS.md`](../AGENTS.md)
- Design decisions and workflows: [`../../docs/`](../../docs/)
- Main README with comparison matrix: [`../README.md`](../README.md)
