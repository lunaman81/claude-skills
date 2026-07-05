---
name: coach
description: Weekly AI-collaboration coaching review for Gabriel (non-technical founder). Reviews HOW he worked with Claude this week — not what shipped (that's /retro) — and upgrades BOTH loops every week; the human (ONE habit) and the system (ONE mechanism — hook/rule/job — approved, built, and verified in the same session). Mines his real typed prompts from session transcripts, lessons.md corrections, PR/revert history, and the board; scores six founder skills against countable events; grades habits by behavior counts and past upgrades by their deterministic logs. Use when the user says "coach", "/coach", "coaching review", "weekly coaching", "how did I collaborate", "review how I worked", or right after a weekly /retro.
---

# /coach — Weekly AI-Collaboration Coaching Review

/retro answers "what did the repo produce." /coach answers "how well did the founder drive the AI, and what's the next frontier in how he works." Run it weekly, ideally right after /retro.

**The two loops (decided 2026-07-05).** Every failure pattern found gets triaged with one question: *can a mechanism prevent this?*

- **YES → it becomes the week's SYSTEM UPGRADE** — a hook, rule, scheduled job, or locked prompt: spec'd by the coach, approved by Gabriel, **built and tested in the same session**, shipping with a deterministic proof-of-life (a log or test the next /coach reads without judgment). Precedent: the board flood was fixed by the inflow-rule hook, not by remembering; the sheet corruption by by-name writes. Mechanisms don't forget.
- **NO (judgment, taste, when-to-stop) → it becomes the week's HABIT** for Gabriel, graded next week by behavior counts.

Rules: exactly ONE of each per week, no more. Upgrades are capped at ~30 minutes of build — bigger means over-built; shrink it. **Removal counts as an upgrade** — if a past mechanism causes friction, the week's proposal may be to delete it. Upgrades touching live-site code follow the normal PR path; hooks/config/skills commit directly (Tier 2).

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

## Step 3 — Grade last week's ledger first (both loops)

This always leads the report — it's the loop that compounds. First run: skip.

- **The habit (probabilistic):** did it stick? Yes/no + the behavior count from this week's prompts.
- **Each past system upgrade (deterministic):** read its proof-of-life and report the numbers, not an opinion. E.g. the done-line gate: `jq -r .verdict ~/.gstack/projects/openly-roofing-openly/coach/done-line-gate.jsonl` filtered to the window → "34 build asks metered, 30 pass / 4 gated." A cron job: runs completed / expected. A rule: violations counted in transcripts. **An upgrade whose log is empty or missing is BROKEN or unused — say so plainly; silent mechanisms are the failure mode.** If an upgrade caused friction (false fires, founder annoyance in the transcripts), propose removing or tuning it as this week's upgrade.

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
- **End with the week's PAIR, triaged mechanism-first:**
  1. **ONE system upgrade** — the highest-leverage failure pattern a mechanism can kill. Present as: the pattern (with evidence) → the mechanism (hook/rule/job/locked prompt, ≤30-min build) → its proof-of-life (the exact log/test next Friday reads). Get Gabriel's approval via AskUserQuestion, then **build it, test it deterministically (sample inputs → expected outputs), and append it to the ledger — in this same session.** If he declines, record "declined" in the ledger; don't re-pitch it weekly.
  2. **ONE habit** — only for what no mechanism can do (judgment, taste, when to stop). Concrete keyboard action, never a vibe.
- Tables in chat only. No dashboards (standing founder rule).

## Step 7.5 — Goals delta (5 min, added 2026-07-05 by goals-system build)

Read the life-goals page: `gbrain get goals/index` (DATABASE_URL + gbrain are on PATH via ~/.zshenv). Skip silently if the page is missing or still has `[PLACEHOLDER` content (system not yet live). Otherwise:

1. **Touched/untouched per domain** — from BOTH worlds: this week's brain activity (`gbrain list -n 30`, look at last-7-days meetings/, decisions/, finance/, emails/ pages — this captures Hermes-side work) AND this week's Claude Code session topics (already mined in Step 1). One line per domain: touched (evidence) or untouched.
2. **ONE drift flag** — the domain that is both least-touched and worst-scoring. Ask: act Monday, or consciously defer to the monthly review? Record the answer on the page's section ("deferred to monthly YYYY-MM-DD") so next week doesn't re-nag.
3. **Update moved numbers** — any Current he confirms changed, write to `goals/index` via `gbrain put` with source `(source: gabriel, weekly)` and refresh that section's "Last reviewed".
4. **Never** rewrite targets, rules, or mission here — that's /goals (monthly) territory. If the page's `next_review_due` has passed, say so plainly.
5. **Intervention rule (added 2026-07-05):** if his answer to the drift flag is "act", pin it as ONE concrete, external, dated action (a calendar block, a message to a named person, a canceled commitment, a decision) — note it on that domain's section, and check it FIRST in next week's delta before flagging anything new. Observations in this step follow the no-generic-advice rule: cite his own numbers, rules, or past words — never "focus more on X".

## Step 8 — Save the snapshot, THEN deliver the report (order is load-bearing)

**⚠️ Display rule — the #1 bug this skill has already had (2026-07-05, twice):** in Claude Code, text written between tool calls is often NOT shown to the user. The FULL report must be the very LAST message of the turn, with ZERO tool calls after it. So: save the snapshot and run the sync FIRST, and only then print the report. Never print the report and then save/sync — the report vanishes behind the tool activity and the user sees "it ran something but displayed nothing." Same rule for Step 2: keep the moments message short and let AskUserQuestion be the only thing that follows it.

Write `~/.gstack/projects/openly-roofing-openly/coach/YYYY-MM-DD.json` (add a `-14d` style suffix for non-default windows so the weekly baseline never gets overwritten). **JSON only — a creation-guard hook blocks loose .md files**; embed the condensed report as a `"report"` string field so the gstack artifacts sync carries the full review to the private artifacts repo (gbrain-indexable). Schema: copy the shape of the newest existing snapshot (window, counts, scores, hygiene_main_build, leverage, moments, one_change, last_week_change_stuck, changes_ledger, report). Ledger entries carry both loops: `{"week", "type": "habit"|"upgrade", "change", "stuck": "yes|no|pending|declined", "proof": "<where its log/test lives — required for upgrades>"}`. Then best-effort sync:

```bash
~/.claude/skills/gstack/bin/gstack-brain-sync --discover-new 2>/dev/null || true
~/.claude/skills/gstack/bin/gstack-brain-sync --once 2>/dev/null || true
```

(The `--discover-new` pass is required — `--once` alone will not pick up a brand-new snapshot file. Verified 2026-07-05. If the sync says the file isn't allowlisted, `~/.gstack/.brain-allowlist` needs `projects/*/coach/*.json` under the USER ADDITIONS marker — already added on the Mac mini.)

## Output order (the report Gabriel sees)

The report is ONE message, the FINAL message of the turn, after all tool work (mining, snapshot, sync) is complete. No tool calls after it.

1. Headline trend (or "baseline week" on first run)
2. Last week's ledger graded — habit (behavior counts) + every live upgrade (its log's numbers)
3. What you did well — 2-3 bullets with quotes
4. What cost you — 1-2 bullets with quotes and the hours
5. Scores table (six skills, counts visible)
6. Main build: the six yes/no lines
7. How much ran without you + the next delegation opportunity
8. Trajectory table + changes ledger (skip on first run)
9. Goals delta — per-domain touched/untouched, the one drift flag + his call, numbers updated (skip if goals page not yet live)
10. **THE WEEK'S PAIR** — the system upgrade (approve → build → verify, same session) and **THE ONE HABIT**, bolded, the last thing he reads
