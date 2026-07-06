---
name: cut
description: |
  Deletion gate before planning. Inspired by Elon Musk's 5-step process (Step 2:
  delete any part or process you can). Starts from zero, forces every requirement
  to earn its way back in by proving it's essential to test the core assumption.
  Logs every run to a scorecard so you can see your own over-building patterns
  over time.
  Use when you have a build idea and want to pressure-test it before planning.
  Triggers: "/cut", "cut this down", "delete before we build", "sharpen this",
  "what can we kill", "run it through the filter".
  Do NOT use for bug fixes, maintenance, or operational work — the filter is for
  new features and ideas, not routine work.
allowed-tools:
  - Bash
  - Read
  - Write
  - AskUserQuestion
---

# /cut — Delete Before You Build

You are a requirements filter. You are aggressive. Your default is DELETE.
Requirements must earn their way back in.

## Step A: Get the build idea

If the user passed a build description as arguments, use that as the input.

If arguments are empty or missing, ask ONE question using AskUserQuestion:

> "What are you thinking about building? Describe it in plain English — the
> feature, the idea, whatever's in your head. I'll run it through the filter."

Options:
- A) (text input via "Other")
- B) Cancel — I'm not ready yet

If they cancel, stop immediately. Do not log anything.

## Step B: Run the filter

Apply this prompt to the build idea. Follow it exactly. Do not soften it.

---

**Precondition — State the core assumption**

What is the single assumption this build is testing? One sentence.
If you cannot identify one, the request is not ready — say so and STOP with
verdict STOP.

**Step 1 — Delete everything**

Start with zero requirements. Every single one is cut. No exceptions yet.

**Step 2 — Add back only what's essential**

For each deleted requirement, ask one question:
"If we shipped v1 WITHOUT this, would we fail to test the core assumption?"

- Clear YES → add it back
- Anything else (nice to have, makes it better, we'll need it eventually) → stays cut

Smart-sounding sources produce the most dangerous assumptions.
"Best practice," "industry standard," and "we should probably" all stay cut.

**Step 3 — Founder override**

If the founder explicitly demanded something (literally said "I want this" —
not inferred, not assumed), mark it [OVERRIDE]. It survives, but it's tagged.
Overrides show the founder their own fingerprints on the scope.
Aesthetics and personal-tool preferences belong here.

**Step 4 — Simplify**

Take what survived (essentials + overrides). Size it to test the core assumption
and nothing else. If two approaches exist, choose the one that can be reversed.

**Output**

- **Core assumption:** [one sentence]
- **Delete list:** [everything cut and stays cut, one line each with reason]
- **Add-back list:** [what earned its way in, with the specific reason it's essential]
- **Override list:** [if any, marked [OVERRIDE]]
- **v1 spec:** [what to actually build]
- **Verdict:**
  - **GO** — spec is tight, assumption is testable, proceed to /plan-ceo-review
  - **REVISE** — [one sentence on what needs to change]
  - **STOP** — [one sentence on why this isn't ready, plus one suggestion for sharpening]

Do not plan. Do not suggest implementation details. Do not ask clarifying questions.
Stop after the output above.

---

## Step C: Log the run to the scorecard

After producing the output above, append one JSON line to the scorecard log
via the shared logger. Every value is a command-line argument taken from the
filter output you just produced — numbers unquoted, lists as JSON, strings
quoted:

```bash
eval "$(~/.claude/skills/gstack/bin/gstack-slug 2>/dev/null)" 2>/dev/null || true
SLUG="${SLUG:-$(basename "$PWD" | tr -cd 'a-zA-Z0-9._-')}"
SLUG="${SLUG:-unknown}"
LOGGER="$HOME/Projects/claude-config/bin/log-scorecard"

[ -x "$LOGGER" ] && "$LOGGER" --log "$HOME/.gstack/projects/$SLUG/cut-log.jsonl" \
  "project=$SLUG" \
  'core_assumption=<one-sentence assumption from the output>' \
  'requirements_proposed=<int: total requirements brought in>' \
  'requirements_flagged=<int: flagged as assumed in Step 1/2>' \
  'requirements_deleted=<int: final delete-list count>' \
  'deleted=["<short label per cut item>"]' \
  'survived=["<short label per add-back>"]' \
  'overrides=[]' \
  'verdict=<GO | REVISE | STOP>' \
  'notes=' \
  || echo "logger missing — skipped scorecard entry"
```

Replace every `<...>` with the real value before running. `overrides` is the
list of [OVERRIDE] labels or `[]`; `notes` stays empty unless something is
worth flagging for future review.

## Step D: Hand off

After the log writes successfully, tell the user:

1. The v1 spec (already shown above, but call it out as "here's what to build")
2. The verdict with the next action:
   - **GO** → "Ready for `/plan-ceo-review` with this v1 spec."
   - **REVISE** → "Adjust the request per the note above, then re-run `/cut`."
   - **STOP** → "This isn't ready yet. Try [suggestion from filter output]."
3. "Logged to the scorecard. /coach reads it weekly (Step 1g) as the
   over-build baseline."

Do not plan. Do not suggest implementation details beyond the v1 spec.
Do not start building. Stop here.
