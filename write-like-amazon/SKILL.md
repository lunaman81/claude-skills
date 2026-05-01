---
name: write-like-amazon
description: Rewrite or draft documents in Amazon's narrative writing style — prose over bullets, customer-first, specific numbers, no weasel words. Use whenever the user is writing a strategy, plan, brief, memo, decision doc, business review, quarterly review, or product proposal — even if they don't explicitly say "Amazon style." Also trigger on explicit phrases: "write like Amazon," "Amazon-style," "6-pager," "six-pager," "narrative memo," "PR-FAQ," "press release FAQ," "working backwards," "WBR," "OP1," "OP2." Three formats supported: 6-pager (decisions and strategy — the default), PR-FAQ (new products), working-backwards (early-stage product briefs).
---

# Write Like Amazon

Amazon's writing culture replaces slide decks with narrative prose. Documents are read silently for the first 20 minutes of a meeting, then debated. The form forces clear thinking — bullets and slides hide weak logic; full sentences expose it.

This skill helps draft new docs or convert existing drafts to that style.

## Step 0: Pick the format before writing anything

Always identify the format first. The three formats serve different jobs and the structures don't overlap — getting this wrong means rewriting from scratch later.

- **6-pager (narrative memo)** — recommending a decision, presenting a strategy, doing analysis, business or quarterly review. **Most internal docs are 6-pagers. Default to this when in doubt.** → `references/six-pager.md`
- **PR-FAQ** — proposing a *new* product or feature that doesn't exist yet, framed as if it had already launched. Press release on top, FAQs below. → `references/pr-faq.md`
- **Working backwards** — earliest-stage thinking about a possible product, before commitment. Customer-first short brief. → `references/working-backwards.md`

### When converting an existing doc

1. **Read the existing doc first.** Don't pick a format from the filename or the user's prompt alone — read the content.
2. Match it against these signals:
   - Has a "decision required," "recommendation," "options considered" section, or is addressed to a stakeholder for sign-off → **6-pager**
   - Announces or proposes a not-yet-built product, has a "we are launching X" framing → **PR-FAQ**
   - Asks "should we even do this," explores customer problem before solution → **working backwards**
3. **State your guess and confirm before rewriting.** Example: *"This reads like a 6-pager — it's a strategy doc with a CEO decision required. I'll rewrite it in that format unless you want PR-FAQ or working backwards instead."* One-keystroke confirmation is fine.
4. If the user pushes back, switch formats.

### When drafting from scratch

1. If the user named the format explicitly ("write a 6-pager," "draft a PR-FAQ"), use it.
2. If not, **ask** — don't guess. Briefly recommend the format you'd default to based on what they described, then ask them to confirm or pick a different one. Example: *"For 'should we add chat to the homeowner side?' I'd default to a 6-pager since it's a decision doc. Sound right, or do you want PR-FAQ instead?"*
3. Don't draft until the format is confirmed.

## Core principles (apply to every format)

These are what makes prose "Amazon-style." Apply them ruthlessly. Most rewriting work is enforcing these.

### 1. Narrative prose, not bullets

Bullets hide weak thinking. They let you list disconnected ideas without explaining how they connect. Full sentences force the logic into the open. Use bullets only for genuinely list-like content — items in a budget, steps of a process, options being compared. Default to prose.

The tell: if you can replace a sentence with three bullets and lose nothing, you weren't saying anything.

### 2. State the thesis in the first sentence

The first sentence tells the reader what the doc is about and what you want them to think or do. No throat-clearing. No "this document explores..."

Bad: *"This document explores our LLM traffic strategy options."*
Good: *"We recommend GO on a 90-day push to win 'roof replacement cost in [city]' on AI search engines, scoped to 30 cities."*

### 3. Specific over vague — kill weasel words

Replace every vague quantifier with a specific number, date, or constraint. If the number isn't known, say "we don't know yet" — that itself is information.

Common weasels to find and replace:

| Weasel | Replace with |
|---|---|
| many, several, some, a lot of | exact count |
| soon, in the near future, shortly | a date |
| robust, scalable, world-class | the actual capacity or constraint |
| leverage | use |
| synergy, paradigm, holistic, ecosystem | nothing — delete |
| often, sometimes, frequently | a frequency or count |
| significant, substantial | the actual magnitude |

(This is one of the few tables that earns its place — the substitution is genuinely list-like.)

### 4. Customer first

Every doc names the customer in the first paragraph and stays anchored there. Be specific: not "users" or "homeowners" — describe a specific person in a specific situation. If a paragraph drifts away from the customer, ask whether it earns its place.

### 5. Data and anecdote, both

Quantitative numbers prove the size of the problem. A specific anecdote — a real customer, a real quote, a real call — proves it's real and not hypothetical. Use both. One without the other is suspect: just data feels detached, just anecdote feels cherry-picked.

### 6. Active voice, named owners

"We will launch X in May" — not "X will be launched." Active voice forces ownership. Passive voice hides who's responsible. When something needs to happen, name who does it and by when.

### 7. Surface tradeoffs and risks

Don't bury the downside. Amazon docs always include what could go wrong, what was considered and rejected, and what we're giving up. If the doc reads like everything is upside, the reader stops trusting it. The strongest docs put the biggest risk in plain sight near the top.

### 8. The "so what" test

Every paragraph earns its place. Read the doc and for each paragraph ask: if I deleted this, would the reader miss anything? If no, delete it. The goal is the smallest doc that fully makes the case.

## Workflow (after format is picked)

### Converting an existing draft

1. Read the relevant reference file for the chosen format's structure.
2. Find the thesis in the existing draft. If it's not in the opening sentence, move it there.
3. Convert bullets to prose wherever the bullets are hiding logic. Keep them only where the content is genuinely list-like.
4. Find weasel words and either replace with specifics or delete.
5. Check the customer is named in the opening with specificity.
6. Check tradeoffs and risks are present and prominent. Add a section if missing.
7. Apply the "so what" test paragraph by paragraph.
8. Read the final version cold. Would a reader picking it up with no prior context understand the situation, the recommendation, and the reasoning? If not, fix.

After rewriting, show the new version and call out the 3-5 most important changes made and why. Don't list every edit — focus on the structural moves.

### Drafting from scratch

1. Read the relevant reference file for the chosen format's structure.
2. Before writing, ask the user for the four things every Amazon doc needs: the customer, the customer's problem, the recommendation or decision, and the data/evidence available. Don't draft without these — guessing produces generic prose.
3. Draft. Apply the core principles throughout.

## Common failure modes

- **Bullet drift.** Most bullets get converted to prose, but some stay "for readability." They were the tell of weak thinking — convert them too.
- **Weasel sneak-in.** "We expect significant growth" creeps back in late edits. Significant compared to what? Replace.
- **Hidden tradeoffs.** The doc presents the recommendation as obviously correct. If it were obviously correct, you wouldn't need a doc. Surface the case against.
- **Generic customer.** "Homeowners want lower prices." Which homeowners? In what situation? Be specific or you're not customer-anchored.
- **Buried lede.** The thesis is in paragraph 4 instead of sentence 1. Move it up.
- **Conclusion-free.** No clear recommendation or decision required. Amazon docs always end with what the reader should do next.
