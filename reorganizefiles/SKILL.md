---
name: reorganizefiles
description: Use when the founder asks to reorganize folder structure, move files, clean up the project root, or says "restructure", "reorganize", "propose a folder structure", "move these files", "the root is cluttered". Produces a before/after folder tree, a blast-radius report (every command, doc, and code path that would break), and a move plan calibrated to the project's actual scale. Executes only after approval, with a snapshot → move → update references → verify → auto-rollback safety envelope.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Glob
  - Grep
---

# /reorganizefiles — Folder structure review and safe moves

Propose a better layout, then execute moves without breaking commands, docs, or automation. Never move anything in the initial pass.

## 1. Scope
Target the folder the founder names, or the project root. Every proposal must match the project's actual scale.

## 2. Map what's here
Walk the tree. Count files per folder. Read every `CLAUDE.md`, `README.md`, `SKILL.md`, and doc in scope. Build a list of every path referenced inside — and in external configs (launchd plists, cron, GitHub Actions).

## 3. Propose a target structure
If the structure is basically fine, say so and stop. Never invent a reorganization to look useful.

Otherwise propose a target tree:
- Flat beats nested when counts are small.
- Group by purpose (`ops/`, `scripts/`, `docs/`, `tasks/`).
- Don't mirror enterprise patterns (`src/`, `lib/`) unless the project actually needs them.

## 4. Blast-radius scan
Read `references/blast-radius-checklist.md`. For every proposed move, list what would break, split into:
- **Auto-updatable** — safe to find-and-replace.
- **Needs the founder's eyes** — ambiguous, regex-hostile, or in external systems.

If a move's blast radius outweighs the benefit, drop it.

## 5. Output — exactly this shape

    # Reorganize plan: <scope>
    Verdict: <PROPOSE | SKIP — structure is fine>

    ## Why change (or why not)
    <1-2 sentences>

    ## Before / after
    <side-by-side trees>

    ## Moves (N total)
    **1. `<old>` → `<new>`**
    - Why: <one sentence>
    - Breaks (N): <file + line for each>
    - Fix plan: <auto-updatable | needs your eyes, and why>

    ## Blast radius summary
    - Auto-updatable: N
    - Needs your eyes: N
    - External systems affected: launchd plist / cron / GitHub Actions (yes/no)

    ## Execute?
    - "execute" — all moves through the safety envelope.
    - "execute N" — one move at a time.
    - "expand N" — why a move matters.

## 6. Safety envelope
When the founder says "execute," read `references/move-safety-envelope.md` and follow it exactly. Never skip verify-before (step 2) or re-verify-after (step 4) — those catch silent breakage.

## 7. Rules
- Plain English. No jargon.
- Match the project's actual scale. Flat beats nested when counts are small. Push back on overkill.
- No moves without approval. Every move runs through the safety envelope.
- Never touch external systems without explicit approval.
- When in doubt, classify as "needs your eyes."
