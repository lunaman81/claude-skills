# claude-skills

Personal Claude Code skills. Drop into `~/.claude/skills/`.

## Skills

- **[cut](cut/SKILL.md)** — Deletion gate before planning. Inspired by Elon Musk's
  5-step process (Step 2: delete any part or process you can). Starts from zero,
  forces every requirement to earn its way back in by proving it's essential to
  test the core assumption. Logs every run to a scorecard.

- **[reorganize-files](reorganize-files/SKILL.md)** — Folder restructure with a
  before/after tree, blast-radius scan for what would break, and a snapshot →
  rollback safety envelope so moves can be undone if anything goes wrong.

## Retired — don't re-add

- **audit** — retired 2026-08-03 into `/setup-review` (in the private
  `claude-config` repo), which absorbed its questions along with `/harness-audit`,
  `/harness-eval`, `/memory-audit` and `/permission-audit`.
- **coach** — moved 2026-08-02 into `openly-monorepo/.claude/skills/coach`. It
  reads Openly's `tasks/lessons.md` and board, and cloud sessions can only load
  skills that live inside the repo clone.
- **write-like-amazon** — moved 2026-08-03 to
  `openly-monorepo/.claude/skills/write-like-amazon`, where it was deliberately
  placed so Jason loads it automatically (decision 2026-06-01). It lived in both
  places identically, which is exactly how the `build` skill drifted into two
  versions.

## Install

```bash
git clone https://github.com/lunaman81/claude-skills.git ~/Projects/claude-skills
ln -s ~/Projects/claude-skills/cut ~/.claude/skills/cut
ln -s ~/Projects/claude-skills/end-session ~/.claude/skills/end-session
ln -s ~/Projects/claude-skills/boost-prompt ~/.claude/skills/boost-prompt
ln -s ~/Projects/claude-skills/reorganize-files ~/.claude/skills/reorganize-files
```

Normally you don't run these by hand — `claude-config/bin/sync-skills pull` runs
at every session start and links anything new automatically.
