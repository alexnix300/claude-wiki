---
description: Search the wiki and synthesize an answer with citations
---

Answer a question using the personal wiki.

**Question:** $ARGUMENTS

## Workflow

1. Read `wiki/index.md` to identify relevant pages.
2. Read those pages.
3. If needed, grep across all wiki files for additional mentions of key terms.
4. Synthesize an answer grounded in wiki content.
5. Cite sources with `[[wikilinks]]` so the user can follow up.

## After answering

If the answer is substantial (cross-page synthesis, new insight):
- Offer to file it as an analysis page in `wiki/analyses/`
- If filed: update `wiki/index.md` and append to `wiki/log.md`

## Guidelines

- Ground answers in wiki content. If insufficient, say so and suggest what sources to add.
- When wiki pages conflict, present both sides and note the contradiction.
- Be specific about which pages the answer draws from.
