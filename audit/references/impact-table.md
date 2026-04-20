# Impact table

Fixed-wording impact lines for `/audit` findings. To add a new finding type: append a row. Keep impact to one sentence, plain English, no jargon. SKILL.md section 3 tells Claude how to use this file — that procedure lives there, not here.

## Table

| Finding | Severity bucket | Impact (use verbatim) |
|---|---|---|
| Command in `CLAUDE.md` points to a file that isn't where it says | 🔴 Broken | Next session runs the command, gets "file not found," and can't continue. |
| Folder nested inside another folder with the same name | 🔴 Broken | Two copies of the same files silently drift; one fix doesn't fix both. |
| Two folders doing the same job (different names, same purpose) | 🔴 Broken | Changes have to be mirrored; one side rots. |
| File referenced by `CLAUDE.md`/README doesn't exist at the referenced path | 🔴 Broken | Instructions point at nothing; the next session follows them and fails. |
| Two files give conflicting instructions | 🔴 Broken | Claude follows whichever it reads first; behavior becomes unpredictable. |
| Empty file (0 bytes) | 🔴 Broken | Clutter; also hides whatever was supposed to be there. |
| Session log 3+ days stale with an open issue | 🟡 Out of date | Next session starts with outdated state, may re-diagnose a fixed problem or miss a real one. |
| Hardcoded line number inside a reference | 🟡 Out of date | The reference breaks the next time the target file is edited. |
| Planning/spec doc at root with `_v\d+` suffix or clearly superseded | 🟡 Out of date | Readers assume it's current; decisions get made against stale context. |
| File exists but nothing active references it | 🟡 Out of date | Clutter at minimum; misleading if it looks authoritative. |
| References to files, repos, tools, or URLs that no longer exist | 🟡 Out of date | Instructions point at dead ends; future sessions waste time chasing them. |
| Same instruction duplicated across global and project `CLAUDE.md` (or a skill) | 🟡 Out of date | One version will drift; future sessions may follow the stale copy. |
| Old model names or dead tool references | 🟡 Out of date | Claude may follow obsolete guidance, producing wrong results. |
| `CLAUDE.md` over 100 lines | 🟢 Too big | Claude starts partly ignoring rules past line 100. |
| Any `SKILL.md` over 100 lines | 🟢 Too big | Same — later rules get dropped and the skill becomes unreliable. |
| Two or more files with identical content and a manual sync instruction | 🟢 Too big | They drift the first time the copy step is forgotten. |
| Vague instructions ("be helpful", "be thoughtful", "consider carefully") | 🟢 Too big | Claude can't act on them; they take up line budget for no value. |
| Same content copy-pasted across files | 🟢 Too big | Changes have to happen in multiple places; copies drift. |
