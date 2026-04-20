---
name: wiki
description: Search and answer a question using the personal wiki
argument-hint: "<your question>"
disable-model-invocation: true
---

Answer a question using the personal wiki.

**Question:** $ARGUMENTS

## Workflow

1. Read `wiki/index.md` to identify relevant pages.
2. Read the relevant pages.
3. If needed, search across all wiki files for additional context:
   - Use Grep to find mentions of key terms across `wiki/`
4. Synthesize an answer using information from the wiki.
5. Cite sources with `[[wikilinks]]` so the user can follow up.

## After answering

If the answer is substantial (more than a few sentences, involves synthesis across multiple pages, or surfaces a new insight):
- Offer to file it as an analysis page in `wiki/analyses/`
- If filed: update index.md and append to log.md

## Guidelines

- Ground answers in wiki content. If the wiki doesn't have enough info, say so and suggest what sources to add.
- When wiki pages conflict, present both sides and note the contradiction.
- Be specific — cite which pages the answer draws from.
