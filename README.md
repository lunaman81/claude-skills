# claude-skills

Personal Claude Code skills. Drop into `~/.claude/skills/`.

## Skills

- **[cut](cut/SKILL.md)** — Deletion gate before planning. Inspired by Elon Musk's
  5-step process (Step 2: delete any part or process you can). Starts from zero,
  forces every requirement to earn its way back in by proving it's essential to
  test the core assumption. Logs every run to a scorecard.

- **[audit](audit/SKILL.md)** — Plain-English health check for a file or folder.
  Reports three kinds of problems — 🔴 Broken, 🟡 Out of date, 🟢 Too big — with
  fixed-wording impact per finding (no improvised drama), sorts fixes into
  "safe to auto-fix" vs. "needs your decision," and offers to apply them so the
  non-technical owner never has to hand-edit files. Output is a verdict + Top 3
  fixes + one-line punch list, with "expand N" to drill down on any finding.

- **[reorganize-files](reorganize-files/SKILL.md)** — Folder restructure with a
  before/after tree, blast-radius scan for what would break, and a snapshot →
  rollback safety envelope so moves can be undone if anything goes wrong.

- **[write-like-amazon](write-like-amazon/SKILL.md)** — Rewrite or draft docs in
  Amazon's narrative writing style: prose over bullets, customer-first, specific
  numbers, no weasel words. Three formats: 6-pager (decisions and strategy —
  the default), PR-FAQ (new products), working-backwards (early product briefs).
  Reads existing drafts first and confirms format before rewriting.

## Install

```bash
git clone https://github.com/lunaman81/claude-skills.git ~/claude-skills
ln -s ~/claude-skills/cut ~/.claude/skills/cut
ln -s ~/claude-skills/audit ~/.claude/skills/audit
ln -s ~/claude-skills/reorganize-files ~/.claude/skills/reorganize-files
ln -s ~/claude-skills/write-like-amazon ~/.claude/skills/write-like-amazon
```

Or copy individual skill folders into `~/.claude/skills/` directly.
