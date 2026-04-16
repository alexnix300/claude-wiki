---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, tips, research]
---

# Step 8 — Tips, Tricks & Schema Evolution

## Tips from usage

### Working with the LLM

- **Stay involved during ingests.** Don't just say "ingest this" and walk away. Read the summaries, check what entities were created, guide emphasis. The first few ingests set the tone for the wiki.
- **File good query answers.** When you ask a question and get a great synthesized answer, file it as an analysis page. Otherwise it's lost to chat history.
- **Use lint periodically.** Say "lint the wiki" once a month. It surfaces orphans, contradictions, and gaps you didn't know about.
- **The LLM suggests sources.** After a lint pass, the LLM often suggests "you might want to find a source on X" — follow these leads.

### Working with Obsidian

- **Graph view is your dashboard.** Open it regularly. Look for:
  - Disconnected clusters (knowledge silos that should be linked)
  - Orphan nodes (pages no one links to)
  - Hub nodes (highly connected pages — these are your core topics)
- **Use the backlinks panel.** When reading any page, the backlinks show everything that references it. This is the wiki working as intended.
- **Don't edit wiki pages yourself** (usually). Let the LLM maintain them. If you edit manually, tell the LLM in the next session so it stays in sync.

### Source management

- **Obsidian Web Clipper** for web articles → saves to `raw/`
- **Download images locally** (Ctrl+Shift+D) so the LLM can view them
- **PDF sources**: Drop PDFs into `raw/`. The LLM can read them directly.
- **Voice/meeting notes**: Transcribe first (Whisper, Otter.ai, etc.), then save transcript to `raw/`.
- **Name sources clearly**: `raw/2026-04-16-client-call-acme.md` is better than `raw/notes.md`

### Git workflow

- Commit after each session or major ingest
- The LLM can do this: "commit all wiki changes"
- Git diff shows exactly what the LLM changed — useful for review
- Branch before big reorganizations

## Schema evolution

The schema (CLAUDE.md) should evolve as you use the wiki. Common evolutions:

### Week 1-2: Discover missing page types
You might find you need:
- A "meeting notes" category
- A "decision log" page type
- A "reading list" or "to-research" queue

Tell the LLM: "Add a meeting-notes category to the wiki schema" and it will update CLAUDE.md and create the directory.

### Month 1: Refine conventions
- Tags that are too generic → make them more specific
- Pages that are too long → split conventions
- Pages that are too short → merge conventions
- Frontmatter fields that aren't useful → remove them

### Month 2+: Add new workflows
- "Archive" workflow for completed projects
- "Merge" workflow for overlapping pages
- "Export" workflow for sharing wiki content externally
- Search tooling (qmd, custom scripts) as the wiki grows beyond ~100 pages

### Ongoing: Question the structure
The initial categories are a starting point. If a category stays empty after a month, remove it. If you keep filing things in "concepts" that are really "techniques," create a techniques category. The structure should serve you, not the other way around.

## The meta-insight

The wiki's value is not in any single page. It's in the connections between pages, the accumulated context, and the fact that everything is already synthesized. The 50th page is more valuable than the first, because it's connected to 49 others. This is why the pattern compounds — and why it's worth the investment of setting it up properly.

## What this guide doesn't cover

- **Multi-user wikis**: This setup is for a single user with a single LLM. Multi-user wikis add collaboration complexity (merge conflicts, different perspectives, access control).
- **Embedding-based search**: At moderate scale (~100 sources, ~hundreds of pages), the index-based approach works. At larger scale, you'd want proper vector search (see qmd or build your own).
- **Other LLM agents**: This guide uses Claude Code. The pattern works with any agent that can read/write local files (Codex, OpenCode, Cursor, etc.) — adjust the schema filename and conventions accordingly.
