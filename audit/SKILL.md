---
name: audit
description: This skill should be used whenever the founder asks for a health check on a file or folder, or any time they've done a stretch of edits to CLAUDE.md, skills, or settings. Use it on phrases like "audit", "audit this", "audit <name>", "is my setup clean", "check my config", "is anything broken", "what's stale in here" — even when they don't explicitly say "audit." Produces a plain-English report with three problem types (broken, out of date, bloated), fixed-wording impact per finding, honest per-fix risk ratings, and a snapshot → verify → apply → re-verify → auto-rollback safety envelope so applied fixes can't silently break a working setup.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Glob
  - Grep
---

# /audit — Quick setup health check

**Purpose.** Give the founder a fast, plain-English health check of a file or folder, then offer to fix the problems — with honest risk ratings and safety rails — without the founder having to edit anything by hand.

**How to execute.** Follow sections 1-7. Never change files in the initial pass: the report offers fixes, the founder approves each, and the safety envelope (section 6) runs on every applied fix.

## 1. Pick what to audit
- If the founder names a file or folder, check that.
- Otherwise check the current project: top-level files, `CLAUDE.md` (global and project), `.claude/`, `docs/`, `tasks/`, any `SKILL.md`.
- If the path doesn't exist, say so in plain English and stop.

## 2. Look around (don't change anything)
- Read every `CLAUDE.md`, `SKILL.md`, and markdown doc in scope. Count lines.
- For every file path referenced inside them, check whether it exists.
- Walk folders once to spot nested duplicates, empty files, identical copies.
- When a reference cites a line number or line content, *verify* the real line — don't just flag the hardcoding.

## 3. Classify each finding against the impact table

Read `references/impact-table.md`. For every finding, match a row in the **Finding** column and copy the **Impact** wording verbatim — never rephrase or invent. Each row carries a severity bucket (🔴 Broken, 🟡 Out of date, 🟢 Too big) — use that to place the finding in the right report section. If no row matches: label impact **"Likely, not verified"** and describe in plain English. Never dramatize. Append the new pattern to the impact table so next audit has a canonical row.

## 4. Sort + rate every finding

Split findings into two lists:

- **Safe to fix automatically** — examples: delete an empty file, move an orphan doc to archive, swap a hardcoded line number for a search marker, trim a clearly-unused section to get under a line limit, fix a command path when the real location is unambiguous.
- **Needs the founder's decision** — examples: which of two duplicates to keep, symlink vs. script for deduped files, whether to close an open session-log item, *anything where picking the wrong option could break a working system.*

For every item on the "safe" list, rate three things honestly:
1. **Risk of doing it** — near-zero / low / medium / high. Name the specific thing that could break ("nothing automated reads this, it's docs-only" is a valid answer).
2. **Risk of leaving it** — the specific future pain, in plain English.
3. **Confidence it's a real problem** — high / medium / low. Cite evidence (e.g. "I checked: line 85 is real; line 79 in docs is wrong").

If any "safe" item carries medium-or-higher doing-it risk, reclassify it to "needs the founder's decision." Don't hide risk to look useful.

## 5. Output — exactly this shape

    # Audit: <what was checked>
    Verdict: <PASS | FAIL> — <N broken, N stale, N bloated>

    ## Top 3 fixes (do these first)
    **1. <plain-English headline>**
    <1-2 sentences: what's wrong + impact from the table>
    - What I'll change: <specific before/after>
    - How I'll check it worked: <plain-English verify>

    (2 and 3 follow the same shape)

    ## All findings

    **🔴 Broken (will cause problems)**
    1. <one line, plain English, file and line named in words>

    **🟡 Out of date (will mislead future sessions)**
    ...

    **🟢 Too big (may get partly ignored by Claude)**
    ...

    ## Risk check — what each "safe" fix would actually cost or cause

    | # | Fix | Risk of doing it | Risk of leaving it | Confidence it's real |
    |---|---|---|---|---|
    | 1 | <short name> | <rating + why> | <plain-English future pain> | <rating + evidence> |

    ## Fix it for you?
    - **Safe — low-risk, I can apply these:** <numbers>. Say "fix safe" for all, "fix N" for one. Every fix runs through the safety envelope (snapshot → verify → apply → re-verify → auto-rollback).
    - **Needs your call:** <numbers>. Say "walk me through" and I'll take you through each.
    - Say "expand N" for why a finding matters, what I'd do, and how I'd verify.

## 6. Safety envelope (before applying any fix)

When the founder says "fix safe" / "fix N" / "walk me through," read `references/safety-envelope.md` and follow the procedure there exactly. It's a six-step snapshot → verify → apply → re-verify → auto-rollback loop. Never skip steps 2 (verify before) or 4 (re-verify after) — those are the steps that catch silent breakage.

## 7. Rules for this skill

- Plain English only. No developer jargon. First mention of a file says "line N of <filename>," not `filename:N`.
- Impact wording must match a row in `references/impact-table.md`, verbatim. No evidence → "Likely, not verified."
- Every finding cites evidence — the file, the line, the fact.
- Verdict is `FAIL` if anything lands in 🔴 Broken. Otherwise `PASS`.
- Don't change anything in the initial report pass — fixes only happen after the founder says "fix safe" / "fix N" / "walk me through," and only via the safety envelope.
- If a bucket is empty, write "0 problems." Never invent findings.
- Err toward honest risk ratings. If you can't confidently say a fix is near-zero risk, it doesn't belong on the safe list.
