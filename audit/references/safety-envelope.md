# Safety envelope

Run this procedure every time the founder says **"fix safe" / "fix N" / "walk me through"**. It guarantees applied fixes can't silently break a working setup.

## Procedure

1. **Snapshot.** Run `git status`.
   - If there are uncommitted changes: `git stash push -m 'audit pre-fix snapshot'` and remember the stash reference.
   - If clean: remember the current commit hash for rollback.

2. **Verify the project works right now.** Pick one concrete, observable thing that proves the current state is healthy, state it explicitly with evidence. Examples:
   - *"run.log shows yesterday's daily push exited clean (exit 0 at 18:04 PT)."*
   - *"CLAUDE.md parses as valid Markdown and is 101 lines."*
   - *"index.html still serves the dashboard on localhost:3000."*

3. **Apply the fix.** Make only the change the founder approved — nothing extra, no unsolicited cleanup.

4. **Re-verify.** Check the *same* concrete thing from step 2 still works. Cite evidence.

5. **If re-verify fails: roll back immediately.**
   - `git reset --hard <snapshot-hash>`
   - If a stash was taken: `git stash pop`
   - Tell the founder plainly what broke, what was restored, and that no change is left behind.

6. **Summary.** Show the founder:
   - Before / after of each change.
   - The re-verify result.
   - Anything they should know about (e.g. "your working tree had uncommitted edits; they're stashed at `stash@{0}` — restore with `git stash pop` when ready").

## If the target is outside a git repo

Skip steps 1 and 5, but still run 2 / 3 / 4 / 6. Before proceeding, tell the founder plainly: *"This path isn't in a git repo, so I can't auto-rollback. Confirm you want me to apply the fix anyway?"*

## Hard rules

- Never skip step 2 or step 4. Without a concrete before/after signal, there's no way to know whether the fix broke anything.
- Never batch-apply multiple fixes without re-verifying between them. One fix, one envelope run, then the next.
- Never modify the target's backup/snapshot in the same pass as the fix.
- If a fix requires touching files in more than one repo, run the envelope separately for each repo.
