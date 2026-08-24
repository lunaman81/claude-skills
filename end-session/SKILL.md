---
name: end-session
description: Close a working session by reconciling `tasks/todo.md` against work that actually shipped, updating `tasks/lessons.md` if there was a correction, and committing as `session-end YYYY-MM-DD`. Use when the user says "end session," "wrap up," "we're done for today," or it's clear the session is winding down. NOT for "save state" or "save context" mid-task — that's /context-save (this skill commits and closes out; it doesn't checkpoint working context). Prevents bloat in `tasks/todo.md`.
---

# end-session

A deliberate ritual for closing a working session, in whichever project you are in. Stops `tasks/todo.md` from bloating with shipped items that never got moved to **Done**. (Step 0 routes by how the repo tracks work — it is not Openly-specific, despite how this line read until 2026-08-18.)

## When to run

- User says "end session," "let's wrap up," "save state," "we're done."
- The user is about to stop working and the session shipped real code or docs.
- Proactively offer to run it if you've shipped 3+ items in a session and todo.md hasn't been touched.

Do NOT run if no real work shipped (no commits, no doc decisions).

## Step 0 — Route by how this repo tracks work

Check the project's CLAUDE.md first:
- **Board-based repos** (the project says its to-do list is a GitHub Projects board and `tasks/todo.md` is retired — e.g., openly): SKIP steps 1–4 and 6. Instead: close board issues that shipped this session, open ONE issue for anything that genuinely needs follow-up, and add a `tasks/lessons.md` line if there was a correction (step 5). Do NOT create a `session-end` commit — the board is the record.
- **todo.md-based repos** (e.g., sentinel, family-office): follow the steps below unchanged.

## Steps

### 1. List commits since the last `session-end`

```
git log $(git log --format=%H --grep='^session-end' -n 1 2>/dev/null || git log --reverse --format=%H -n 1)..HEAD --oneline
```

If no previous session-end commit exists, fall back to commits in the last 24 hours.

### 2. Walk the In progress section of `tasks/todo.md`

For each item:
- Did it ship in one of the commits from step 1? → propose moving it to **Done**.
- Is it marked `[x]`? → it must move. `[x]` items in **In progress** are wrong by definition.
- Was it superseded by a decision (check `tasks/decisions.md` and `tasks/session-log.md`)? → propose moving to Done or Blocked with a note.

### 3. Show the founder the proposed moves before writing

Plain English. One line per item. Example:

> Proposing to move 3 items from In progress → Done:
> - "Pre-launch monitoring v1 spec" — shipped in commit `4b5473c` (2026-05-05)
> - "Round 3 polish on Speed/PriceGap/Elimination" — shipped in commits `f617082` → `c18e5b8` (2026-05-11)
> - "GitHub migration finalized" — shipped in commits `2ec0e7f` + `587560b` (2026-05-11)
>
> No `[x]` items remain in In progress after this. OK to write?

Wait for confirmation. **Never write without explicit OK.**

### 4. Update `tasks/todo.md`

For each approved item:
- Remove from **In progress**.
- Add to **Done** at the top of the section as a single bullet: `**YYYY-MM-DD — <title>** — <one-line summary with commit hashes if relevant>.`

### 5. Add a lesson ONLY on the second occurrence

Ask two questions, in order. Both must be yes.

1. **"Did the founder correct my approach, and has this SAME mistake happened before?"** Grep `tasks/lessons.md` and `tasks/lessons-archive.md` to check — do not go from memory. A first-time mistake is fixed in the moment and **nothing is written**.
2. **"Is there no automatic check that catches it?"** If a hook, test, or CI job would catch it, build that instead and write nothing.

If both are yes, add **one line** with today's date:
`- YYYY-MM-DD — [the mistake] → **Rule:** […] → **Next:** [the check that would end it, or "no check possible — judgment"]`

One line, not a paragraph. The file is capped at ten by `tools/guards/rules-budget.mjs`, which fails the pull request at eleven. At the cap, the way forward is to build a check and delete a line — not to raise the cap.

**Why this changed (2026-08-24).** The old rule was "write one whenever the founder corrects you." That produced 194 entries in four months, averaging 190 words each. Nine problems accounted for 72 of them, one was written five separate times with its own tally reading "Prose is 0-for-4 on this", and zero checks were ever built. Eighteen entries said "archive this" and none moved. Anthropic's own guidance sets the bar at the second occurrence for exactly this reason.

### 6. Commit and push

Stage:
- `tasks/todo.md`
- `tasks/lessons.md` (if updated)
- Any other docs touched this session that haven't been committed

Commit message format:
```
session-end YYYY-MM-DD: <one-line summary of session>
```

Examples:
- `session-end 2026-05-11: Round 3 polish + GitHub migration finalization`
- `session-end 2026-05-07: monitoring spec finalized + lesson logged`

Then push per the push policy in the global CLAUDE.md: docs/todo commits like this one can push directly ONLY if the founder already approved the session's changes — otherwise ask first. Anything touching live-site code never ships this way (PR only).

## Anti-bloat checks (before declaring done)

Run these against the full `tasks/todo.md`, not just the items you moved. They catch the slow drift that makes the file unreadable over weeks.

1. **Zero `[x]` items in the In progress section.** `grep -c "^- \[x\]" tasks/todo.md` against the In progress block must return 0.
2. **In progress fits in your head.** If a fresh agent at next session-start couldn't summarize what's active in 30 seconds, the section needs further pruning. Move stalled items to Blocked.
3. **Done entries have dates.** Every Done bullet starts with `**YYYY-MM-DD —**`.
4. **No meeting-organized sub-sections in In progress.** Items are organized by topic, not by which sync they came from. Sync decisions live in `tasks/decisions.md`.
5. **No relative dates anywhere.** `grep -in "tomorrow\|today\|next week\|yesterday\|this week" tasks/todo.md` must return nothing inside In progress/Blocked/Decisions pending. "Tomorrow" rots in 24 hours; absolute dates or no date.
6. **No item longer than 3 lines of context.** Walk the file. If any single bullet has more than 3 lines under it, that's coaching prose — it belongs in `tasks/session-log.md` or a doc, with a link from here.
7. **No duplicates.** For any unusual keyword (a Vercel project name, a commit hash, a doc path), `grep` it — if it appears in two sections of the file, those items should be merged.
8. **No misplaced types.** In progress should contain workstreams only. If you see a pending founder decision, move it to `Decisions pending`. If you see a watch-list or parking-lot item, move it to `Appendix`. If `Decisions pending` or `Appendix` doesn't exist yet, create it.
9. **Owner clarity on mixed-owner workstreams.** If a workstream's heading names a single owner (e.g., "Gabriel — fulfillment workstream") or a single counterparty (e.g., "Phase 1.5 wrap with Jason"), sub-bullets can skip owner tags. If the workstream mixes both founders without naming them (catch-all sections like "Other open items"), every sub-bullet needs `[Gabriel]` / `[Jason]` / `[Both]`.

If any of checks 5–9 fail, fix them in the same session-end commit. They are the failure modes that bloated todo.md before — catching them weekly is what keeps the file readable.

## What "shipped" means

A commit landed on the active branch that closes the work. The change is verifiable in the diff. If unsure, ask the founder before moving. Do not optimistically promote things to Done.

## Failure modes to avoid

- **Moving items that aren't actually shipped.** Verify against `git log` and the diff, not the conversation. The conversation lies; commits don't.
- **Adding new In progress items without prompting.** End-of-session is reconciliation, not planning. New items go in via a separate request.
- **Skipping the founder confirmation in step 3.** Always show the plan. They'll catch items you misread.
- **Forgetting the lessons line when there was a correction.** Re-read your own back-and-forth — if the founder said "no," "stop," "don't," or "actually," there's probably a lesson.
