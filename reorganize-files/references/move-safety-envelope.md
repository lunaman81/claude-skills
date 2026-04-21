# Move safety envelope

Run this for every move the founder approves. SKILL.md section 6 tells Claude when to use this — the procedure lives here.

## Steps

1. **Snapshot.** Stash any uncommitted work. Commit the current state as a rollback point with a clear message: `restructure: pre-move snapshot for <old path> → <new path>`.

2. **Verify before.** Prove the thing works at the old path. Examples:
   - If it's a script: run it (or a dry-run flag) and confirm exit code 0.
   - If it's a doc: confirm it exists and the referencing command opens it.
   - If it's a config: confirm the system that reads it is currently reading it.
   Record the verify command so step 4 can re-run the same one.

3. **Apply the move.**
   - `git mv <old> <new>` (preserves history).
   - Update every **auto-updatable** reference from the blast-radius list via find-and-replace. Show the diff.
   - For **needs-your-eyes** references, pause and ask the founder per reference. Don't guess.

4. **Re-verify.** Run the same command from step 2. If it fails, go to step 6.

5. **Final sweep.** Run `grep -rn "<old-path>" .` and the external grep. If anything still references the old path (that wasn't intentionally left), go to step 6.

6. **Auto-rollback on failure.** If step 4 or 5 fails:
   - `git reset --hard <snapshot commit>` to restore pre-move state.
   - Report what failed, in plain English.
   - Stop. Don't continue to the next move.

7. **Commit on success.** One commit per move (or one per logical batch, if the founder prefers). Message: `restructure: <old> → <new>`.

## Rules
- Never skip step 2 (verify before) or step 4 (re-verify after). Those are the steps that catch silent breakage.
- Never use `git push --force` or `git rebase` during a restructure. If a conflict happens, stop and ask.
- If the move touches an external system (launchd plist, cron, GitHub Actions), pause after step 3 and remind the founder to test that system before continuing.
- If five consecutive moves pass cleanly and the remaining moves are similar, the founder may say "continue without pause" to batch the rest. Until then, one move at a time.
