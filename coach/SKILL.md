---
name: coach
description: Weekly AI-collaboration coaching review for Gabriel (non-technical founder). Reviews HOW he worked with Claude this week — not what shipped (that's /retro). Mines his real typed prompts from session transcripts, lessons.md corrections, PR/revert history, and the board; scores six founder skills against countable events; tracks week-over-week trajectory; ends with exactly ONE habit to change. Use when the user says "coach", "/coach", "coaching review", "weekly coaching", "how did I collaborate", "review how I worked", or right after a weekly /retro.
---

# /coach — Weekly AI-Collaboration Coaching Review

/retro answers "what did the repo produce." /coach answers "how well did the founder drive the AI, and what's the next frontier in how he works." Run it weekly, ideally right after /retro.

**Scope boundary:** coach HOW Gabriel works, never WHAT to build. Product priorities belong to Gabriel, the board, and /growth-doctor.

**Arguments:** `/coach` = last 7 days. `/coach 14d` = explicit window.

## Non-negotiable style rules

- **Plain English, zero jargon.** Gabriel is non-technical. First dry run failed with "i don't understand" because the report used terms like "dimensions," "hygiene checklist," "L3/L4." Use the friendly skill names below, explain anything technical in one clause, and lead with the simple story: What you did well → What cost you → Scores → ONE habit.
- **Every observation quotes a real moment** — his actual words, a PR number, a screenshot event. "You scored low on framing" is useless; "your speed spec never said how we'd verify it, and that cost 36 hours" is coaching.
- **No flattery, no padding.** Name vagueness, thrash, skipped gates when you see them. Direct, warm, practical.
- **If the week is thin, say the review is thin.** Never invent patterns from sparse data.
- **Keep the whole report under ~600 words** plus the two tables.

## Step 1 — Mine the evidence (no self-report, no user input)

Run all of these. `<WINDOW_START>` is a **bare date, `YYYY-MM-DD`** (midnight-aligned, e.g. 7 days back) — the same string feeds both `find -newermt` and the jq timestamp comparison; a date with a timezone offset breaks the jq compare. Run steps 1b–1d from the openly repo root — 1a is wrapped in a subshell so it can't change your working directory.

**a. Gabriel's actual typed prompts** from Claude Code transcripts:

```bash
(cd ~/.claude/projects/-Users-gabrielluna-ostaseski-Projects-openly && find . -name '*.jsonl' -newermt '<WINDOW_START>' | while read f; do
  jq -r 'select(.type=="user" and (.message.content|type)=="string" and .isMeta!=true)
    | select(.timestamp >= "<WINDOW_START>")
    | select(.message.content | (startswith("<command-name>") or startswith("<local-command-stdout>") or startswith("<command-message>") or startswith("Caveat:") or startswith("<task-notification>") or startswith("<bash-input>") or startswith("<bash-stdout>")) | not)
    | [.timestamp, (.sessionId[0:8]), (.message.content | gsub("[\\n\\t]";" ") | .[0:400])] | @tsv' "$f" 2>/dev/null
done | sort > /tmp/coach-week-prompts.tsv)
```

**Calibration rules (learned on real data, 2026-07-05):**
- **Dedupe identical message bodies** — fan-out workflows log the same agent prompt many times at the same timestamp. Those are NOT Gabriel typing.
- Long polished "You are a …" mega-prompts may be agent-crafted prompts Gabriel pasted for handoffs/subagents. Count them as evidence of **prompt delegation** (a good leverage habit), not as his raw writing.
- Distinct `sessionId`s per day = the parallelism measure for Step 6.
- Expect roughly 200–400 real typed messages in a full week; if you see <50, the week is thin — say so.

**b. Corrections he had to make:** `git log --since="<WINDOW_START>" -p --format="COMMIT %h %ai %s" -- tasks/lessons.md apps/site-luna/tasks/lessons.md` — added lines are the correction log.

**c. Rework and gates:** `gh pr list --state all --limit 60 --json number,title,state,createdAt,mergedAt` filtered to the window. Look for revert chains, fix-after-fix on one subsystem, PRs held/redrafted. Measure gate discipline HERE (PRs + incident issues), not from transcripts.

**d. Board hygiene:** `gh issue list --state all --limit 100 --json number,title,createdAt,closedAt` — count created+closed same day (agent-narration smell), and flag any open 🚨 alert issues.

**e. Coach history:** read ALL `~/.gstack/projects/openly-roofing-openly/coach/*.json` (oldest first) for the trajectory, the changes ledger, and last week's ONE habit.

## Step 2 — Confirm the read

Pick the 3–5 defining moments of the week. Present them in plain English with his own quotes, then ONE AskUserQuestion: "Did I read these right?" (options: right / mostly-with-corrections / wrong). Fold corrections in before scoring. Keep this message SHORT — the full report comes after confirmation.

## Step 3 — Grade last week's ONE habit first

Did it stick? Yes/no + the evidence (count it in this week's prompts). This always leads the report — it's the loop that compounds. First run: skip.

## Step 4 — Score six founder skills (counts first, then a 1–10)

| Skill (use these names in the report) | Countable anchor |
|---|---|
| Writing clear asks up front | % of substantive asks that say what "done" looks like / how it's verified |
| Keeping things simple | overbuild pushbacks he made vs overbuilt work he accepted |
| Avoiding repeat-loops | thrash loops (3+ redirects on one task); corrections per shipped item; did loops end in a mechanism fix? |
| Catching problems / demanding proof | evidence demands; false "done" claims he caught; "done" accepted without proof that later bit |
| Safety checks that run without you | agreed gates (QA, smoke, design eval, PR) that ran vs were skipped; new gates created; gate violations |
| Recovering when stuck | flailing sessions ridden down vs stopped-and-reframed (fresh session, handoff prompt, root-cause demand) |

Scores are whole numbers, no decimals, no composite index. If a count can't be established honestly, say so instead of guessing.

## Step 5 — The week's main build: six yes/no questions

For the single biggest build of the week: Was "done" defined before building? Was each change small enough to reason about? Was the important path tested (not just the demo path)? Did anyone look at how it fails? Was overbuilding avoided? Could future-Gabriel resume from what's written down? One evidenced line each.

## Step 6 — How much of the week ran without him

Estimate the mix from session overlap, fan-outs, background tasks, and cron/routine artifacts: **you in the chair → one supervised build → parallel sessions → delegated agents → fully automatic**. Name any specific task he babysat by hand that the evidence shows could be delegated or scheduled. The ONE habit (Step 7) may come from this ladder instead of the skills table — some weeks "run it without you" beats "phrase it better."

## Step 7 — Trajectory, headline, ONE habit

- **Trajectory table** across ALL saved weeks (counts, not vibes): e.g. "asks with done-criteria: 60% → 68% → 75%".
- **Changes ledger:** every past week's ONE habit and whether it stuck ("4 of 6 habits became permanent" is the truest improvement measure).
- **Headline:** up / flat / down vs the 4-week rolling average, one sentence why.
- **End with EXACTLY ONE habit for next week** — the highest-leverage one, as a concrete action he can do at the keyboard, never a vibe.
- Tables in chat only. No dashboards (standing founder rule).

## Step 8 — Save the snapshot

Write `~/.gstack/projects/openly-roofing-openly/coach/YYYY-MM-DD.json` (Write tool creates the dir). **JSON only — a creation-guard hook blocks loose .md files**; embed the condensed report as a `"report"` string field so the gstack artifacts sync carries the full review to the private artifacts repo (gbrain-indexable). Schema: copy the shape of the newest existing snapshot (window, counts, scores, hygiene_main_build, leverage, moments, one_change, last_week_change_stuck, changes_ledger, report). Then best-effort sync:

```bash
~/.claude/skills/gstack/bin/gstack-brain-sync --discover-new 2>/dev/null || true
~/.claude/skills/gstack/bin/gstack-brain-sync --once 2>/dev/null || true
```

(The `--discover-new` pass is required — `--once` alone will not pick up a brand-new snapshot file. Verified 2026-07-05. If the sync says the file isn't allowlisted, `~/.gstack/.brain-allowlist` needs `projects/*/coach/*.json` under the USER ADDITIONS marker — already added on the Mac mini.)

## Output order (the report Gabriel sees)

1. Headline trend (or "baseline week" on first run)
2. Did last week's ONE habit stick? (yes/no + proof)
3. What you did well — 2-3 bullets with quotes
4. What cost you — 1-2 bullets with quotes and the hours
5. Scores table (six skills, counts visible)
6. Main build: the six yes/no lines
7. How much ran without you + the next delegation opportunity
8. Trajectory table + changes ledger (skip on first run)
9. **THE ONE HABIT** — bolded, concrete, last thing he reads
