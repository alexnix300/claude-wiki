# Set up my personal wiki

**Reference repo:** https://github.com/alexnix300/claude-wiki

Before you begin, fetch the latest version of this repo. Read the `setup-prompt.md`,
`README.md`, `docs/`, and `examples/` to get the most up-to-date structure,
conventions, skill definitions, and config patterns. Use what you find there as
the ground truth — if anything below conflicts with the repo, the repo wins.

I want you to set up a personal knowledge base (wiki) that you will build and
maintain for me. Here's how it works:

## What this is

Instead of me writing notes manually, you (the LLM) will maintain a structured
wiki of interlinked markdown files. When I share information — articles, meeting
notes, ideas, anything — you read it, extract what matters, and integrate it into
the wiki. You update existing pages, create new ones, add cross-references, and
keep everything consistent. The knowledge compounds over time.

I browse the wiki in Obsidian (a free markdown editor). You do all the writing
and maintenance. I direct the work and ask questions.

## What I need you to do right now

### 1. Ask me about my use case

Before building anything, ask me:
- What's this wiki for? (work, personal life, research, reading, a mix?)
- What kinds of things will I be tracking? (projects, clients, goals, a
  specific topic, general knowledge?)
- Where should the wiki live on my computer? (suggest ~/Documents/WIKI as a
  default, but let me choose)

Use my answers to customize the wiki categories and structure. For example:
- If I say "work" → include projects/, clients/, meeting-notes/
- If I say "research on AI" → include papers/, concepts/, open-questions/
- If I say "personal" → include goals/, journal/, health/, habits/
- If I say "mix of everything" → include a broad set of categories

Don't assume — ask, then build what fits.

### 2. Create the wiki

Based on my answers, create:

**The vault directory** with:
- `raw/` — where I'll drop source documents (articles, PDFs, transcripts). You
  read from here but never modify these files.
- `raw/assets/` — for downloaded images and attachments
- `wiki/` — your workspace. You create and maintain all files here.
- Subdirectories inside `wiki/` based on my use case (e.g. `wiki/projects/`,
  `wiki/entities/`, `wiki/research/`, etc.)

**Two special files:**
- `wiki/index.md` — a catalog of every page in the wiki, organized by category,
  with one-line summaries. You update this whenever pages are created or changed.
- `wiki/log.md` — a chronological record of what's been done (ingests, updates,
  queries). Append-only.

**The schema** (`CLAUDE.md` at the vault root) — a file that tells you (the LLM)
how the wiki is structured, what conventions to follow, what page format to use,
and what workflows to run. This is the rulebook. You and I will evolve it over
time.

**Git** — initialize a git repo in the vault for version history.

### 3. Set up cross-session persistence

This is important: I want the wiki to be active in EVERY Claude Code session,
not just when I'm in the wiki directory. To do this:

Create or update `~/.claude/CLAUDE.md` (the global config) with instructions
that tell you to:

**At the start of every session:**
- Read the wiki index and recent log entries to load context

**During every session:**
- Autonomously capture information worth persisting — project updates, people
  mentioned, decisions made, research findings, personal info I share
- File it in the appropriate wiki category
- Don't ask permission for routine updates — just do it naturally

**At the end of every session:**
- Review what was discussed
- Update or create relevant wiki pages
- Update the index and log

Include clear rules about what to capture (decisions, context, rationale,
status changes) vs. what to skip (transient debugging, one-off fixes).

### 4. Create wiki skills (slash commands)

Create these slash commands so I can use the wiki with simple one-word commands:

- `/ingest <file>` — Process a source document into the wiki (read it, discuss
  takeaways, create/update pages, cross-reference everything)
- `/wiki-status` — Show a quick dashboard of the wiki's state (page counts,
  recent activity, pending sources)
- `/lint-wiki` — Run a full health check (orphan pages, broken links, stale
  info, contradictions, suggestions for improvement)
- `/wiki <question>` — Ask a question and get an answer grounded in wiki
  content, with citations
- `/wiki-save <info>` — Save a piece of information to the right wiki category

Each skill should be a directory under `~/.claude/skills/` with a `SKILL.md`
file containing the instructions you follow when the command is invoked.

Every skill should reference four **Maintenance Principles** for wiki editing
(put them in a `## Principles` section in each SKILL.md):

1. **Read Before Edit** — Always read a page fully before editing; check the
   index for similar pages before creating
2. **Minimal Page Surface** — Start pages with minimum sections needed; no
   preparatory scaffolding
3. **Surgical Updates** — Edit only what needs changing; preserve existing
   structure; state explicitly if a full rewrite is warranted
4. **Verify After Edit** — Frontmatter parses, wikilinks resolve, index
   updated if new page, log entry appended

Same four principles go into the global `~/.claude/CLAUDE.md` as a section,
so they apply to autonomous capture too — not just slash-command invocations.

Also set up proactive skill identification: as we work together over time,
notice when I repeat the same type of request and suggest creating a skill for
it. Keep track of all custom skills in `wiki/personal/custom-skills.md`.

### 5. Remember my tools and preferences

Create a page at `wiki/personal/tools-and-preferences.md` that tracks my
preferred tools, apps, and workflows. From our very first interaction, whenever
I mention a tool I use (editor, framework, hosting, design tool, etc.),
save it to this page automatically — don't ask, just note it.

Then always use my preferred tools in suggestions and commands. If I use pnpm,
never suggest npm. If I use Vercel, suggest Vercel-compatible configs. If I
switch tools later, update the page.

Also ask me during initial setup:
- What tools/apps do you use most? (editor, design tool, project management,
  communication, hosting, etc.)
- Any strong preferences? (tabs vs spaces, dark mode, specific frameworks, etc.)

### 6. Ask me about agent personality

Ask me 3 quick questions about how I want you to communicate:

1. **Tone**: casual / professional / direct no-nonsense / other
2. **Verbosity**: concise / balanced / detailed
3. **Proactivity**: just do it / ask first / ask for anything non-obvious

Save answers to `wiki/personal/agent-personality.md` with these sections:
Tone, Verbosity, Proactivity, Additional guidelines.

Going forward, whenever I give you feedback on communication style
("be more concise", "stop apologizing", "no emoji", "less hedging"),
add the rule to the "Additional guidelines" section so it persists across
sessions. The file is a contract I've set for how you should behave — it
wins over default behavior.

Add instructions to the global `~/.claude/CLAUDE.md` to:
- Read this file at every session start
- Apply its guidelines throughout the session
- Update it when I give communication feedback

### 7. Set up user profile / interaction memory

Create `wiki/personal/user-profile.md` with these empty sections:
- Recurring needs (what I ask about most)
- Working patterns (time of day, session style)
- Communication style (direct, exploratory, technical, casual)
- Domain expertise (where I'm expert vs learning)
- Interests (topics I return to)

Over time, fill this in silently based on patterns you observe across
sessions. **Be conservative:**
- Only record patterns observed **2+ times**
- Update in place — overwrite, don't accumulate history
- No speculation, no armchair psychology, no trait inferences beyond
  observable behavior
- If I correct you ("I don't usually..."), update immediately

Use what you learn to adapt how you communicate with me — but don't
reference the profile explicitly unless I ask. It should shape behavior
silently.

Add instructions to the global `~/.claude/CLAUDE.md` to:
- Read this file at every session start to inform how to communicate with me
- Update it conservatively as patterns emerge
- Keep it factual and useful, not invasive

### 8. Show me how to use it

After setup, explain in plain language:
- How to add a source (drop a file in raw/ and tell you to process it, or just
  type `/ingest`)
- How to ask questions against the wiki (`/wiki`)
- How to check wiki health (`/wiki-status`, `/lint-wiki`)
- How to save info quickly (`/wiki-save`)
- How to browse the wiki in Obsidian (graph view, backlinks, etc.)
- What happens automatically each session (context loading, autonomous capture,
  tool preference learning)
- What skills are available and how to invoke them

Keep the explanation simple — assume I'm not technical. No jargon.

## Page format

Use this format for all wiki pages:

```markdown
---
type: (e.g. project, entity, concept, source, etc.)
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [relevant, tags]
---

# Page Title

Content with [[wikilinks]] to other pages.
```

## Key principles

- You own the wiki. I never need to edit files manually.
- Use [[wikilinks]] for all cross-references (Obsidian renders these as
  clickable links).
- Keep pages focused — one topic/entity/project per page.
- Be concise. Bullet points over paragraphs. Specifics over vague summaries.
- When new info contradicts old info, flag it explicitly.
- The wiki should get more valuable over time, not just bigger.
