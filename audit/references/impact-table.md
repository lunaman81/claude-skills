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
| Permission already granted by a broader rule above it (a specific command or tool sitting under a wildcard or blanket grant that already covers it) | 🟢 Too big | The extra rule does nothing — it just makes the list longer and harder to read. |
| A blanket "do anything" grant for a whole tool (e.g. allowing all shell commands) sitting above narrower safety rules | 🔴 Broken | It silently overrides every careful rule below it, so the safety rules stop meaning anything. |
| A one-time setup command still on the approved list long after it ran (move/copy/make-folder with a fixed path) | 🟡 Out of date | Dead leftover that can never run again; clutter that hides what's still real. |
| A permission for a tool or integration that isn't connected anywhere | 🟡 Out of date | It grants access to something that doesn't exist, so nobody can tell what's actually in use. |
| A command that should pause for approval but also sits on the auto-approve list | 🔴 Broken | The pause may never happen, so a risky or go-live action can run without your okay. |
| A password, API key, or token saved in plain text inside a config file | 🔴 Broken | Anyone who can read the file — or any synced copy of it — gets a working secret. |
| The same command approved twice, once narrow and once broad (e.g. "commit with a message" plus "commit anything") | 🟢 Too big | The narrow one is pointless; the broad one already covers it. |
