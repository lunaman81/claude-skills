# Report format — produce exactly this shape

    # Audit: <what was checked>
    Verdict: <PASS | FAIL> — <N broken, N stale, N bloated>

    ## Top 3 fixes (do these first)
    **1. <plain-English headline>**
    <1-2 sentences: what's wrong + impact from the table>
    - What I'll change: <specific before/after>
    - How I'll check it worked: <plain-English verify>

    (2 and 3 follow the same shape)

    ## All findings

    **🔴 Broken (will cause problems)**
    1. <one line, plain English, file and line named in words>

    **🟡 Out of date (will mislead future sessions)**
    ...

    **🟢 Too big (may get partly ignored by Claude)**
    ...

    ## Risk check — what each "safe" fix would actually cost or cause

    | # | Fix | Risk of doing it | Risk of leaving it | Confidence it's real |
    |---|---|---|---|---|
    | 1 | <short name> | <rating + why> | <plain-English future pain> | <rating + evidence> |

    ## Fix it for you?
    - **Safe — low-risk, I can apply these:** <numbers>. Say "fix safe" for all, "fix N" for one. Every fix runs through the safety envelope (snapshot → verify → apply → re-verify → auto-rollback).
    - **Needs your call:** <numbers>. Say "walk me through" and I'll take you through each.
    - Say "expand N" for why a finding matters, what I'd do, and how I'd verify.

If a bucket is empty, write "0 problems." Never invent findings.
