---
name: audit
description: This skill should be used whenever the founder asks for a health check on a file or folder, or any time they've done a stretch of edits to CLAUDE.md, skills, or settings. Use it on phrases like "audit", "audit this", "audit <name>", "is my setup clean", "check my config", "is anything broken", "what's stale in here" — even when they don't explicitly say "audit." Produces a plain-English report of three problem types (broken, out of date, bloated), with fixed-wording impact per finding, a safe-vs-decision fix split, and offered fixes the founder can approve one-by-one.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Glob
  - Grep
---

# /audit — Quick setup health check

**Purpose.** Give the founder a fast, plain-English health check of a file or folder, then offer to fix the problems without making the founder edit anything by hand.

**How to execute.** Follow sections 1-6 below. Never change files in the initial pass — the report offers fixes, the founder approves each.

## 1. Pick what to audit
- If the founder names a file or folder, check that.
- Otherwise check the current project: top-level files, `CLAUDE.md` (global and project), `.claude/`, `docs/`, `tasks/`, any `SKILL.md`.
- If the path doesn't exist, say so in plain English and stop.

## 2. Look around (don't change anything)
- Read every `CLAUDE.md`, `SKILL.md`, and markdown doc in scope. Count lines.
- For every file path referenced inside them, check whether it exists.
- Walk folders once to spot nested duplicates, empty files, identical copies.

## 3. Classify each finding against the impact table

Read `references/impact-table.md`. For every finding, match it to a row in the **Finding** column and copy the **Impact** wording into the report *verbatim* — never rephrase or invent. Each row also carries a severity bucket (🔴 Broken, 🟡 Out of date, 🟢 Too big) — use that to place the finding in the right report section.

If no row matches: label the impact **"Likely, not verified"** and describe in plain English. Never dramatize. Append the new pattern to `references/impact-table.md` so next audit has a canonical row.

## 4. Sort every finding into one of two lists

- **Safe to fix automatically:** delete empty files, move orphaned docs to archive, swap a hardcoded line number for a marker, trim a clearly-unused section to get under a line limit, fix a command path when the real location is unambiguous.
- **Needs the founder's decision:** which of two duplicates to keep, symlink vs. script for deduped files, whether to close an open session-log item, anything destructive or where the right call depends on context only the founder has.

## 5. Output — exactly this shape

    # Audit: <what was checked>
    Verdict: <PASS | FAIL> — <N broken, N stale, N bloated>

    ## Top 3 fixes (do these first)
    **1. <plain-English headline>**
    <1-2 sentences: what's wrong + impact from the table>
    - What I'll change: <specific before/after, in plain English>
    - How I'll check it worked: <plain-English verify, no raw commands>

    (2 and 3 follow the same shape)

    ## All findings

    **🔴 Broken (will cause problems)**
    1. <one line, plain English, file and line named in words>
    ...

    **🟡 Out of date (will mislead future sessions)**
    ...

    **🟢 Too big (may get partly ignored by Claude)**
    ...

    ## Fix it for you?
    - **Safe — I can fix these right now:** <comma-sep numbers>. Say "fix safe" for all, "fix N" for one.
    - **Needs your call:** <comma-sep numbers>. Say "walk me through" and I'll take you through each.
    - Say "expand N" (e.g. "expand 2, 5") to see why it matters, what I'd do, and how I'd verify.

## 6. Rules for this skill

- Plain English only. No developer jargon. First mention of a file says "line N of <filename>," not `filename:N`.
- Impact must match a row in the table, verbatim. If nothing fits, write "Likely, not verified" + plain-English reason.
- Every finding cites evidence — the file, the line, the fact. No evidence → "Likely, not verified."
- Verdict is `FAIL` if anything lands in 🔴 Broken. Otherwise `PASS`.
- Don't change anything in this pass. The report offers fixes; the founder approves each.
- If a bucket is empty, write "0 problems." Never invent findings.
