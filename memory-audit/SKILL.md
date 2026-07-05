---
name: memory-audit
description: Re-run the memory-stack audit — measure whether knowledge captured in past sessions actually resurfaces and compounds (capture→resurface ratios, repeat-correction rate, stale-memory count), grade every change from the last run against evidence, and report the delta. Use when the user says "memory audit", "/memory-audit", "is my memory compounding", "re-run the memory stack audit", "knowledge audit", or the 4-week review is due. Each run reads the prior ledger from claude-config/audits/memory/ and writes a new one, so runs compound. NOT /audit (config breakage/staleness), NOT /drift or /harness-drift (whether the system's picture of WHO GABRIEL IS is accurate), NOT /harness-audit (setup shape/structure), NOT /coach (weekly collaboration review) — this audits whether KNOWLEDGE FLOWS: captured → resurfaced → promoted → decayed.
---

# memory-audit — does every session compound into the next?

The memory stack: auto-memory dirs (`~/.claude/projects/*/memory/`), CLAUDE.md family,
per-repo `tasks/lessons.md`, capture skills (/coach /retro /end-session /goals /cut),
hooks/injection layer, gbrain (bridge only — content governance is Hermes's), and
session transcripts as ground truth.

**Ledger:** `~/Projects/claude-config/audits/memory/YYYY-MM-DD.md` — one file per run,
committed to the private claude-config repo (git history = history of outputs, changes,
impacts). NOT gbrain: this ledger audits gbrain among other systems and must stay
readable when gbrain is down; a pointer page `docs/memory-audit-ledger` in gbrain says
where the history lives.

**Cadence:** default mode every 4 weeks; `full` quarterly or when things feel off.

## Steps (default mode — cheap, ~minutes)

### 1. Load the prior ledger (this is what makes runs compound)
```
ls ~/Projects/claude-config/audits/memory/ | sort | tail -1   # read that file
```
Open with the delta: each change listed in the prior run's Changes section gets graded
now (see step 3). First run: baseline is in `2026-07-05.md`.

### 2. Re-measure the three metrics (same commands every run, comparable numbers)
- **Corrections/week** — dated lesson entries, last 28 days, all live lessons files:
  `grep -oE '2026-[0-9]{2}-[0-9]{2}' <each live lessons.md> | sort | uniq -c`
  (live files listed in the prior ledger; update the list if repos changed)
- **Repeat-correction rate** — correction classes appearing ≥2× in the window: read the
  new lesson lines (git log -p since last run), cluster by class, count repeats. This is
  the number that must fall.
- **Stale-memory count** — for each memory file in `~/.claude/projects/*/memory/` older
  than 6 weeks or making verifiable claims (paths, tools, states): spot-check the 5
  stalest. Count contradictions with reality or with sibling memories.
- **Recall ratio (spot)** — Read-tool calls on memory topic files and lessons.md across
  transcripts since last run vs session count:
  `grep -l '"name":"Read"' + file_path filters over ~/.claude/projects/*/*.jsonl`

### 3. Grade every change from the prior run
For each Changes entry: **kept** (mechanism still in place, metric moved or neutral),
**adjusted** (needed rework — say what), **rolled back** (made things worse or never
fired — say why, restore from the snapshot named in the entry). Evidence required per
grade: a command output, file state, or transcript count — never recollection.

### 4. Write the new ledger file + commit
Sections: `## Verdict` (one paragraph, delta-first), `## Metrics` (table: metric,
prior, now), `## Changes graded` (prior run's changes with grades + evidence),
`## New changes` (anything newly proposed/applied this run, each with its snapshot
path), `## Next review due` (date). Commit to claude-config (direct commit — private
repo, standing approval for ledger commits).

## Full mode (`/memory-audit full` — quarterly, expensive: ~15 agents / ~1M tokens)
Requires the user's explicit go (mention cost). Run the saved workflow:
```
Workflow({ scriptPath: "$HOME/.claude/workflows/memory-stack-audit.js" })
```
It fans out one auditor per store + adversarial verifier per claim + completeness
critic. Feed its verified findings into steps 3–4 as extra evidence. Update the
workflow's store list first if stores were added/removed since the script was saved.

## Standing rules (from the 2026-07-05 founder decisions)
- Push beats pull: never fix a resurfacing gap with a "remember to read X"
  instruction — use a hook or a scripted skill read.
- Corrections replace, never append. One fact = one file = current state.
- Archive, don't delete; every applied change names its snapshot path.
- gbrain content (emails/people/meetings) is Hermes's working set — never decay it
  based on Claude-Code-side read counts. Only the bridge (MCP, daemons, retrieval
  quality) is in scope here.
- 60-day dead-store rule: a store with zero reads in 60 days gets archived or gets a
  reader — flag candidates in the Verdict.
