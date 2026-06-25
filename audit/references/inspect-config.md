# Inspect — JSON config (settings, permissions, hooks, MCP)

A markdown read-through misses everything here. **Parse the JSON — never eyeball it.** Run these probes, then map each hit to an impact-table row.

1. **Redundant under a broader rule** (*duplicate*). Flag any entry already covered above it:
   - a blanket tool grant (bare `"Bash"`, `"Read"`, `"Edit"`) covers every narrower rule for that tool;
   - an `mcp__<server>__*` wildcard covers every specific `mcp__<server>__tool`;
   - a broad command rule (`Bash(git commit:*)`) covers a narrow one (`Bash(git commit -m ...)`).
   Identical behaviour after removal → safe cut.

2. **Dead one-time commands** (*dead*). Literal `mv`/`cp`/`ln`/`mkdir` with absolute paths, or exact-string command approvals — jobs that already ran once. Safe cut.

3. **Tools that aren't connected** (*dead*). Cross-check every `mcp__<server>__…` entry against what's actually configured: run `claude mcp list` and read `~/.claude.json`. A server that appears nowhere → likely dead. **Verify before cutting** — it may be connected only inside a specific project.

4. **Blanket-grant hole** (*self-defeating*). A bare tool name in `allow` (`"Bash"`) auto-approves *everything* for that tool and nullifies every specific rule and every `ask`/`deny` below it. Usually the real finding — and the real fix.

5. **Gate coherence / precedence trap** (*self-defeating*). Anything in `ask` or `deny` that is *also* matched by an `allow` rule may not fire. Watch money-path commands (push / PR-merge / release / deploy) and destructive ones (delete / force-push). Reliable fix: make sure the gated command is **not** covered by `allow` at all, so the pause/block is guaranteed.

6. **Secrets in plain text** (*risky*). Scan values for API keys, tokens, passwords. Flag; recommend an environment variable. Higher severity if the value sits under any git-tracked or synced path (check before rating).
7. **Simplify** (elegance, not a defect). Propose consolidations that cut lines *and* prompts: merge narrow command rules into one broader rule — but never one that broadens a gated or dangerous command (push / merge / deploy / delete). Recommend boldly; the gates plus the safety envelope keep it safe.

## The one principle that protects speed

A long `allow` list is **not** bloat by itself — every entry is a prompt the founder never sees. Only flag entries that are **dead** (2–3) or **redundant** (1, 5). **Never recommend trimming working approvals to "tidy up"** — that just brings back the prompts the founder is trying to avoid. See [[feedback_permission_philosophy_speed_default]].

## Config-safe verification (pairs with the safety envelope)

JSON breaks silently — one bad comma disables the whole file. For any config edit:

1. **Back up first** even outside git: `cp settings.json settings.json.audit-bak`.
2. **Edit with an assertion**, not by hand: the script counts entries removed/added and refuses to write unless the count matches intent exactly (`assert removed == N`).
3. **Re-verify after:** the file still parses (`python3 -m json.tool`), and structural keys (`hooks`, `enabledPlugins`, `mcpServers`) are unchanged.
4. Parse fails or counts off → restore the backup, report plainly, leave nothing behind.

## Plain-English translations for the report

- wildcard + specific entries → "a master key plus redundant individual keys"
- blanket `"Bash"` grant → "one rule that lets Claude run *anything*, cancelling your safety rules"
- dead one-time command → "a leftover approval for a job that already finished"
- gate-also-in-allow → "a stop sign that won't actually stop anything"
- plaintext secret → "a live password sitting in readable text"
