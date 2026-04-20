---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, profile, memory, research]
---

# Step 12 — User Profile / Interaction Memory

## Context

Every Claude Code session starts from zero knowledge of who the user is. They might be a senior engineer or a beginner. They might work best with bullet points or with prose. They might ask the same type of question every week. None of this is remembered.

The user profile system fixes this. It's a learned memory of the user's habits, needs, and patterns — accumulated silently across sessions — used to adapt how the agent communicates.

## How it works

### Storage

A profile page at `wiki/personal/user-profile.md` with five sections:

- **Recurring needs** — what they ask about most often
- **Working patterns** — time of day, session style, context-switching behavior
- **Communication style** — direct / exploratory / technical / casual
- **Domain expertise** — where they're expert vs learning
- **Interests** — topics they return to, passions they mention

### Updates (conservative by design)

The profile is intentionally **cautious**:

- **Only record patterns observed 2+ times.** One-offs don't get recorded — they might be noise.
- **Update in place.** The profile is current state, not history. If "learning: Rust" becomes "intermediate: Rust," overwrite.
- **No speculation.** If the user is brief in two conversations, record "prefers concise responses" — not "probably an introvert."
- **No armchair psychology.** Never infer traits beyond observable behavior.
- **Correction-first.** If the user says "I don't usually work at night," overwrite immediately.

### Usage

At session start, the agent reads the profile and uses it to:

- **Adapt communication style** — match their verbosity, technicality, directness
- **Anticipate needs** — if pattern says "asks about X after Y," surface X proactively
- **Calibrate explanations** — don't over-explain to an expert, don't assume knowledge with a beginner

The agent **does not reference the profile explicitly** unless asked. It's invisible scaffolding, not a performance.

## Why this differs from agent-personality

These two files are related but distinct:

| File | What it is | How it's populated |
|------|-----------|---------------------|
| `agent-personality.md` | What the user **tells** Claude about how to behave | Setup questions + explicit feedback |
| `user-profile.md` | What Claude **observes** about the user over time | Silent pattern recognition |

Agent-personality is a contract the user sets. User-profile is a mental model the agent builds.

They reinforce each other. If the user sets "concise" in personality but then asks detailed questions in practice, the profile captures that tension — the agent might ask for clarification rather than silently misjudging.

## Examples

### After 5 sessions, a profile might look like:

```markdown
## Recurring needs
- Wiki system improvements (skills, global config edits)
- Open source publishing decisions

## Working patterns
- Mostly afternoon/evening sessions
- Iterative style — asks follow-up questions based on previous responses
- Switches context quickly between unrelated threads

## Communication style
- Short, direct requests. Often 1-2 sentences.
- Uses lowercase, occasional typos — informal but technical.
- Expects the agent to infer context and just execute.

## Domain expertise
- Expert: LLM tooling, product design
- Learning: Rust, distributed systems

## Interests
- Personal knowledge management
- Open source tools for solo developers
```

This is the kind of profile that emerges after 10-20 sessions, not after one.

## Privacy considerations

The profile lives in the user's own wiki. It never leaves the local machine unless the user explicitly shares the wiki (e.g. pushing to a public git repo). The open-source repo `agent-wiki` ships with an empty template — no one else's profile data is ever published.

For users who don't want this feature, it's opt-out: delete the file and the global config will gracefully skip it.

## Comment — Why conservative is the right default

Aggressive inference feels creepy fast. "I noticed you've been working late — everything OK?" is not helpful; it's intrusive.

Conservative inference stays useful without being strange. "The user prefers bullet points over paragraphs" is observable, factual, and actionable. "The user seems stressed this week" is not observable, speculative, and boundary-crossing.

The rule: record what they do, not what you think they feel.

## Prompt used

> "user's habits - memory of interactions - remembers main user's needs, habits, requests and user's personality to adaptively talk to them (used in global md)"

Implemented with conservative scope — only factual, observable patterns recorded 2+ times.

## See also

- [[11-agent-personality]] — user-set communication preferences (companion feature)
- [[10-tools-and-preferences]] — user's tools (similar pattern: persistent personal data)
- [[04-global-config]] — how the global config references these files at session start
