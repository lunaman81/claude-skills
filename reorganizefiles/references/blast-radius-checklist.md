# Blast-radius checklist

For every proposed move, check every place below for references to the old path. SKILL.md section 4 tells Claude when to use this — the procedure lives here.

## Inside the repo
- [ ] Global `CLAUDE.md` (`~/.claude/CLAUDE.md`)
- [ ] Project `CLAUDE.md` (project root)
- [ ] `README.md` and every file under `docs/`
- [ ] Every `SKILL.md` in `~/.claude/skills/` and the user's skills project
- [ ] Shell scripts (`*.sh`) — hardcoded paths and `cd` targets
- [ ] JavaScript/HTML — `require()`, `import`, `<script src>`, fetch URLs
- [ ] `package.json` scripts, `tsconfig.json`, any config file with paths
- [ ] Task files (`tasks/*.md`) — stale references still count
- [ ] `.gitignore` — stale ignore lines can hide moved files

## Outside the repo
- [ ] launchd plists (`~/Library/LaunchAgents/`, `/Library/LaunchDaemons/`)
- [ ] cron jobs (`crontab -l`)
- [ ] GitHub Actions workflows (`.github/workflows/*.yml`)
- [ ] Any deployed server or Mac Mini that runs the project — hardcoded paths there break silently
- [ ] Tailscale or SSH configs that reference the old path

## How to search
- Run `grep -rn "<old-path>" .` for the in-repo pass.
- Run `grep -rn "<old-path>" ~/Library/LaunchAgents/ ~/.claude/` for the external pass.
- For common filenames (e.g., `run.js`), also search for the bare filename — `grep -rn "run.js" .` — to catch references without the folder prefix.

## Classify each hit
- **Auto-updatable** — exact path match, not inside a comment or unrelated string, safe to find-and-replace.
- **Needs the founder's eyes** — partial match, regex-hostile, inside a string that means something different, or in an external system where a wrong edit could break automation.

When in doubt, classify as "needs the founder's eyes." A false positive costs one extra question; a false negative can break daily auto-push.
