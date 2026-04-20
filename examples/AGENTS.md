# AGENTS.md — Agentic Wiki

**Place this file at the root of your wiki vault.** It is read by any agent supporting the `AGENTS.md` convention: Codex, OpenCode, Cursor, Aider, goose, Zed, Warp, Gemini CLI, Jules, Factory, and others.

This file defines the rules for maintaining an LLM-managed personal knowledge base. The human curates sources and asks questions; the agent writes, updates, and cross-references the wiki.

## Vault layout

```
raw/                   # Immutable source documents. Agent reads but never modifies.
raw/assets/            # Downloaded images and attachments.
wiki/                  # Agent-maintained markdown pages.
wiki/index.md          # Master catalog of all pages, organized by type.
wiki/log.md            # Append-only chronological log of operations.
wiki/sources/          # One summary page per ingested document.
wiki/entities/         # People, organizations, clients, systems.
wiki/concepts/         # Ideas, frameworks, patterns.
wiki/topics/           # Thematic synthesis across multiple sources.
wiki/analyses/         # Filed query results and investigations.
wiki/projects/         # Project tracking — work, personal, research.
wiki/personal/         # Goals, routines, preferences, life context.
wiki/research/         # Technology exploration, paper drafts, evaluations.
wiki/meta/             # Documentation of the wiki system itself.
```

## Page format

Every wiki page uses YAML frontmatter followed by Obsidian-style markdown:

```markdown
---
type: source | entity | concept | topic | analysis | project | personal | research
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [relevant, tags]
---

# Page Title

Content with [[wikilinks]] to other pages.
```

Every page must have at least one inbound link from `wiki/index.md` or another page.

## Core operations

- **Ingest** — User adds a source to `raw/`. Agent reads it, writes a summary in `wiki/sources/`, creates or updates entity/concept/topic pages mentioned, adds cross-references, updates `wiki/index.md` and `wiki/log.md`.
- **Query** — User asks a question. Agent reads `wiki/index.md`, reads the relevant pages, synthesizes an answer with `[[wikilinks]]` as citations. Substantial answers may be filed as `wiki/analyses/` pages.
- **Lint** — Agent audits for orphans, broken wikilinks, stale content, contradictions, and missing pages; reports findings; fixes with consent.
- **Autonomous capture** — During normal work, agent captures project decisions, new entities, research findings, personal context. Captures decisions, rationale, status changes, new entities; skips transient debugging or one-off fixes.

## Maintenance principles

Four behavior rules for wiki editing:

1. **Read Before Edit** — Always read a page fully before editing. Before creating, check `wiki/index.md` for a similar page. Before propagating, read each related page.
2. **Minimal Page Surface** — Start pages with only the sections needed for the content available now. No preparatory scaffolding.
3. **Surgical Updates** — Edit only the section(s) that require change. Preserve existing structure. State a full rewrite explicitly before making one.
4. **Verify After Edit** — Frontmatter parses, wikilinks resolve, `wiki/index.md` updated if a new page was created, `wiki/log.md` appended.

## Index and log conventions

- `wiki/index.md`: catalog organized by page type, one entry per page: `- [[Page Title]] — one-line summary`. Update on every new page.
- `wiki/log.md`: append-only, each entry prefixed `## [YYYY-MM-DD] operation | Subject`. Operations: `ingest`, `query`, `lint`, `update`, `create`.

## Style guidelines

- Concise. Bullet points over prose.
- Specific claims over vague summaries.
- Use `[[wikilinks]]` for every cross-reference.
- When sources contradict each other, flag the contradiction explicitly on both pages.
- Attribute claims to their source.

## Agent-specific extensions

This `AGENTS.md` provides the portable baseline. For agent-specific features (slash commands, cross-session persistence, etc.), see:

- `for-claude-code/` — Full Claude Code kit (global config, skills, vault schema)
- `for-opencode/` — OpenCode slash commands
- `for-cursor/` — Cursor `.cursor/rules/` variant
- `../README.md` — Feature comparison matrix and which variant to use when

For agents not listed above (Aider, goose, Zed, Warp, Gemini CLI, Jules, Factory, etc.): this `AGENTS.md` alone is sufficient. Place it at the vault root and the agent will pick it up when working inside the vault.
