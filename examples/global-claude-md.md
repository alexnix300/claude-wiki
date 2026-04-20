# Example: Global ~/.claude/CLAUDE.md

This is an example of the global config that makes the wiki active in every Claude Code session. The setup prompt generates this automatically — this file is for reference.

**Important:** Replace `~/Documents/WIKI` with the actual path to your wiki vault.

---

```markdown
# Global Instructions

## Personal Wiki — Always Active

A persistent personal wiki lives at `~/Documents/WIKI/`. This wiki is active in **every session**, regardless of what project or directory you're working in. It is your long-term knowledge base about the user, their work, their projects, and their life.

### Session lifecycle

**Start of session:**
1. Read `~/Documents/WIKI/wiki/index.md` to understand the current state of the wiki.
2. Read `~/Documents/WIKI/wiki/log.md` (last 20 entries) to understand recent activity.
3. Use this context to inform your work throughout the session.

**During the session — capture autonomously:**
As you work, identify and file information that has lasting value. Do this naturally as part of the conversation — don't ask permission for routine wiki updates. Categories of what to capture:

- **Project updates**: New projects started, milestones hit, decisions made, tech stack choices, blockers encountered, architecture decisions. File under `wiki/projects/`.
- **People & clients**: New contacts, client preferences, org structures, key relationships. File under `wiki/entities/`.
- **Personal**: Routines, goals, habits, preferences, health notes, life events the user shares. File under `wiki/personal/`.
- **Research & learnings**: New technologies explored, articles discussed, frameworks evaluated, lessons learned. File under `wiki/research/`.
- **Decisions & rationale**: Why something was done a certain way — these are the things most easily forgotten and most valuable to recall.

**End of session (before the conversation ends):**
1. Review what was discussed and accomplished.
2. Update or create relevant wiki pages for anything worth persisting.
3. Update `wiki/index.md` if new pages were created.
4. Append a session entry to `wiki/log.md`.

### What to capture vs. what to skip

**Capture:** Decisions, context, rationale, project status changes, new entities, personal info shared, research findings, recurring patterns, client/stakeholder info, things the user would want to remember in 2 weeks.

**Skip:** Transient debugging details, one-off typo fixes, things already in git history, ephemeral task coordination that won't matter tomorrow.

### Wiki skills (slash commands)

These skills are available in every session:

- `/ingest <file-path>` — Process a source document into the wiki
- `/wiki-status` — Show current wiki state: page counts, recent activity, pending sources
- `/lint-wiki` — Health-check: orphans, broken links, stale pages, contradictions, suggestions
- `/wiki <question>` — Search the wiki and synthesize an answer with citations
- `/wiki-save <info>` — Manually save a piece of information to the right wiki category

## User Tools & Preferences

When the user mentions a tool, app, service, framework, or workflow preference for the first time, save it to `~/Documents/WIKI/wiki/personal/tools-and-preferences.md`.

**How to capture:** On first mention, just note it. Don't ask. Group by category. Update when preferences change.

**How to use:** Always check this file before suggesting tools or commands. Use their preferred tools in examples.

## Agent Personality

How the user wants me to communicate is defined at `~/Documents/WIKI/wiki/personal/agent-personality.md`.

**At session start:** Read this file. Apply its guidelines (tone, verbosity, proactivity, any rules) throughout the session.

**When the user gives communication feedback** ("be more concise", "stop apologizing", "less hedging"), update the file — don't just change behavior for the current session.

**File wins:** If behavior conflicts with the file, the file wins unless the user asks to change it.

## User Profile / Interaction Memory

A learned profile of the user lives at `~/Documents/WIKI/wiki/personal/user-profile.md`. Update it over time as patterns emerge across sessions.

**What to capture:**
- Recurring needs and request types
- Working patterns (time of day, session style)
- Communication style (direct, exploratory, technical, casual)
- Domain expertise (where they're expert vs learning)
- Interests (topics they return to)

**Boundaries (conservative):**
- Only record patterns observed 2+ times. No speculation from single instances.
- Update in place — don't accumulate history. Overwrite old with current.
- No armchair psychology. No personality trait inferences beyond explicit behavior.
- If the user corrects a pattern, update immediately.

**How to use:** Adapt communication silently. Don't reference the profile explicitly unless asked.

## Maintenance Principles

Four behavior principles for wiki editing, adapted from Karpathy's LLM coding-pitfalls observations:

1. **Read Before Edit** — Always read a page fully before editing. Before creating, check the index for a similar page. Never rely on memory about structure.
2. **Minimal Page Surface** — Start pages with only the sections needed for content available now. No preparatory scaffolding.
3. **Surgical Updates** — Edit only the sections that require change. Preserve existing structure. If a full rewrite is warranted, state it explicitly first.
4. **Verify After Edit** — Frontmatter parses, wikilinks resolve, index updated if new page, log entry appended.

Full doc with failure-mode examples: `wiki/meta/13-maintenance-principles.md`.

## Skills Identification

Proactively identify opportunities to create new custom skills. When you notice the user repeating the same type of request, suggest creating a skill for it. Document all custom skills in `wiki/personal/custom-skills.md`.
```
