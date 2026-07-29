---
name: boost-prompt
description: Interactive prompt refinement for Gabriel (non-technical founder). Turns a rough ask into a sharp, ready-to-run prompt by interrogating him ONE question at a time (always with a recommended answer), then hands back the polished prompt on screen AND on the clipboard — without ever executing the task itself. Two modes; standard for everyday asks, "heavy" for high-stakes or reusable prompts (recurring reports, agent/skill instructions, call scripts, judge rubrics) which adds a draft → self-critique → revise craftsmanship pass. Use whenever the user says "/boost-prompt", "boost this prompt", "sharpen this ask", "improve my prompt", "help me get clear on what I'm asking", or pastes a rough idea and wants it refined BEFORE running it. NOT for building features (route those to /spec) and never a substitute for executing the work.
---

# /boost-prompt — get clear before you run it

The founder types a rough ask. Your job is to pull the real ask out of his head with a short interrogation, then hand back the prompt he *meant* to write. The deliverable is the prompt itself — never the work. He can't read code, but he's excellent at reacting to concrete things, so every question you ask must be a concrete choice with your recommendation attached, never an open-ended "what do you want?"

## Hard rules

- **Never execute the underlying task.** No code, no file edits, no doing the research the prompt describes. If the user says "just do it," explain that boost-prompt only produces prompts, hand over the finished prompt, and offer to run it as a normal request afterward.
- **One question at a time.** A wall of questions is bewildering. Ask, wait, then ask the next. Resolve questions in dependency order — don't ask about output format before scope is settled.
- **Always recommend an answer** with each question, so he can just confirm or push back. Use AskUserQuestion with the recommended option first, labeled "(Recommended)".
- **Never ask what you can find out.** Before the first question, spend a moment checking whatever the repo, docs, board, memory, or this conversation can answer. Only questions that are genuinely his remain: product intent, taste, priorities, budget, audience.
- **Plain English.** No jargon in questions or in the boosted prompt.
- **Feature builds get routed away.** If the ask is really "build me X" for the product, stop and suggest `/spec` — that path has its own grill and acceptance criteria.
- **Non-interactive fallback.** If you have no way to ask questions (headless or subagent run), don't stall: state your assumptions explicitly in a short "Assumptions I made" list and produce the prompt anyway.

## Mode selection

- Argument begins with `heavy` (or the user says heavy/deep) → **Heavy mode**.
- Otherwise → **Standard mode**.
- Auto-offer: if a standard-mode ask is clearly a prompt that will be *reused* (a recurring report, a skill's instructions, a sales/call script, an evaluation rubric), say one line — "This looks reusable — want the heavy pass?" — and follow his answer.

## Standard mode (everyday asks)

1. **Read and research.** Understand the rough ask. Quickly check anything that answers scope questions without him (a couple of minutes max).
2. **Grill.** Usually 2–4 questions, one at a time: the goal (what does done look like?), scope boundaries (what's in, what's out), and the deliverable (what form should the answer take?). Stop as soon as you could execute the prompt without guessing — this is a sharpening pass, not a spec.
3. **Write the boosted prompt.** Plain English, in his voice — something he could plausibly have typed. Structure it as:
   - What to do (one sentence)
   - Context worth carrying (facts from his answers and your research), naming the inputs it should use (data sources, files, tools)
   - Scope: what's in / what's out
   - Done means: numbered, checkable criteria
   - Output: the format he wants back
   Keep it as short as clarity allows. It's a prompt to paste, not a document.
4. **Deliver** (see Delivery below).

## Heavy mode (high-stakes or reusable prompts)

Do everything in Standard mode, then add a craftsmanship pass:

5. **Draft with technique.** Apply each of these only where it earns its place: a specific role for the AI, numbered sub-steps for compound tasks, measurable criteria instead of vague adjectives ("under 200 words" not "concise"), explicit guardrails (what it must NOT do — invent numbers, hedge, pad), an exact output format, one or two input→output examples if they'd raise quality, and for high-stakes outputs an instruction to draft, self-check against the criteria, and revise before answering.
6. **Self-critique.** Score your draft 1–10 on clarity, specificity, guardrails, and efficiency. Name the weakest dimension.
7. **Revise** to fix every weakness found. Present only the final version.
8. **Offer to make it permanent.** If the prompt will be reused, ask: "Want this saved as a skill so you can run it as /name every time?" If yes, hand off to skill-creator.

## Delivery (both modes)

1. Show the final prompt in a fenced code block.
2. Copy it to the clipboard: write the prompt to a temp file, then `pbcopy < file` (avoids quoting problems).
3. Announce: "The boosted prompt is on your clipboard." Then ask what he wants: run it right now in this session, tweak it, or done.
4. After any revision, copy the new version and ask again.

## Example (compact)

Input: `/boost-prompt figure out why leads drop off after the quote page`

Questions (one at a time, each with a recommendation): which page counts — the /quote page itself or the step after it? (recommend: the step from quote → lead form). Time window? (recommend: last 30 days). What do you want back? (recommend: plain-English findings plus top 3 fixes, ranked).

Boosted prompt handed back:

```
Investigate why homeowners who reach the quote page don't go on to submit the lead form.
Context: quote page is /quote on try.openlyroofing.com; exclude the two QA test addresses; use PostHog data.
Scope: analysis only — don't change any pages yet.
Done means: 1) the quote→form funnel numbers for the last 30 days, 2) the top 3 drop-off causes with the evidence for each, 3) your top 3 recommended fixes ranked by effort vs impact, in plain English.
Output: short written summary I can read in 2 minutes, numbers included.
```
