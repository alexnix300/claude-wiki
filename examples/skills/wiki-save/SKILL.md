---
name: wiki-save
description: Manually save a piece of information to the appropriate wiki category
argument-hint: "<info to save>"
disable-model-invocation: true
---

Save information to the personal wiki.

**Input:** $ARGUMENTS

## Workflow

1. Analyze the input to determine what type of information it is:
   - About a project → `wiki/projects/`
   - About a person, org, or client → `wiki/entities/`
   - A personal note (goal, routine, preference) → `wiki/personal/`
   - A research finding or tech evaluation → `wiki/research/`
   - A concept or framework → `wiki/concepts/`
   - A broader theme → `wiki/topics/`

2. Check if a relevant page already exists (read index.md first, then check the target directory).

3. If page exists: **update it** with the new information. Update the `updated:` date.
   If no page exists: **create one** with proper frontmatter.

4. Add `[[wikilinks]]` to connect with related pages.

5. Update `wiki/index.md` if a new page was created.

6. Append to `wiki/log.md`.

## Guidelines

- Ask the user to clarify if you're unsure where to file it.
- Be concise — extract the essential info, don't pad.
- If the input is vague ("save this for later"), ask what specifically to capture.
