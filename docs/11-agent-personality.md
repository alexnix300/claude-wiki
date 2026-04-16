---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, personality, research]
---

# Step 11 — Agent Personality

## Context

Claude has a generic personality out of the box — diplomatic, helpful, slightly verbose, prone to hedging and apologizing. For some users that's ideal. For others, it's friction. A power user who says "check this code" doesn't want "I'd be happy to help! Let me take a look at your code..." — they want the answer.

The agent personality system lets users define how they want Claude to communicate, and makes those preferences persistent across all sessions.

## How it works

### Storage

User preferences live at `wiki/personal/agent-personality.md`. The file has four sections:

- **Tone** — formal / casual / direct / other
- **Verbosity** — concise / balanced / detailed
- **Proactivity** — just do it / ask first / ask for anything non-obvious
- **Additional guidelines** — free-form rules that accumulate from feedback

### Setup (hybrid)

During initial wiki setup, the agent asks 3 quick questions (tone, verbosity, proactivity) and populates the file with answers. This front-loads the most impactful preferences.

Everything else is learned over time: when the user gives feedback ("stop apologizing", "be more casual", "no emoji"), the agent adds the guideline to the "Additional guidelines" section. The change persists to future sessions.

### Session loop

**Every session start:** Read `wiki/personal/agent-personality.md`. Apply its guidelines throughout the session.

**On user feedback:** When the user comments on communication style, update the file. Don't just change behavior for the current session.

**File wins:** If behavior conflicts with the file, the file wins. Never override preferences silently.

## Examples of guidelines captured over time

After a few sessions of user feedback, the "Additional guidelines" section might look like:

```markdown
## Additional guidelines

- Don't apologize for mistakes. Fix them and move on.
- No emoji unless specifically requested.
- Don't hedge when uncertain — be direct, mark it as a guess if needed.
- Use bullet points and tables over paragraphs.
- Skip preambles ("Let me...", "I'll...") — just do it.
- When showing code, don't explain what the code does unless asked.
```

Each of those came from a single user comment in a prior session. Now they're permanent.

## What this replaces

Before: Users repeat "be more concise" every session. Or they put personality instructions in every prompt. Or they accept the default and just live with it.

After: One file. Read at every session start. Updated when preferences change. Consistent behavior across everything.

## Comment — Why it works

The key insight: users **express** their communication preferences through feedback every session ("stop doing X", "just do Y"). The system captures this feedback rather than letting it evaporate.

Without persistence, every feedback message has a half-life of one conversation. With persistence, one message changes behavior forever.

## Prompt used

> "agent personality (global md)"

The user asked for this feature during wiki system development. Implemented with a hybrid approach: ask 3 minimal questions during setup, then let preferences accumulate from feedback.

## See also

- [[12-user-profile-memory]] — learned profile of the user (companion feature)
- [[10-tools-and-preferences]] — user's tools (similar pattern: persistent preferences in one file)
- [[04-global-config]] — how the global config references these files at session start
