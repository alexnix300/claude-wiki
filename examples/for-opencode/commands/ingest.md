---
description: Ingest a source document into the personal wiki
---

Ingest a source into the personal wiki.

**If a file path is provided via `$ARGUMENTS`:** Read that file as the source.
**If no arguments:** Ask the user to paste content or specify a file path.

## Ingest workflow

1. Read the source thoroughly.
2. Discuss 3-5 key takeaways with the user — surface what's interesting, ask what to emphasize.
3. Create a source summary page in `wiki/sources/` with frontmatter (type, created, updated, tags).
4. For each person, org, project, or system mentioned:
   - Check if an entity page exists in `wiki/entities/`. If yes, update it. If no, create it.
5. For each concept, framework, or pattern worth tracking:
   - Check `wiki/concepts/`. Update or create.
6. Update any relevant topic, project, or research pages.
7. Add `[[wikilinks]]` cross-references across all touched pages.
8. Update `wiki/index.md` — add new pages, update summaries.
9. Append an entry to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] ingest | Source Title
   Summary of what was ingested and what pages were created/updated.
   ```

## Page format

```markdown
---
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [relevant, tags]
---

# Source Title

## Summary
Brief overview.

## Key takeaways
- Specific point 1
- Specific point 2

## Details
Detailed notes organized by theme.

## See also
- [[Related Page]]
```

## Principles (Maintenance Principles)

- **Read Before Edit** — read source fully before summarizing; read each entity/concept/topic page before updating
- **Minimal Page Surface** — new entity pages should contain only what the source says, not a full template
- **Surgical Updates** — update only sections this source actually touches
- **Verify After Edit** — at end of ingest: wikilinks resolve, frontmatter valid, `wiki/index.md` lists new pages, `wiki/log.md` has entry
