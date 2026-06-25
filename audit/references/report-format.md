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

    ## Simplifications I recommend (elegance, not defects)
    1. **<headline>** — <what's cleaner + why> · I'd: <action> · reversible because: <verify/rollback>

    (none? write "0 — already lean.")

    ## Risk check — what each "safe" fix would actually cost or cause

    | # | Fix | Risk of doing it | Risk of leaving it | Confidence it's real |
    |---|---|---|---|---|
    | 1 | <short name> | <rating + why> | <plain-English future pain> | <rating + evidence> |

    ## Fix it for you?
    - **Say "execute" (or "build")** — apply everything I recommend (safe fixes + simplifications) in one go, each safety-checked, pausing only on the judgment calls.
    - **Safe only:** "fix safe". **One item:** "fix N". **The judgment calls:** "walk me through".
    - Say "expand N" for why a finding matters, what I'd do, and how I'd verify.

If a bucket is empty, write "0 problems." Never invent findings.
