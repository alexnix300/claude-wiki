# Claude Wiki

A personal knowledge base that builds itself. Paste one prompt into [Claude Code](https://claude.ai/claude-code), answer a few questions, and get a structured wiki that grows automatically with every session.

> **Academic paper:** [`PAPER.md`](PAPER.md) — *The Agentic Wiki: A Practitioner's Implementation of the LLM-Maintained Personal Knowledge Base.* Full writeup of the architecture, cross-session persistence, adaptive personalization, failure modes, and open research questions. Based on Andrej Karpathy's LLM Wiki methodology.

## What is this?

Most people's experience with LLMs and documents is **retrieval** — upload files, ask questions, get answers. The LLM rediscovers knowledge from scratch every time. Nothing accumulates.

Claude Wiki is different. The LLM **builds and maintains a persistent wiki** — a structured collection of interlinked markdown files. When you add a source, the LLM reads it, extracts what matters, and integrates it into the existing wiki: updating entity pages, revising summaries, flagging contradictions, strengthening the synthesis. The knowledge compounds.

You browse the wiki in [Obsidian](https://obsidian.md). The LLM does all the writing. You direct the work.

## Works with

Reference implementation targets Claude Code. Other supported agents via the `AGENTS.md` universal schema + agent-specific extensions:

| Agent | Variant | Slash commands | Cross-session auto-load |
|-------|---------|:-:|:-:|
| **Claude Code** (reference) | [`examples/for-claude-code/`](examples/for-claude-code/) | ✅ skills | ✅ global config |
| **OpenCode** | [`examples/for-opencode/`](examples/for-opencode/) | ✅ commands | ❌ per-project |
| **Cursor** | [`examples/for-cursor/`](examples/for-cursor/) | ⚠️ manual intents | ❌ per-project |
| **Codex, Aider, goose, Zed, Warp, Gemini CLI, Jules, Factory, others** | [`examples/AGENTS.md`](examples/AGENTS.md) at vault root | ❌ | ❌ per-project |

See [`examples/README.md`](examples/README.md) for the full feature matrix and known limitations per agent.

## Quick start

### Prerequisites

- [Claude Code](https://claude.ai/claude-code) (Anthropic's CLI/desktop agent)
- [Obsidian](https://obsidian.md) (free markdown editor — for browsing the wiki)

### Setup (2 minutes)

1. Open Claude Code
2. Copy the contents of [`setup-prompt.md`](setup-prompt.md) and paste it into Claude Code
3. Answer the questions (plain language, multiple choice)
4. Open the created folder in Obsidian
5. Done

That's it. The prompt handles everything:
- Creates your wiki vault with directories customized to your use case
- Sets up the schema (conventions, page formats, workflows)
- Configures cross-session persistence (wiki is active in every future session)
- Creates slash commands (`/ingest`, `/wiki`, `/lint-wiki`, etc.)
- Learns your preferred tools
- Explains how to use it

## How it works

### Three layers

```
raw/     Your source documents (articles, notes, transcripts). Immutable.
wiki/    LLM-generated pages (summaries, entities, concepts, projects). The LLM owns this.
schema   CLAUDE.md — tells the LLM how to maintain the wiki. You co-evolve this.
```

### Four operations

| Command | What it does |
|---------|-------------|
| `/ingest <file>` | Process a source into the wiki — summary, entities, cross-references |
| `/wiki <question>` | Search the wiki and get an answer with citations |
| `/lint-wiki` | Health check — orphans, broken links, stale info, suggestions |
| `/wiki-save <info>` | Save a piece of information to the right category |
| `/wiki-status` | Dashboard — page counts, recent activity, pending sources |

### Autonomous capture

The wiki grows without explicit effort. Every Claude Code session:
- **Start**: loads wiki context (index + recent activity)
- **During**: captures projects, decisions, people, research as they come up
- **End**: persists anything worth keeping, updates the log

You just work normally. The knowledge base fills in behind you.

## Use cases

- **Work**: projects, clients, meeting notes, decisions, architecture
- **Personal**: goals, routines, habits, health, journal entries
- **Research**: papers, articles, evolving thesis, open questions
- **Reading**: chapter summaries, characters, themes, plot threads
- **Learning**: course notes, concept maps, technique evaluations
- **Mix**: a broad wiki covering everything — the default

## Wiki structure

The setup prompt creates directories based on your use case. A typical structure:

```
your-wiki/
├── raw/                  # Your source documents (you add these)
│   └── assets/           # Downloaded images
├── wiki/                 # LLM-maintained (don't edit manually)
│   ├── index.md          # Master catalog of all pages
│   ├── log.md            # Chronological activity record
│   ├── sources/          # One summary per ingested document
│   ├── entities/         # People, orgs, clients, systems
│   ├── concepts/         # Ideas, frameworks, patterns
│   ├── topics/           # Thematic synthesis pages
│   ├── analyses/         # Filed query results
│   ├── projects/         # Project tracking
│   ├── personal/         # Goals, routines, preferences
│   └── research/         # Tech exploration, learning notes
└── CLAUDE.md             # Schema — the wiki's rulebook
```

## Skills (slash commands)

Five built-in skills are created during setup. Over time, Claude suggests new skills based on your workflow patterns — you accumulate a personalized command palette.

See [`docs/09-skills.md`](docs/09-skills.md) for details.

## Tools & preferences

Claude remembers your preferred tools from first mention — editor, framework, hosting, everything. It then uses your tools in all suggestions and commands. No configuration needed.

See [`docs/10-tools-and-preferences.md`](docs/10-tools-and-preferences.md) for details.

## Agent personality & user profile

Two additional layers shape how Claude communicates with you:

- **Agent personality** (`wiki/personal/agent-personality.md`) — You configure how Claude should talk: tone, verbosity, proactivity. Setup asks 3 quick questions to seed it. Feedback ("be more concise", "stop apologizing") persists to this file, so you only say it once.
- **User profile** (`wiki/personal/user-profile.md`) — A conservative, learned profile of your habits, needs, and communication style. Updated silently over time (2+ observations, no speculation). Claude uses it to adapt how it communicates with you — without ever calling attention to it.

See [`docs/11-agent-personality.md`](docs/11-agent-personality.md) and [`docs/12-user-profile-memory.md`](docs/12-user-profile-memory.md).

## Documentation

The [`docs/`](docs/) folder contains detailed guides explaining every aspect of the setup:

| Doc | Content |
|-----|---------|
| [00-overview](docs/00-overview.md) | Guide overview and quick start |
| [01-initial-prompt](docs/01-initial-prompt.md) | The setup prompt — design decisions and commentary |
| [02-obsidian-setup](docs/02-obsidian-setup.md) | Vault creation, directories, git |
| [03-schema-design](docs/03-schema-design.md) | CLAUDE.md schema — conventions and formats |
| [04-global-config](docs/04-global-config.md) | Cross-session persistence via global config |
| [05-wiki-categories](docs/05-wiki-categories.md) | Page types and what goes where |
| [06-workflows](docs/06-workflows.md) | Ingest, query, lint, autonomous capture |
| [07-obsidian-plugins](docs/07-obsidian-plugins.md) | Recommended Obsidian plugins |
| [08-tips-and-evolution](docs/08-tips-and-evolution.md) | Usage tips and schema evolution |
| [09-skills](docs/09-skills.md) | Slash commands and proactive skill creation |
| [10-tools-and-preferences](docs/10-tools-and-preferences.md) | Tool preference tracking |
| [11-agent-personality](docs/11-agent-personality.md) | User-set communication preferences that persist |
| [12-user-profile-memory](docs/12-user-profile-memory.md) | Conservative learned profile of the user |

## Examples

The [`examples/`](examples/) folder contains:
- Example skill files you can copy to `~/.claude/skills/`
- Example CLAUDE.md schema for the wiki vault
- Example global config for `~/.claude/CLAUDE.md`
- A blank wiki template you can use as a starting point

## Why this works

The tedious part of maintaining a knowledge base is the bookkeeping — updating cross-references, keeping summaries current, noting contradictions, maintaining consistency. Humans abandon wikis because maintenance grows faster than value. LLMs don't get bored and can touch 15 files in one pass. The wiki stays maintained because the cost of maintenance is near zero.

The human's job: curate sources, direct analysis, ask good questions, think about what it means.
The LLM's job: everything else.

## Contributing

This is an evolving pattern. If you've adapted it for your use case and found improvements:
- Open an issue to discuss ideas
- Submit a PR with your changes
- Share your experience in discussions

## License

MIT
