# examples/ — Multi-agent Support

The Agentic Wiki is agent-agnostic at the core — the wiki format, page layout, and workflows are defined as markdown and Obsidian wikilinks, readable by any LLM agent. What differs between agents is: schema file naming, slash-command support, and cross-session persistence mechanism.

This folder ships variants for several agent ecosystems. Pick the one that matches your toolchain.

## Quick pick

| Your agent | Use this |
|------------|----------|
| **Claude Code** (reference implementation) | [`for-claude-code/`](for-claude-code/) |
| **OpenCode** | [`for-opencode/`](for-opencode/) + [`AGENTS.md`](AGENTS.md) at vault root |
| **Cursor** | [`for-cursor/`](for-cursor/) or [`AGENTS.md`](AGENTS.md) at vault root |
| **Codex, Aider, goose, Zed, Warp, Gemini CLI, Jules, Factory, others** | [`AGENTS.md`](AGENTS.md) at vault root |

## Feature comparison

| Feature | Claude Code | OpenCode | Cursor | AGENTS.md (generic) |
|---------|:-----------:|:--------:|:------:|:-------------------:|
| Schema / wiki format | ✅ | ✅ | ✅ | ✅ |
| Slash commands | ✅ skills | ✅ commands | ⚠️ manual | ❌ |
| Cross-session auto-load (any directory) | ✅ global config | ❌ per-project | ❌ per-project | ❌ per-project |
| Autonomous capture (outside vault) | ✅ | ❌ | ❌ | ❌ |
| Proactive skill identification | ✅ | ❌ | ❌ | ❌ |
| Maintenance principles | ✅ | ✅ | ✅ | ✅ |
| Agent personality / user profile | ✅ | ⚠️ manual | ⚠️ manual | ⚠️ manual |

**Legend:**
- ✅ Supported natively
- ⚠️ Works but requires manual setup or in-chat invocation
- ❌ Not supported by the agent's current design

## Why Claude Code is the reference

Claude Code has a global config mechanism (`~/.claude/CLAUDE.md`) that loads at the start of every session regardless of working directory. This is what powers autonomous capture — the wiki grows from work you do in unrelated projects, not just when you're inside the vault. No other agent ships this primitive out of the box. The Claude Code variant is therefore the **reference** — the most complete expression of the Agentic Wiki pattern.

That doesn't mean other agents are crippled. The schema, page format, workflows, and maintenance principles all port cleanly via `AGENTS.md`. What you lose without Claude Code is the invisible cross-session maintenance. You can reproduce it manually by always launching the agent from inside the vault directory.

## Folder contents

- [`AGENTS.md`](AGENTS.md) — Universal schema. Place at vault root. Works with Codex, OpenCode, Cursor, Aider, goose, Zed, Warp, Gemini CLI, Jules, Factory, and ~15 other agents supporting the `AGENTS.md` convention.
- [`wiki-template/`](wiki-template/) — Blank wiki vault structure (agent-agnostic). Copy this to bootstrap a new vault, then add the schema file your agent needs.
- [`for-claude-code/`](for-claude-code/) — Full Claude Code kit: global config, vault schema, 5 slash commands as `SKILL.md` directories.
- [`for-opencode/`](for-opencode/) — 5 slash commands in OpenCode format + setup README. Pair with `AGENTS.md` at vault root.
- [`for-cursor/`](for-cursor/) — Cursor project rule (`.cursor/rules/agentic-wiki.mdc`) with always-apply flag + setup README.

## Contributing an agent variant

If you use an agent not listed here and want to contribute a variant:
- Start from `AGENTS.md` as the baseline (it works everywhere)
- Add an `examples/for-<agent>/` folder with the agent's command/skill format
- Document limitations honestly in a `README.md` inside the folder
- Open a PR
