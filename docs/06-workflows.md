---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, workflows, research]
---

# Step 6 — Core Workflows

## Context

The wiki has three explicit workflows (ingest, query, lint) plus the implicit autonomous capture that happens every session via the global config.

## Workflow 1: Ingest

**Trigger:** User adds a file to `raw/` and asks the LLM to process it, or pastes content directly into the conversation.

**Steps:**

1. Read the source thoroughly
2. Discuss key takeaways with the user (don't just silently process — surface insights, ask if anything should be emphasized)
3. Create a source summary page in `wiki/sources/`
4. Create or update entity pages for people, orgs, projects mentioned
5. Create or update concept/topic pages as needed
6. Update cross-references across all touched pages
7. Update `wiki/index.md`
8. Append to `wiki/log.md`

**Example prompt:**

> "I just added an article to raw/. Please ingest it."

Or simply paste text:

> "Here's the transcript from today's client call: [content]. Please ingest this."

**Comment:** A single ingest can touch 10-15 wiki pages. The LLM creates the source summary, but also updates entity pages (new person mentioned?), project pages (new decision?), topic pages (new data point?). This is where the compounding happens.

## Workflow 2: Query

**Trigger:** User asks a question about something in the wiki.

**Steps:**

1. Read `wiki/index.md` to find relevant pages
2. Read those pages
3. Synthesize an answer with `[[wikilinks]]` to sources
4. If the answer is substantial, offer to file it as an analysis page in `wiki/analyses/`

**Example prompts:**

> "What do we know about Acme Corp's authentication requirements?"
> "Compare the three deployment approaches we've discussed."
> "What's the current status of all active projects?"

**Comment:** The query workflow benefits from the wiki's accumulated structure. Instead of searching through raw sources, the LLM reads pre-synthesized pages with cross-references already in place. A question that would require re-reading 5 raw sources can be answered from 2-3 wiki pages.

## Workflow 3: Lint

**Trigger:** User asks for a wiki health check, or periodically as maintenance.

**Steps:**

1. Check for orphan pages (no inbound links)
2. Check for stale information (superseded by newer sources)
3. Check for contradictions between pages
4. Check for important concepts mentioned but lacking their own page
5. Suggest new questions to investigate or sources to look for
6. Report findings and fix issues with human approval

**Example prompts:**

> "Lint the wiki."
> "Any contradictions or stale info?"
> "What's missing from the wiki?"

**Comment:** Linting is underrated. It surfaces gaps you didn't know existed and keeps the wiki honest. Running it monthly (or whenever things feel messy) is enough.

## Workflow 4: Autonomous capture (implicit)

**Trigger:** Every session, automatically, via global `~/.claude/CLAUDE.md`.

**What happens:**

- Session start: LLM reads index and recent log for context
- During session: LLM files noteworthy information as it comes up
- Session end: LLM reviews and persists anything worth keeping

**What gets captured:**

- Project status changes, decisions, architecture choices
- New people, clients, organizations mentioned
- Personal info, routines, goals shared by the user
- Research findings, technology evaluations
- Rationale for decisions (the "why" behind choices)

**What doesn't get captured:**

- Transient debugging details
- One-off fixes already in git
- Ephemeral task coordination

**Comment:** This is the "killer feature" — the wiki grows without explicit effort. You just work normally, and the knowledge base fills in behind you. Over weeks, you accumulate a detailed record of decisions, context, and connections that would otherwise be lost to chat history.

## How workflows interact

```
Ingest → creates/updates many pages → triggers index + log updates
Query → reads existing pages → optionally creates analysis pages
Lint → reads all pages → suggests updates → human approves
Autonomous → captures during normal work → creates/updates pages
```

All four workflows converge on the same wiki. A page created by autonomous capture gets enriched by a later ingest. An analysis from a query gets cross-referenced when a related source is ingested. The knowledge compounds.
