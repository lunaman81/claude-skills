---
name: qa-gate
description: The checklist to run before reporting any code change as done, QA'd, tested, verified, or ready to ship/merge. Covers the thing you fixed PLUS the regressions a change can cause — the happy path, the real end-to-end flow (never a debug-flag shortcut), console errors, and mobile. Use whenever you changed code that affects behavior and are about to say "done" / "it works" / "ready to merge," or when the founder asks "did you QA it?", "is it ready?", "are you sure you tested it?", "did you run the browser tests?". A code change is not verified until this gate is green.
---

# qa-gate

The gate you run before telling the founder a code change works.

Built because the same miss kept happening: "I verified my fix" got reported as "I verified the feature." The fix was tested; the regressions weren't. The honest failure is confirmation bias — testing the thing you changed, not the system around it. (See Openly `apps/site-luna/tasks/lessons.md`, PR #110.)

## When to run

- You changed code that affects behavior and are about to report it **done / working / QA'd / verified / ready to merge**.
- The founder asks "did you QA it?", "is it ready?", "are you sure you tested everything?", "did you run the browser tests?".
- **Always** before merging a PR that touches a live site: `apps/site-luna/`, `apps/site-jason/`, `packages/`, `supabase/`.

Do NOT run for pure docs/config changes with no runtime behavior.

## The gate — all five, each with evidence

Run against the Vercel **preview** deploy (or production), driving the real UI with the `/browse` tool and **real inputs, not mocks**.

1. **The fix** — the specific thing you changed now does what it should.
2. **The happy path you might have broken** — a normal, valid input still reaches the success state. New guards, early-returns, or validation must not block good input. (This is the one most often skipped.)
3. **The real end-to-end path** — exercise the actual flow with a real input. NEVER substitute a debug flag, URL param, or test hook (e.g. `?fallback=...`) for a real path — that tests the screen, not the logic that routes to it.
4. **Console** — no new browser-console errors on the flows you touched.
5. **Mobile** — if the UI changed, repeat the key check at a phone viewport (`375x812`). Element refs renumber on narrow layouts — re-snapshot, don't reuse desktop refs.

## Output

A pass/fail matrix, one row per check, each with its evidence (what you did → what you saw). Post it as a comment on the PR.

## The rule

Do not report the change as done, working, verified, or ready until **every row is green** — or, if a row genuinely can't be tested, say so explicitly and why. Never round a partial pass up to "complete."

> "My fix works" is not "the feature still works." The gate is the difference.
