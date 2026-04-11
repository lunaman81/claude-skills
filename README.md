# claude-skills

Personal Claude Code skills. Drop into `~/.claude/skills/`.

## Skills

- **[cut](cut/SKILL.md)** — Deletion gate before planning. Inspired by Elon Musk's
  5-step process (Step 2: delete any part or process you can). Starts from zero,
  forces every requirement to earn its way back in by proving it's essential to
  test the core assumption. Logs every run to a scorecard.

## Install

```bash
git clone https://github.com/lunaman81/claude-skills.git ~/claude-skills
ln -s ~/claude-skills/cut ~/.claude/skills/cut
```

Or copy individual skill folders into `~/.claude/skills/` directly.
