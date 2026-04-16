# Example: Wiki Vault CLAUDE.md

This is an example of the schema file that lives at the root of the wiki vault. The setup prompt generates this customized to your use case — this file is a general-purpose template for reference.

---

```markdown
# LLM Wiki — Schema & Conventions

This is a personal knowledge base maintained by an LLM. The human curates sources, asks questions, and directs analysis. The LLM writes, updates, and maintains the wiki.

## Directory structure

raw/                  # Immutable source documents
raw/assets/           # Downloaded images and attachments
wiki/                 # LLM-generated and maintained
wiki/index.md         # Content catalog
wiki/log.md           # Chronological operation log
wiki/sources/         # One page per ingested source
wiki/entities/        # People, orgs, clients, systems
wiki/concepts/        # Ideas, frameworks, patterns
wiki/topics/          # Broader thematic synthesis
wiki/analyses/        # Filed query results
wiki/projects/        # Project pages
wiki/personal/        # Routines, goals, habits
wiki/research/        # Tech exploration, learning notes

## Page format

Every wiki page uses this structure:

---
type: source | entity | concept | topic | analysis | project | personal | research
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [list of source filenames]
tags: [relevant tags]
---

# Page Title

Content. Use [[wikilinks]] for cross-references.

## See also

- [[Related Page]]

## Cross-references

- Use Obsidian-style [[wikilinks]] for all internal links.
- Every page should have at least one inbound link.

## Workflows

### Ingest a source
1. Read the source thoroughly.
2. Discuss key takeaways with the human.
3. Create a source summary in wiki/sources/.
4. Create or update entity, concept, and topic pages.
5. Update cross-references, index.md, and log.md.

### Answer a query
1. Read wiki/index.md to find relevant pages.
2. Read relevant pages.
3. Synthesize an answer with [[wikilinks]].
4. Offer to file substantial answers as analysis pages.

### Lint the wiki
1. Check for orphan pages, broken links, stale info.
2. Check for contradictions and missing pages.
3. Suggest improvements. Fix with human approval.

## Style guidelines

- Concise. No filler.
- Bullet points over prose.
- Specific claims over vague summaries.
- Flag contradictions explicitly.
- Attribute claims with [[wikilinks]].
```
