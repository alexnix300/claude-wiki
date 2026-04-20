---
name: ingest
description: Ingest a source document into the personal wiki
argument-hint: "<file-path or paste content directly>"
disable-model-invocation: true
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

## Guidelines

- Be concise. Bullet points over prose.
- Prefer specific claims over vague summaries.
- Use `[[wikilinks]]` for every cross-reference.
- If the source contradicts existing wiki content, flag it explicitly on both pages.

## Principles

Apply the Maintenance Principles (see `wiki/meta/13-maintenance-principles.md`):

- **Read Before Edit** — read the source fully before summarizing; read each entity/concept/topic page before updating it
- **Minimal Page Surface** — new entity pages should contain only what the source actually says, not a full template
- **Surgical Updates** — update only the sections in existing pages that this source actually touches; don't restructure unrelated content
- **Verify After Edit** — at end of ingest: confirm all new wikilinks resolve, frontmatter is valid, `wiki/index.md` lists new pages, `wiki/log.md` has an entry
