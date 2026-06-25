# Inspect — prose & markdown (CLAUDE.md, SKILL.md, docs)

Probes for every markdown/prose file in scope. Each maps to a failure-type and an impact-table row.

1. **Count lines** against the cap (CLAUDE.md and any SKILL.md: 100; other docs per the project's doc-discipline caps). Over → *oversized*.
2. **Check every referenced path exists.** For each file, folder, repo, tool, or URL named inside, confirm it's really there. Missing → *dead*.
3. **Verify cited lines.** When a doc cites a line number or quotes a line, open the target and confirm it actually is that line/content — don't just flag the hardcoding, check it. Wrong → *false-claim*.
4. **Spot duplicates.** Walk the folder once for nested same-name folders, identical copies, and the same instruction repeated across global + project CLAUDE.md or a skill → *duplicate*.
5. **Spot conflicts.** Two files (or two lines) giving contradictory instructions → *self-defeating*.
6. **Spot superseded/orphan docs.** `_v\d+` suffixes, draft/archive names, or docs nothing references → *dead*.
7. **Spot empty files** (0 bytes) → *dead*. And **vague filler** ("be helpful", "consider carefully") that costs line budget for no action → *oversized*.
8. **Simplify** (elegance, not a defect). Propose merging duplicated sections, moving bulk into references, flattening nesting — anything shorter and clearer with no loss of function. Recommend, don't just note.

Translate every finding to plain English in the report — the founder is non-technical.
