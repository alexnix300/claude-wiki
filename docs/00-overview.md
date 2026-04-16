---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, instructions, research]
---

# LLM Wiki — Setup Guide Overview

This folder documents the complete process of setting up a personal wiki maintained by an LLM (Claude Code) inside Obsidian. It's written as a reproducible guide — every prompt, every file created, every decision explained.

## What this is

A pattern where an LLM incrementally builds and maintains a structured, interlinked knowledge base (markdown wiki) on your behalf. You curate sources, ask questions, and direct the work. The LLM writes, cross-references, and maintains everything.

Key difference from RAG: the wiki is a **persistent, compounding artifact**. Knowledge is synthesized once and kept current — not re-derived on every query.

## Prerequisites

- **Claude Code** (Anthropic's CLI agent) — or another LLM agent with file read/write access
- **Obsidian** — for browsing/visualizing the wiki (free, works on Mac/Windows/Linux)
- **Git** (optional) — for version history

## Quick start (non-technical users)

1. Install [Obsidian](https://obsidian.md) (free) and [Claude Code](https://claude.ai/claude-code)
2. Open Claude Code
3. Paste the seed prompt from [[01-initial-prompt]]
4. Answer the questions the agent asks you (plain language, multiple choice)
5. Open the created folder in Obsidian
6. Done — the wiki is live and grows automatically from every session

That's it. The single prompt in Step 1 handles everything: asks your preferences, creates the vault, sets up the schema, configures cross-session persistence, creates slash commands, learns your tools, and explains how to use it. No terminal commands, no manual file editing.

## Guide structure (for research / deep understanding)

These documents explain what the prompt does and why, step by step:

1. [[01-initial-prompt]] — **The single setup prompt** — paste this and everything happens
2. [[02-obsidian-setup]] — What the agent creates: vault, directories, git, initial files
3. [[03-schema-design]] — The CLAUDE.md schema — conventions, page formats, workflows
4. [[04-global-config]] — Global ~/.claude/CLAUDE.md — cross-session wiki behavior
5. [[05-wiki-categories]] — Page types and directory organization explained
6. [[06-workflows]] — Ingest, query, lint, autonomous capture — the four operations
7. [[07-obsidian-plugins]] — Recommended Obsidian plugins and settings
8. [[08-tips-and-evolution]] — How to evolve the schema over time, tips from usage
9. [[09-skills]] — Custom slash commands: built-in wiki skills and proactive skill creation
10. [[10-tools-and-preferences]] — How user tool preferences are captured and used across sessions
11. [[11-agent-personality]] — User-set communication preferences that persist across sessions
12. [[12-user-profile-memory]] — Conservative learned profile of the user's habits and patterns

Guides 02-12 are reference material. You don't need to read them to use the wiki — the prompt in 01 is self-contained. They exist for research purposes and for understanding the design decisions.
