---
name: gotchas
description: "Record pitfalls and workarounds encountered during a session to docs/gotchas.md. Use after encountering errors or unexpected behavior, or before ending a session. Cleanup and staleness checks are handled by the entropy-detector agent."
user-invocable: true
---

# Gotchas Skill

Record "known pitfalls and workarounds" discovered during a session to `docs/gotchas.md`.

> Cleanup and staleness checks of existing entries are handled by the `entropy-detector` agent.

## Steps

1. **Read current state** — Read `docs/gotchas.md` to understand existing categories and entries

2. **Review the session** — Search conversation context for:
   - Error messages and their workarounds
   - Gaps between documentation and actual behavior
   - Type mismatches or runtime constraints
   - Non-intuitive library/framework behaviors
   - Build/deploy pitfalls

3. **Present candidates** — List discovered pitfalls and ask user for confirmation:
   ```
   Found the following gotchas during this session:

   1. [Category] Description -> Workaround
   2. [Category] Description -> Workaround

   Select which to add (by number, or "all").
   ```
   - If no candidates: report "No new gotchas found in this session" and finish

4. **Duplicate check** — Verify no semantic overlap with existing entries. If duplicates exist, propose updating the existing entry

5. **Append** — Add user-selected entries to the appropriate category section:
   - Add to existing category if applicable
   - Create new section (`## Category Name`) if no match
   - Group entries by relevance within categories

## Format Convention

Maintain consistent style with existing entries:

```markdown
- Brief description of the problem -> Solution/workaround
```

## Notes

- Never add entries without user confirmation
