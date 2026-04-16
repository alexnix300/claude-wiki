---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, categories, research]
---

# Step 5 — Wiki Categories & Page Types

## Context

The wiki organizes knowledge into distinct page types, each in its own directory. This structure helps the LLM file information consistently and helps you browse the wiki in Obsidian.

## Categories

### Sources (`wiki/sources/`)

One page per ingested document. The foundation layer — everything else is derived from or connected to sources.

**When created:** During the ingest workflow.

**Contains:**
- Title, author, date, URL (if applicable)
- Structured summary (not just a copy of the original)
- Key takeaways (bulleted, specific)
- Tags for categorization
- Links to entity/concept/topic pages that were created or updated

**Example filename:** `wiki/sources/building-a-second-brain.md`

### Entities (`wiki/entities/`)

One page per person, organization, client, product, or system. These are the "nouns" of your knowledge base.

**When created:** During ingest (mentioned in a source), during conversation (user mentions a person/org), or proactively by the LLM when an entity appears repeatedly.

**Contains:**
- Who/what it is (brief description)
- Relationship to user (client, colleague, tool, etc.)
- Key facts and context
- Links to sources, projects, and other entities

**Example filenames:** `wiki/entities/acme-corp.md`, `wiki/entities/john-smith.md`

### Concepts (`wiki/concepts/`)

One page per idea, framework, technique, or pattern worth tracking across sources.

**When created:** When a concept appears in multiple sources or when the user explores an idea worth filing.

**Contains:**
- Definition/explanation
- Where it appears (which sources, projects)
- How it connects to other concepts
- User's take or application

**Example filenames:** `wiki/concepts/rag-architecture.md`, `wiki/concepts/pomodoro-technique.md`

### Topics (`wiki/topics/`)

Broader thematic pages that synthesize across multiple sources and entities. Think of these as "overview articles."

**When created:** When enough material accumulates around a theme, or when the user asks a broad question worth filing.

**Contains:**
- Synthesis of what's known across all sources
- Key entities involved
- Open questions
- Evolution over time (how understanding has changed)

**Example filenames:** `wiki/topics/authentication-patterns.md`, `wiki/topics/q2-priorities.md`

### Analyses (`wiki/analyses/`)

Filed answers to queries, comparisons, investigations. These are the output of exploration — questions you asked and answers worth keeping.

**When created:** After a query produces a substantial answer. The LLM offers to file it; the user decides.

**Contains:**
- The question that prompted it
- The synthesized answer with citations
- Any tables, comparisons, or structured data
- Date (analyses can become stale)

**Example filenames:** `wiki/analyses/react-vs-vue-2026.md`, `wiki/analyses/auth-provider-comparison.md`

### Projects (`wiki/projects/`)

One page per project — personal or client work. The most dynamic category; updated frequently as work progresses.

**When created:** When the user starts a new project or mentions an ongoing one for the first time.

**Contains:**
- Status (active, paused, completed)
- Goals and scope
- Tech stack and architecture decisions
- Timeline and milestones
- Blockers and open questions
- Key decisions and rationale
- Links to relevant entities, concepts, sources

**Example filenames:** `wiki/projects/client-dashboard-redesign.md`, `wiki/projects/personal-site-rebuild.md`

### Personal (`wiki/personal/`)

Routines, goals, habits, health, preferences, life context. Information about the user as a person, not tied to a specific project.

**When created:** When the user shares personal information, goals, routines, or preferences.

**Contains:**
- Whatever is shared — goals, habits, routines, preferences
- Changes over time (the LLM updates rather than duplicates)
- No judgment — factual recording of what the user shares

**Example filenames:** `wiki/personal/morning-routine.md`, `wiki/personal/2026-goals.md`, `wiki/personal/work-preferences.md`

### Research (`wiki/research/`)

Technology evaluations, learning notes, tool comparisons, deep dives. Knowledge gained from exploration.

**When created:** When the user explores a technology, evaluates tools, or does focused research on a topic.

**Contains:**
- What was researched and why
- Findings (specific, not vague)
- Conclusions or next steps
- Links to sources if applicable

**Example filenames:** `wiki/research/llm-wiki-pattern.md`, `wiki/research/bun-vs-node-2026.md`

### Meta (`wiki/meta/`)

Setup documentation, instructions, this guide. Not part of the knowledge base proper — it's documentation about the system itself.

**When created:** During initial setup and when documenting process changes.

## Comment — Organic growth

You don't need to populate all categories from day one. The wiki grows organically:
- Session 1: Maybe one project page and one entity page
- Session 10: A handful of projects, several entities, a few research pages
- Session 50: A rich, interconnected knowledge base

The categories exist to give the LLM clear filing conventions from the start. Empty categories are fine — they'll fill naturally.
