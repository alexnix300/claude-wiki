---
type: meta
created: 2026-04-20
updated: 2026-04-20
tags: [setup, principles, discipline, research]
---

# Step 13 — Maintenance Principles

## Context

The wiki schema defines **conventions** — page format, directory layout, tag style, cross-references. But conventions describe *what* a page looks like, not *how* the agent should behave when writing and editing. Behavior discipline is a separate layer.

Without explicit behavior principles, a well-instructed agent can still:

- Rewrite a page that only needed a one-line update
- Create new pages padded with "preparatory" sections that never get filled
- Make assumptions about a page's structure instead of reading it first
- Ship broken wikilinks or unindexed pages because verification is implicit

Four principles, adapted from Andrej Karpathy's observations about LLM coding pitfalls (as encoded in [`forrestchang/andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills)), close this gap. They apply to wiki maintenance the same way they apply to code editing: reasoning before action, minimality, surgical scope, verification.

## 1. Read Before Edit

**Always read a page fully before editing it.** Do not rely on memory or assumption about its current structure. If you haven't read it in this session, read it again.

**Before creating:** search the index first. Confirm no similar page already exists. If one does, update it instead.

**Before propagating:** when an ingest or update touches multiple pages, read each related page before modifying it. Don't infer what's there.

**Failure mode:** agent remembers a page having a "Status" section, writes to it, but the section was actually renamed "Current state" weeks ago — creating a duplicate section and inconsistent structure.

## 2. Minimal Page Surface

**Start pages with the minimum sections needed for the content you have right now.** Expand only when real content arrives.

No preparatory scaffolding. No empty "Future work," "TBD," or "Notes" sections "just in case." Empty sections read as noise and invite padding later.

A new entity page with one known fact should be one paragraph, not a six-section template. If more arrives, add sections then.

**Failure mode:** agent creates a project page with Status, Goals, Tech Stack, Timeline, Blockers, Decisions, Open Questions, Related People — based on one sentence the user mentioned — and then fills 2 of those 8 sections. The other 6 stay empty for months and hide the real signal.

## 3. Surgical Updates

**Edit only the section(s) that require change.** Preserve existing structure even when it's imperfect. If you think a full rewrite is warranted, state that explicitly and get consent before rewriting.

When a user says "add a line about X," add the line. Do not reformat, reorganize, or "improve" the surrounding content.

When ingesting new information about an existing entity, update the relevant sections. Do not restructure the whole page.

**Failure mode:** agent receives a one-fact update, silently rewrites the entire page "for consistency," and in the process loses nuance the user had added by hand.

## 4. Verify After Edit

**After any edit, verify:**

- Frontmatter parses (YAML valid, required fields present, `updated:` date current)
- `[[wikilinks]]` resolve to real pages (or flag unresolved links explicitly)
- `wiki/index.md` has an entry if a new page was created
- `wiki/log.md` has an entry for the operation

Verification is not optional. A broken wikilink discovered three sessions later costs more than verification in the moment.

**Failure mode:** agent creates a new page, forgets to add it to `index.md`, and three weeks later the page is orphaned — present on disk, invisible to future queries.

## Relationship to conventions

The conventions (in `CLAUDE.md` and meta docs 03–12) define the *what*. These principles define the *how*. Both are needed:

- Conventions without principles: correct format, sloppy execution
- Principles without conventions: disciplined, but inconsistent

Both read at session start. Both reinforced in skills (`/ingest`, `/wiki-save`, `/lint-wiki`).

## Attribution

Adapted from [`forrestchang/andrej-karpathy-skills`](https://github.com/forrestchang/andrej-karpathy-skills), which encodes four of Karpathy's observations about LLM coding pitfalls (Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution) as Claude Code guidelines. The present document translates those principles from code editing to wiki maintenance.

## See also

- [[03-schema-design]] — conventions (the *what*)
- [[06-workflows]] — ingest, query, lint operations
- [[09-skills]] — slash commands that reference these principles
- [[00-overview]] — guide overview
