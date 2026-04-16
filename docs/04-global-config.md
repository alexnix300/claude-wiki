---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, config, research]
---

# Step 4 — Global Configuration (~/.claude/CLAUDE.md)

## Context

The wiki schema (`CLAUDE.md` inside the vault) only works when Claude Code is running *inside* the vault directory. But you want the wiki to be active in every session — even when working on a completely different project in a different directory. This requires a **global** configuration file.

## The key insight

Claude Code reads `~/.claude/CLAUDE.md` at the start of every session, regardless of working directory. By putting wiki instructions there, the LLM maintains the wiki as a background activity across all sessions.

## What the global config does

Three phases per session:

### Session start
```
1. Read ~/Documents/WIKI/wiki/index.md → understand current wiki state
2. Read ~/Documents/WIKI/wiki/log.md (last 20 entries) → recent activity
3. Use this context throughout the session
```

### During session (autonomous)
Capture information with lasting value as it comes up:
- **Project updates** → `wiki/projects/`
- **People & clients** → `wiki/entities/`
- **Personal info** → `wiki/personal/`
- **Research & learnings** → `wiki/research/`
- **Decisions & rationale** → wherever relevant

The LLM does this without being asked. It's part of normal session behavior.

### Session end
```
1. Review what was discussed/accomplished
2. Update or create relevant wiki pages
3. Update wiki/index.md if new pages were created
4. Append session entry to wiki/log.md
```

### Capture vs. skip heuristic

**Capture:** Decisions, context, rationale, project status changes, new entities, personal info, research findings, recurring patterns, things you'd want to remember in 2 weeks.

**Skip:** Transient debugging, one-off typo fixes, things in git history, ephemeral task coordination.

## Prompt used

> "can we add to global claude.md to use wiki across the sessions doesn't matter what we working on? for example - creating structure for: personal routines and chats, projects - personal / clients, research, etc. and constantly update it so we dont need to call it each time and it is doing it autonomously each session. in the beginning of each session it updates the current state, request and along the way saving info correctly across the wiki"

## The actual file

Location: `~/.claude/CLAUDE.md`

Complete contents:

```markdown
# Global Instructions

## Personal Wiki — Always Active

A persistent personal wiki lives at `~/Documents/WIKI/`. This wiki is
active in **every session**, regardless of what project or directory you're
working in. It is your long-term knowledge base about the user, their work,
their projects, and their life.

### Session lifecycle

**Start of session:**
1. Read `~/Documents/WIKI/wiki/index.md` to understand the current
   state of the wiki.
2. Read `~/Documents/WIKI/wiki/log.md` (last 20 entries) to understand
   recent activity.
3. Use this context to inform your work throughout the session.

**During the session — capture autonomously:**
As you work, identify and file information that has lasting value. Do this
naturally as part of the conversation — don't ask permission for routine wiki
updates. Categories of what to capture:

- **Project updates**: New projects started, milestones hit, decisions made,
  tech stack choices, blockers encountered, architecture decisions. File under
  `wiki/projects/`.
- **People & clients**: New contacts, client preferences, org structures, key
  relationships. File under `wiki/entities/`.
- **Personal**: Routines, goals, habits, preferences, health notes, life
  events the user shares. File under `wiki/personal/`.
- **Research & learnings**: New technologies explored, articles discussed,
  frameworks evaluated, lessons learned. File under `wiki/research/`.
- **Decisions & rationale**: Why something was done a certain way — these are
  the things most easily forgotten and most valuable to recall.

**End of session (before the conversation ends):**
1. Review what was discussed and accomplished.
2. Update or create relevant wiki pages for anything worth persisting.
3. Update `wiki/index.md` if new pages were created.
4. Append a session entry to `wiki/log.md`.

### What to capture vs. what to skip

**Capture:** Decisions, context, rationale, project status changes, new
entities, personal info shared, research findings, recurring patterns,
client/stakeholder info, things the user would want to remember in 2 weeks.

**Skip:** Transient debugging details, one-off typo fixes, things already in
git history, ephemeral task coordination that won't matter tomorrow.

### Wiki structure

(directory tree matching the vault layout)

### Page format

Use Obsidian-style markdown with YAML frontmatter.

### Guidelines

- Use [[wikilinks]] for all internal cross-references.
- Update existing pages rather than creating duplicates.
- Keep pages focused. One project per page, one entity per page.
- Update the updated: date in frontmatter when modifying a page.
- Always update wiki/index.md when creating new pages.
- Always append to wiki/log.md when making wiki changes.
- Be concise. Bullet points over prose.
- When working in a different project directory, use absolute paths.
```

## Comment — Why global, not project-level

Project-level CLAUDE.md files only apply when working in that directory. The wiki needs to capture information from *any* session — a coding session on Project A might reveal decisions worth logging, a research session might surface entities worth tracking. The global config ensures nothing falls through the cracks.

The tradeoff: the global CLAUDE.md adds instructions to every session, even ones where the wiki isn't relevant. In practice this is fine — the LLM reads the wiki index (lightweight) and only writes when there's something worth capturing. If a session is purely mechanical (e.g. "fix this typo"), nothing gets written.
