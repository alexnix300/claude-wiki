---
description: Health-check the personal wiki for issues and improvement opportunities
---

Run a health check on the personal wiki.

## Checks to run

1. **Orphan pages** — wiki pages with no inbound links
2. **Broken wikilinks** — `[[wikilinks]]` pointing to nonexistent pages
3. **Stale pages** — frontmatter `updated:` older than 30 days
4. **Missing pages** — concepts/entities/topics mentioned in wikilinks but no dedicated page
5. **Contradictions** — conflicting claims across pages
6. **Frontmatter issues** — missing required fields (type, created, updated, tags)
7. **Empty or thin pages** — less than 3 lines of content below frontmatter
8. **Index sync** — disk files missing from index.md, or index entries to nonexistent pages
9. **Unprocessed sources** — files in `raw/` without summary in `wiki/sources/`
10. **Suggestions** — new pages, merges, investigation ideas, knowledge gaps

## Output

Report findings organized by severity:
- **Fix now**: Broken links, index out of sync, missing frontmatter
- **Review**: Stale pages, thin pages, contradictions
- **Suggestions**: New pages, merges, investigation ideas

Offer to fix issues automatically where possible. Ask before making changes.

## Principles

Linting is itself the **Verify After Edit** principle at wiki-scale — it catches what per-edit verification missed. When fixing issues:
- **Read Before Edit** — read each page before modifying
- **Surgical Updates** — fix only the specific issue; don't reformat unrelated content
- **Verify After Edit** — after fixes, re-run relevant lint checks
