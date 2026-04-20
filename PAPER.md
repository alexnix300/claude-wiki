---
type: research
created: 2026-04-20
updated: 2026-04-20
status: draft for public release
author: Alex Nix
tags: [knowledge-management, agentic-systems, llm, memory, publication]
---

# The Agentic Wiki
### A Practitioner's Implementation of the LLM-Maintained Personal Knowledge Base

**Author:** Alex Nix
**Status:** Working draft — for public release
**Inspired by:** Andrej Karpathy's *LLM Wiki* methodology

---

## Abstract

Most LLM-document systems operate as retrieval: upload files, query them, receive a synthesis. Nothing accumulates. Every query re-derives knowledge from scratch, and the structure the model built while answering one question is discarded before the next. This paper describes the **Agentic Wiki** — an implementation of Karpathy's LLM Wiki methodology in which the model does not merely retrieve but **maintains** a persistent, interlinked markdown knowledge base. Sources are ingested once; the wiki is kept current across sessions; cross-references, contradictions, and syntheses are compiled as a side effect of daily work. The contribution of this paper is not the concept — that belongs to Karpathy and, distantly, to Vannevar Bush — but a working reference implementation: Obsidian as the frontend, Claude Code as the maintainer, five slash commands as the interface, an open-source repository reproducible in a single prompt. We describe the architecture, the cross-session persistence mechanism, the three-layer personalization model (tools, personality, profile), and the failure modes we observed in production.

---

## 1. Motivation

Three failure modes motivated this work.

**RAG has no memory.** Conventional retrieval-augmented systems — NotebookLM, ChatGPT file uploads, vector-store chatbots — treat each query as independent. The model searches, synthesizes, and forgets. If a user asks a subtle question requiring cross-document synthesis, the model reconstructs that synthesis on demand and discards it after the session. The connections it found are not stored. The second identical question runs the same expensive synthesis again. Karpathy observed the same thing from the practitioner side: *"I thought I had to reach for fancy RAG, but the LLM has been pretty good about auto-maintaining index files and brief summaries of all the documents"* [1]. The fix turned out to be structural, not algorithmic.

**Human wikis collapse under maintenance.** Every knowledge base fails in the same way: the cost of keeping the graph consistent — updating cross-references, reconciling contradictions, pruning stale pages — grows faster than the value the graph produces. Humans abandon wikis not because wikis are bad, but because maintenance is boring and boredom scales with page count.

**LLMs are maintenance-native.** An LLM does not experience maintenance as drudgery. It will update fifteen pages in one pass, chase every cross-reference, flag every contradiction, and do it again the next session without complaint. The scarce resource that killed human wikis — attention to bookkeeping — is abundant for agents.

The opportunity, then, is to build a system in which the human owns curation, exploration, and judgment, and the agent owns the entire maintenance layer.

## 2. Related work & lineage

**Vannevar Bush (1945)** proposed the Memex [2] — a personal, curated knowledge store with associative trails between documents. What Bush could not solve was the cost of building and maintaining those trails. Every ambitious personal-knowledge project since (Zettelkasten, Roam, Obsidian, TiddlyWiki) has been a partial instantiation of the Memex idea, hamstrung by that same maintenance problem.

**Andrej Karpathy's "LLM Knowledge Bases"** post [1] is the direct inspiration for the present work. Karpathy describes a workflow in which source documents sit in a `raw/` directory and the LLM incrementally "compiles" them into a wiki — a tree of markdown files with summaries, concept articles, backlinks, and an auto-maintained index. Obsidian is the frontend; the LLM writes and maintains the wiki; the human rarely edits directly. Operations include ingest, Q&A against the wiki, rendering outputs (markdown, slides, charts) that can be filed back as pages, and periodic linting for consistency and missing data. At ~100 articles and ~400K words, Karpathy reports the index-first approach outperforms "fancy RAG" — a practitioner's anecdote that matches the theoretical argument in §1. His closing line — *"I think there is room here for an incredible new product instead of a hacky collection of scripts"* — frames the present work as a response: a reproducible, one-prompt implementation rather than a bespoke setup.

**Obsidian and the Zettelkasten tradition** supply the frontend and the linking conventions. Obsidian's `[[wikilink]]` syntax, graph view, and plain-markdown storage make it an ideal viewer for an agent-maintained vault. The Zettelkasten principle of atomic, interlinked notes matches what an LLM produces naturally.

**Classical RAG systems** provide the contrast. RAG retrieves; an Agentic Wiki retrieves *and persists the synthesis*. RAG rebuilds structure per query; an Agentic Wiki compiles structure once and then keeps it current.

## 3. System architecture

The Agentic Wiki has three layers.

**Raw sources (`raw/`)** — the immutable document collection. Articles, PDFs, meeting transcripts, voice notes, clipped web pages. The agent reads from here but never modifies. This is the ground truth.

**The wiki (`wiki/`)** — markdown pages generated and maintained by the agent. Organized by page type:

| Directory | Purpose |
|-----------|---------|
| `wiki/sources/` | One summary page per ingested document |
| `wiki/entities/` | People, organizations, clients, systems |
| `wiki/concepts/` | Ideas, frameworks, patterns |
| `wiki/topics/` | Thematic synthesis across multiple sources |
| `wiki/analyses/` | Filed query results and investigations |
| `wiki/projects/` | Ongoing projects — work, personal, research |
| `wiki/personal/` | Goals, routines, preferences, life context |
| `wiki/research/` | Technology exploration, paper drafts, evaluations |
| `wiki/meta/` | Documentation of the system itself |

Two files are special: **`wiki/index.md`** is a master catalog — every page listed with a one-line summary, organized by type. The agent reads the index first when answering queries; it functions as a lightweight in-model search index and works surprisingly well up to ~100 sources and hundreds of pages. **`wiki/log.md`** is append-only and chronological, with each entry prefixed `## [YYYY-MM-DD] operation | Subject` so simple unix tools can parse it.

**The schema (`CLAUDE.md`)** — a markdown file at the vault root that tells the agent how the wiki is structured, what conventions to follow, what page format to use, and what workflows to run. This is the rulebook. It is the most important file in the system. Without it, the agent is a chatbot; with it, the agent is a disciplined wiki maintainer whose behavior is consistent across sessions.

Every wiki page uses the same format: YAML frontmatter (type, created, updated, tags) followed by markdown with `[[wikilinks]]` for cross-references. Every page must have at least one inbound link.

## 4. Core operations

**Ingest.** A source enters `raw/`. The agent reads it, discusses key takeaways with the user, writes a summary page in `wiki/sources/`, and propagates: for every person mentioned, update or create an entity page; for every concept, a concept page; for every relevant topic or project, update the synthesis. A single ingest can touch 10–15 pages. This is where the compounding happens.

**Query.** The user asks a question. The agent reads `wiki/index.md`, identifies relevant pages, reads them, and synthesizes an answer with `[[wikilinks]]` as citations. Substantial answers — comparisons, investigations, multi-page syntheses — are offered back for filing as `wiki/analyses/`. Good answers should not disappear into chat history; they compound with ingested sources.

**Lint.** Periodically, the agent audits: orphan pages, broken wikilinks, stale content (frontmatter `updated:` older than a threshold), contradictions across pages, concepts mentioned in wikilinks but missing a dedicated page, sources in `raw/` without summaries. The agent reports findings by severity and offers to fix them. Lint is the anti-entropy mechanism.

**Autonomous capture.** This is the operation that makes the system feel alive. At session start, the agent reads the index and recent log for context. During the session, it captures noteworthy information — project decisions, new people, research findings, personal context — as the conversation produces them. At session end, it persists anything worth keeping. The user does not explicitly invoke this; it is part of every session by default.

## 5. Cross-session persistence

Autonomous capture works because of one small implementation detail: a **global configuration file** at `~/.claude/CLAUDE.md` that Claude Code reads at the start of every session regardless of working directory. This file tells the agent to:

1. Read the wiki index and recent log entries on session start
2. Capture projects, decisions, entities, research findings, and personal info as they arise
3. Persist anything worth keeping before the session ends

The user can be coding on an unrelated project, browsing a terminal in an unrelated directory, or writing prose — and the wiki still grows. The maintenance cost of the knowledge base is invisible, which is the point. Humans abandon wikis because maintenance is visible; an Agentic Wiki's maintenance cost is near zero and out of sight.

The capture heuristic is deliberate: **capture** decisions, rationale, status changes, new entities, personal context, research findings; **skip** transient debugging, one-off typo fixes, anything that will not matter tomorrow.

## 6. Slash-command interface

The wiki exposes five slash commands, each implemented as a `SKILL.md` file under `~/.claude/skills/<name>/`:

| Command | Purpose |
|---------|---------|
| `/ingest <file>` | Run the full ingest workflow on a source |
| `/wiki <question>` | Search the wiki, synthesize an answer with citations |
| `/wiki-status` | Dashboard: page counts, recent activity, pending sources |
| `/lint-wiki` | Full health check with suggestions |
| `/wiki-save <info>` | Manually file a piece of information in the right category |

The skill format is a single markdown file with YAML frontmatter. There is no code. The agent interprets the markdown as instructions when the command is invoked. This matters for non-technical users: a new skill is just a paragraph of English prose, not a function.

**Proactive skill identification** extends this. The global config instructs the agent to watch for recurring user workflows and suggest promoting them to skills. A freelancer accumulates `/invoice-prep`; a researcher accumulates `/literature-review`; the palette reflects how the user actually works, not what was shipped on day one.

## 7. Adaptive personalization

Three complementary memory layers determine how the agent communicates with the user.

**Tools & preferences (`wiki/personal/tools-and-preferences.md`).** The first time the user mentions a tool — a package manager, a hosting provider, a design application — the agent saves it. On subsequent interactions, suggestions are grounded in the user's actual stack. No explicit configuration is required; the file fills in naturally by the third or fourth session.

**Agent personality (`wiki/personal/agent-personality.md`).** The user defines, at setup time, how the agent should communicate — tone (casual / professional / direct), verbosity (concise / balanced / detailed), proactivity (ask first / just do it). Subsequent feedback ("stop apologizing", "no emoji", "be direct") is appended as a persistent guideline. A one-session correction becomes permanent.

**User profile (`wiki/personal/user-profile.md`).** A conservatively learned model of the user's habits, recurring needs, communication patterns, and domain expertise. Updated silently, in place, only when a pattern is observed multiple times. The boundary is explicit: no speculation, no armchair psychology, no trait inferences beyond observable behavior. The goal is to adapt communication — not to perform a caricature of the user back to them.

The three layers separate what the user *tells* the agent (personality), what the user *uses* (tools), and what the agent *observes* (profile). They reinforce each other but never merge. A contradiction between the personality file and the profile is a signal — probably a clarification the agent should request, not silently resolve.

## 8. Reference implementation

The system is open source: **https://github.com/alexnix300/claude-wiki**. The repository includes:

- `setup-prompt.md` — a single prompt that, pasted into Claude Code, sets up the entire system: asks 6–8 plain-language questions, creates the vault with customized subdirectories, writes the schema, configures cross-session persistence, creates the five slash commands, seeds the personality and tools files, and explains usage.
- `docs/` — 13 numbered guides explaining every design decision, from the seed prompt through workflows, skills, and personalization.
- `examples/` — skill templates, an example global config, an example vault schema, and a blank wiki directory structure.
- `README.md` and `LICENSE` (MIT).

The setup is deliberately non-technical. A user installs Obsidian, installs Claude Code, pastes one prompt, answers a handful of multiple-choice questions, and opens the created folder in Obsidian. There is no terminal work, no manual file editing, no configuration language to learn. The agent handles everything.

## 9. Failure modes

Production experience surfaced five failure modes. None is fatal; all are tractable.

| Failure | Symptom | Mitigation |
|---------|---------|-----------|
| **Drift between copies** | Local wiki, global config, and public repo fall out of sync | Explicit sync workflow in the global config; periodic reconciliation prompts |
| **Inconsistent autonomous capture** | Agent captures aggressively in some sessions, sparsely in others | Tighten the capture heuristic in the schema; prefer explicit examples ("capture: decisions; skip: debugging noise") |
| **Over-aggressive profile inference** | Profile records traits that were speculative, not observed | Conservative boundaries in the schema: "observed 2+ times", "no armchair psychology", "update in place, never accumulate" |
| **Stale pages** | Pages written months ago contradict newer sources | Lint with an explicit staleness threshold; surface during routine `/lint-wiki` |
| **Unflagged contradictions** | New source contradicts an existing page silently | Ingest workflow explicitly requires contradiction checking; both pages get a flag |

The most serious is drift. Any system with three copies of the same state (local wiki, global config, public repo) must either eliminate copies or mechanize their reconciliation. We chose mechanization: the global config contains explicit sync instructions, and the agent is asked to verify sanitization before every public-repo commit.

## 10. Open research questions

- **Multi-user wikis.** This implementation is single-user. A team wiki introduces merge conflicts, access control, and differing perspectives on the same entity. None of these are solved.
- **Scale past the index.** At ~100 sources and a few hundred pages, the `index.md`-first retrieval works. At 10× that scale, proper vector search will be required. When to introduce embeddings, and how to hybridize them with the existing index, is an open design question.
- **Agent-agnostic portability.** The current implementation is Claude Code-specific in three ways: the `CLAUDE.md` filename, the `~/.claude/skills/` skill format, and the slash-command syntax. A truly portable version would carry a neutral `SCHEMA.md` and generate agent-specific adapters. This is a natural next step.
- **Objective evaluation.** "The wiki is useful" is a subjective claim. Metrics that would support it — query latency, answer precision, structural health over time, user retention — are not yet operationalized.
- **Local-first vs. cloud-sync.** The system as described is fully local. A team or multi-device user eventually wants sync. Git is the obvious substrate, but conflict resolution in an agent-maintained vault raises questions the git model does not answer.
- **Skill discovery at scale.** Proactive skill identification works for one user with a few skills. What it looks like with a community of users contributing skill templates, without becoming an unvetted app store, is unclear.

## 11. Conclusion

The Agentic Wiki is not a new idea. It is an implementation of an idea — Karpathy's — in a form that is reproducible in one prompt, usable by non-technical users, and open-sourced under MIT. The contribution is the concreteness: a directory layout that works, a schema that enforces consistency, a set of operations that compound, a personalization model that stays conservative, and a reference repository anyone can fork.

The key empirical observation is that **maintenance is the constraint**, and **LLMs remove it**. Every prior personal-knowledge system — from Bush's Memex forward — has assumed a human doing the bookkeeping. An agent that does the bookkeeping changes the equation. The wiki becomes useful not because any single page is valuable but because the fiftieth page is connected to forty-nine others, and the agent made those connections without being asked.

We expect many variations of this pattern. We encourage forks.

## References

[1] Karpathy, A. *LLM Knowledge Bases* [post]. X (@karpathy). https://x.com/karpathy/status/2039805659525644595

[2] Bush, V. (1945). *As We May Think.* The Atlantic, July 1945. https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/

## See also

- [[aoc-framework-paper]] — companion paper on prompt architecture (A.O.C. framework)
- [[two-stage-architect-pattern-paper]] — separating creative intent from technical generation
- [[00-overview]] — meta documentation of the system
- [[04-global-config]] — cross-session persistence mechanism
- [[09-skills]] — slash-command interface
- [[11-agent-personality]] — user-set communication contract
- [[12-user-profile-memory]] — conservative learned profile

---

*Citation:* Nix, A. (2026). *The Agentic Wiki: A Practitioner's Implementation of the LLM-Maintained Personal Knowledge Base.* Based on Karpathy's *LLM Knowledge Bases* [1]. https://github.com/alexnix300/claude-wiki
