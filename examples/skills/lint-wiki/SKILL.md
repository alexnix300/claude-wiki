---
name: lint-wiki
description: Health-check the personal wiki for issues and improvement opportunities
disable-model-invocation: true
---

Run a health check on the personal wiki.

## Checks to run

### 1. Orphan pages
Find wiki pages that have no inbound links — nothing in index.md or other pages links to them.

### 2. Broken wikilinks
Find `[[wikilinks]]` that point to pages that don't exist.

### 3. Stale pages
Pages where `updated:` in frontmatter is more than 30 days old. Flag for review — the info may be outdated.

### 4. Missing pages
Concepts, entities, or topics mentioned in `[[wikilinks]]` across the wiki but that don't have their own page yet.

### 5. Contradictions
Scan for claims that conflict across pages (same entity with different facts, same concept defined differently).

### 6. Frontmatter issues
Pages missing required frontmatter fields (type, created, updated, tags).

### 7. Empty or thin pages
Pages with less than 3 lines of content below the frontmatter.

### 8. Index sync
Pages that exist on disk but aren't listed in index.md, or index entries pointing to non-existent pages.

### 9. Unprocessed sources
Files in `raw/` without a corresponding summary in `wiki/sources/`.

### 10. Suggestions
Based on the wiki's current state, suggest:
- New pages that should exist (frequently mentioned but no page)
- Pages that could be merged (overlapping content)
- Topics worth investigating further
- Sources that would fill knowledge gaps

## Output

Report findings organized by severity:
- **Fix now**: Broken links, index out of sync, missing frontmatter
- **Review**: Stale pages, thin pages, contradictions
- **Suggestions**: New pages, merges, investigation ideas

Offer to fix issues automatically where possible. Ask before making changes.

## Principles

Apply the Maintenance Principles (see `wiki/meta/13-maintenance-principles.md`). Linting is itself the **Verify After Edit** principle at wiki-scale — it catches what per-edit verification missed.

When fixing issues found during lint:
- **Read Before Edit** — read each page before modifying it
- **Surgical Updates** — fix only the specific issue; don't reformat the whole page
- **Verify After Edit** — after fixes, re-run relevant lint checks to confirm resolution
