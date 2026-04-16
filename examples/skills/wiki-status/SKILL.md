---
name: wiki-status
description: Show the current state of the personal wiki
disable-model-invocation: true
---

Show the current state of the personal wiki.

## What to report

1. **Overview**: Read `wiki/index.md` and report page counts by category.

2. **Recent activity**: Read the last 10 entries from `wiki/log.md`.

3. **Page stats**: Count markdown files in each wiki subdirectory.

4. **Health quick-check**:
   - Any empty categories?
   - Any pages updated more than 30 days ago? (check frontmatter `updated:` dates)
   - Any obvious orphans? (pages not mentioned in index.md)

5. **Sources pending**: List any files in `raw/` that don't have a corresponding summary in `wiki/sources/`.

## Output format

Present as a clean summary the user can scan in 10 seconds. Use a table for page counts. Flag anything that needs attention.
