---
type: meta
created: 2026-04-16
updated: 2026-04-16
tags: [setup, obsidian, plugins, research]
---

# Step 7 — Obsidian Plugins & Settings

## Context

Obsidian has a rich plugin ecosystem. A few plugins make the wiki significantly more useful. All are optional — the wiki works without them.

## Recommended settings

### Files and links

- **Attachment folder path**: Set to `raw/assets/` so downloaded images go to a consistent location
- **New link format**: "Shortest path when possible" (keeps wikilinks clean)
- **Default location for new notes**: "Same folder as current file" or vault root

### Hotkeys

- Bind "Download attachments for current file" to `Ctrl+Shift+D` (or your preference). Useful after clipping web articles — downloads all remote images to local disk.

## Recommended plugins

### Core plugins (built-in, just enable)

- **Graph view** — Visualize connections between pages. The best way to see the shape of your knowledge base. Shows hubs (pages with many connections) and orphans (isolated pages).
- **Tags** — Browse all tags used across the wiki. Useful with the frontmatter tags convention.
- **Outgoing links / Backlinks** — See what a page links to and what links to it. Essential for navigating the wiki.

### Community plugins

#### Dataview

**What it does:** Runs queries over page frontmatter. If your wiki pages have YAML metadata (type, tags, dates), Dataview can generate dynamic tables and lists.

**Example queries you can embed in any page:**

```dataview
TABLE type, updated, tags
FROM "wiki/projects"
WHERE type = "project"
SORT updated DESC
```

```dataview
LIST FROM #active-project
```

**Why useful:** Dynamic dashboards. A "Projects Overview" page that auto-updates as project pages change.

#### Obsidian Web Clipper (browser extension)

**What it does:** Converts web articles to markdown with one click. Saves them to a configured folder in your vault.

**Setup:** Install the browser extension → configure it to save to `raw/` in your vault.

**Why useful:** Fastest way to get web sources into your raw collection for ingestion.

#### Marp (optional)

**What it does:** Renders markdown as slide decks. Obsidian has a Marp plugin.

**Why useful:** You can ask the LLM to generate a presentation from wiki content, and view it directly in Obsidian.

#### Templater (optional)

**What it does:** Template system for new pages.

**Why useful:** If you create pages manually (not just via LLM), templates ensure they follow the frontmatter conventions.

## Comment — Start minimal

Don't install everything at once. Start with:
1. Graph view (built-in, just enable)
2. Backlinks panel (built-in)
3. Web Clipper (for sourcing)

Add Dataview once you have 20+ pages and want dashboards. Add Marp if you need presentations. The wiki works fine with zero plugins — they just make browsing nicer.
