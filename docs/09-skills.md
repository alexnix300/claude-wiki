---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, skills, research]
---

# Step 9 — Custom Skills (Slash Commands)

## Context

Claude Code supports custom skills — reusable slash commands that automate multi-step workflows. Each skill is a markdown file with instructions the LLM follows when invoked. Skills are the wiki's "API" — they give the user (including non-technical users) simple one-word commands for complex operations.

## How skills work

### File structure

```
~/.claude/skills/
├── ingest/
│   └── SKILL.md
├── wiki-status/
│   └── SKILL.md
├── lint-wiki/
│   └── SKILL.md
├── wiki/
│   └── SKILL.md
└── wiki-save/
    └── SKILL.md
```

### Skill file format

```yaml
---
name: skill-name
description: What this skill does (used for auto-invocation hints)
argument-hint: "<what arguments look like>"
disable-model-invocation: true
---

Instructions the LLM follows when the skill is invoked.
Markdown format. Can reference $ARGUMENTS for user input.
```

### Key fields

| Field | Purpose |
|-------|---------|
| `name` | The slash command name (e.g. `ingest` → `/ingest`) |
| `description` | What it does — shown to user, helps LLM decide when to suggest it |
| `argument-hint` | Shows user what to type after the command |
| `disable-model-invocation` | `true` = user must type the command manually. Prevents accidental invocation. |

### Invoking

Type `/<skill-name>` in Claude Code:
- `/ingest raw/article.md`
- `/wiki-status`
- `/lint-wiki`
- `/wiki what do we know about authentication?`
- `/wiki-save client prefers weekly standups on Tuesdays`

## Built-in wiki skills

Five skills are created during setup:

### /ingest

**Purpose:** Process a source document into the wiki.

**What it does:**
1. Reads the source file (or asks user to paste content)
2. Discusses key takeaways with the user
3. Creates a source summary page
4. Creates/updates entity pages for people, orgs, systems mentioned
5. Creates/updates concept and topic pages
6. Adds cross-references across all touched pages
7. Updates index.md and log.md

**Usage:** `/ingest raw/2026-04-16-client-call.md` or `/ingest` then paste content.

**Why it matters:** Ingest is the most complex workflow — it can touch 10-15 pages. Having it as a one-word command means non-technical users never need to remember the steps.

### /wiki-status

**Purpose:** Quick overview of the wiki's current state.

**What it shows:**
- Page counts by category
- Recent log entries
- Pending unprocessed sources in raw/
- Basic health indicators

**Usage:** `/wiki-status`

**Why it matters:** Gives users a dashboard view without needing to navigate files.

### /lint-wiki

**Purpose:** Comprehensive health check.

**What it checks:**
- Orphan pages (no inbound links)
- Broken wikilinks
- Stale pages (not updated in 30+ days)
- Missing pages (linked but don't exist)
- Contradictions between pages
- Frontmatter issues
- Index out of sync
- Unprocessed sources

**What it suggests:**
- New pages that should exist
- Pages to merge
- Topics worth investigating
- Knowledge gaps to fill

**Usage:** `/lint-wiki`

### /wiki

**Purpose:** Ask a question and get an answer grounded in wiki content.

**What it does:**
1. Reads the index to find relevant pages
2. Reads those pages
3. Synthesizes an answer with citations
4. Offers to file substantial answers as analysis pages

**Usage:** `/wiki what's the status of all active projects?`

### /wiki-save

**Purpose:** Manually save a piece of information to the wiki.

**What it does:**
1. Analyzes the input to determine the right category
2. Checks if a relevant page exists
3. Updates existing page or creates new one
4. Adds cross-references, updates index and log

**Usage:** `/wiki-save John mentioned they're switching to Kubernetes next quarter`

## Proactive skill identification

The global config instructs the LLM to watch for opportunities to create new skills:

**When to suggest:**
- The user repeats the same type of request across sessions
- A multi-step workflow they run frequently
- A task specific to their domain that could be streamlined

**Process:**
1. LLM suggests: "You do this often — want me to create a /skill-name command for it?"
2. User agrees
3. LLM creates `~/.claude/skills/<name>/SKILL.md`
4. LLM documents it in `wiki/personal/custom-skills.md`
5. Skill is available immediately — no restart needed

**Examples of skills that might emerge:**
- `/standup` — Generate a standup summary from recent wiki activity
- `/client-brief <client>` — Compile everything known about a client
- `/weekly-review` — Summarize the week's wiki changes
- `/research <topic>` — Web search + wiki integration
- `/meeting-prep <topic>` — Pull relevant wiki context before a meeting

## Comment — Why skills matter for non-tech users

Skills are the UX layer that makes the wiki accessible. Instead of remembering "read the index, find relevant pages, check for entities..." the user types `/ingest`. Instead of "search across wiki files for mentions of..." they type `/wiki`.

The skill file itself is just a prompt — plain English instructions. The LLM interprets and executes them. This means:
- Skills are easy to create (just describe what should happen)
- Skills are easy to modify (edit the markdown)
- Skills are easy to share (copy the file)
- No code required

## Prompt used

> "can we also add important things: 1. skills creation - we should identify user's needs and suggest creating personal skills"

This prompted creation of:
1. Five built-in wiki skills (`~/.claude/skills/`)
2. A custom-skills tracking page (`wiki/personal/custom-skills.md`)
3. Proactive skill identification instructions in global `~/.claude/CLAUDE.md`
