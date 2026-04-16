---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, tools, preferences, research]
---

# Step 10 — Tools & Preferences Tracking

## Context

Every user has preferred tools — their editor, their package manager, their hosting provider, their communication apps. If the LLM doesn't remember these, it suggests wrong tools, wrong commands, wrong workflows every session. The fix: capture preferences on first mention and use them everywhere.

## How it works

### Capture (automatic)

When the user mentions a tool or preference naturally in conversation, the LLM saves it to `~/Documents/WIKI/wiki/personal/tools-and-preferences.md`. No explicit "remember this" needed.

**Examples of what triggers a save:**
- "Let me push this to Vercel" → saves: Hosting: Vercel
- "I'll open this in Figma" → saves: Design: Figma
- "Run it with pnpm" → saves: Package manager: pnpm
- "I use VS Code for everything" → saves: Editor: VS Code
- "We communicate on Slack" → saves: Communication: Slack

### Storage format

A single wiki page organized by category:

```markdown
# Tools & Preferences

## Development
- **Editor**: VS Code
- **Terminal**: iTerm2
- **Package manager**: pnpm
- **Language**: TypeScript (primary), Python (scripts)

## Design
- **Design tool**: Figma
- **Prototyping**: Figma

## Communication
- **Team chat**: Slack
- **Video calls**: Google Meet

## Workflow
- **Branching**: feature branches off main
- **Commits**: conventional commits
- **CI/CD**: GitHub Actions

## Environment
- **OS**: macOS
- **Shell**: zsh
```

### Usage (automatic)

Before suggesting tools, commands, or configurations, the LLM checks this file:
- If user uses pnpm → never suggest `npm install`, always `pnpm add`
- If user uses Vercel → suggest Vercel-compatible configurations
- If user uses Figma → reference Figma when discussing design workflows
- If user prefers tabs → don't reformat with spaces

### Updates

When preferences change ("I switched from npm to Bun"), the LLM updates the file — replaces the old value, doesn't add a second entry. The file is always current state, not a history.

## What gets captured

| Category | Examples |
|----------|----------|
| Development | Editor, IDE, terminal, shell, languages, frameworks, package manager, linter, formatter |
| Design | Design tool, prototyping tool, image editor |
| Communication | Chat, video, email, project management |
| Services | Hosting, CI/CD, monitoring, analytics, auth provider |
| Workflow | Branching strategy, commit conventions, testing approach, code review process |
| Environment | OS, shell, machine specs, display preferences (dark mode, etc.) |
| Opinions | Strong preferences ("I hate ORMs", "always use TypeScript") |

## What doesn't get captured

- One-time tool usage ("I just tried X") — only save if it's their regular tool
- Generic tool mentions in source documents — only save the user's own preferences
- Transient environment details ("I'm on slow wifi today")

## Integration with skills

Tools preferences inform skill creation. If the user uses:
- **Linear** for project management → a `/sync-linear` skill might be suggested
- **Figma** for design → a `/figma-review` skill might be suggested
- **Notion** for docs → ingest workflows could be adapted for Notion exports

The tools file is a signal for what skills would be most valuable.

## Prompt used

> "tools use - preferable user's tools should be remembered from first touch so claude always knows about them"

This prompted:
1. A tools-and-preferences wiki page (`wiki/personal/tools-and-preferences.md`)
2. Instructions in global `~/.claude/CLAUDE.md` to capture and use preferences automatically
3. Rules about when to capture (natural mention) vs. when to skip (one-time use)

## Comment — Why this matters

The difference between a generic assistant and a personalized one is whether it remembers how you work. A user who says "deploy this" should get a Vercel command if they use Vercel, a Railway command if they use Railway, or a bare `ssh` command if they self-host. Without preference tracking, the LLM guesses — and guessing wrong erodes trust fast.

First-touch capture is the key insight: don't make the user fill out a preferences form. Just listen and learn. By session 3-4, the LLM knows their entire toolchain without ever having been explicitly told.
