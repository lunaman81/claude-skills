---
name: audit
description: This skill should be used whenever the founder asks for a health check on a file or folder, or any time they've done a stretch of edits to CLAUDE.md, skills, or settings. Use it on phrases like "audit", "audit this", "audit <name>", "is my setup clean", "check my config", "is anything broken", "what's stale in here" — even when they don't explicitly say "audit." Produces a plain-English report with three problem types (broken, out of date, bloated), fixed-wording impact per finding, honest per-fix risk ratings, and a snapshot → verify → apply → re-verify → auto-rollback safety envelope so applied fixes can't silently break a working setup. NOT for shortening prose docs like specs, roadmaps, or strategy (that's /doc-simplifier) and NOT a code-quality dashboard (that's /health) — this audits config, skills, and setup for breakage.
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
- Otherwise check the current project: top-level files, `CLAUDE.md` (global and project), `.claude/` (including `settings.json`), `docs/`, `tasks/`, any `SKILL.md`.
- If the path doesn't exist, say so in plain English and stop.

## 2. Look around (don't change anything)
The skill works on any file type. Match each file to its inspection guide, and reason in failure-types — not a fixed checklist.

- **Pick the guide per file:**
  - prose & markdown (`CLAUDE.md`, `SKILL.md`, docs) → `references/inspect-docs.md`
  - JSON config — settings, permissions, hooks, MCP (`settings.json`) → `references/inspect-config.md`
  - any other type → reason from the six failure-types in section 3.
- **Two habits for every file:**
  - **Parse, don't eyeball.** Open structured files as data; never judge a config by skimming it.
  - **Verify against the live system.** A reference is only real if the target exists; a tool grant only counts if the tool is connected; a cited line is only right once you've read it.
- **Judge against purpose, not just freshness.** Ask what the file is *for*, and whether anything in it defeats that job — a rule that cancels a safety rule, a gate that doesn't gate.

## 3. Classify each finding against the impact table

Read `references/impact-table.md`. Every row is an instance of one of six **failure-types** — **dead** (points at something gone), **duplicate** (already covered elsewhere), **self-defeating** (one rule cancels another), **false-claim** (says something checkable that's wrong), **risky** (e.g. a secret in the open), **oversized**. Match each finding to a row and copy the **Impact** wording verbatim — never rephrase. Use the row's severity bucket (🔴 Broken, 🟡 Out of date, 🟢 Too big) to place it. If a finding fits a type but no row matches: label it **"Likely, not verified,"** describe it plainly, and **append a new row** so the next audit catches it automatically. Never dramatize.

## 4. Sort + rate every finding

Split findings into two lists:

- **Safe to fix automatically** — e.g. delete an empty file, move an orphan doc to archive, swap a hardcoded line number for a search marker, remove a permission already covered by a broader rule, trim a clearly-unused section under a line limit.
- **Needs the founder's decision** — which of two duplicates to keep, whether a disconnected tool is truly unused, anything touching the money path, *anything where picking wrong could break a working system.*

**Also run a simplification pass.** Beyond defects, actively hunt for ways the setup could be simpler or more elegant with no loss of function — consolidate narrow rules into one broader rule (never broadening a gated or dangerous command), merge duplicated structure, move bulk into references, flatten needless nesting. List these as **recommended simplifications**, each with a clear recommendation. Don't stay silent because nothing is "broken," and don't bury a good simplification behind excessive caution — if it's reversible and you can verify it, propose it and recommend it boldly.

For every "safe" item, rate three things honestly:
1. **Risk of doing it** — near-zero / low / medium / high. Name the specific thing that could break.
2. **Risk of leaving it** — the specific future pain, in plain English.
3. **Confidence it's a real problem** — high / medium / low. Cite evidence.

If any "safe" item carries medium-or-higher doing-it risk, reclassify it to "needs the founder's decision." Don't hide risk to look useful.

## 5. Output

Produce the report in the exact shape in `references/report-format.md`: a verdict line, the top-3 fixes, all findings in the three buckets, the risk-check table, and the "fix it for you?" offer.

## 6. Apply fixes — safety envelope

**Triggers.** **"execute"** or **"build"** applies the *full recommended set* — every safe fix and every recommended simplification — pausing only on items marked "needs your decision." **"fix safe"** applies just the safe fixes; **"fix N"** one item; **"walk me through"** the judgment calls.

For each change, read `references/safety-envelope.md` and follow it exactly — snapshot → verify → apply → re-verify → auto-rollback — re-verifying *between* changes, never batching blind. For JSON config edits, also use the config-safe verification in `references/inspect-config.md` (back up, assert exact counts, re-validate the file parses). Never skip the verify-before or re-verify-after steps — those catch silent breakage.

## 7. Rules for this skill

- Plain English only. No developer jargon. First mention of a file says "line N of <filename>," not `filename:N`.
- Impact wording must match a row in `references/impact-table.md`, verbatim. No evidence → "Likely, not verified."
- Every finding cites evidence — the file, the line, the fact.
- Verdict is `FAIL` if anything lands in 🔴 Broken. Otherwise `PASS`.
- Don't change anything in the initial report pass — fixes happen only after "fix safe" / "fix N" / "walk me through," and only via the safety envelope.
- If a bucket is empty, write "0 problems." Never invent findings.
- Err toward honest risk ratings. If you can't confidently say a fix is near-zero risk, it doesn't belong on the safe list.
- Default to proposing simplifications boldly — the safety envelope, not silence, is what keeps bold changes safe.
