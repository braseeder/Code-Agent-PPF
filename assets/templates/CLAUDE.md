# {Project name}

{One sentence: what this project is and what "done" looks like.}

This project runs on PPF discipline: state lives in `_ppf/`, every workload ends in a review cycle, and the files below are the memory — read them, keep them current.

## Where things live

| File | What it holds |
|---|---|
| `_ppf/GOAL.md` | the contract: goal, success criteria, non-goals |
| `_ppf/WORKLOG.md` | workload counter + findings and resolutions per cycle |
| `_ppf/MISTAKES.md` | lessons ledger — required reading before starting any workload |
| `.claude/agents/` | the enabled reviewers: {list enabled agents here} |

## The rules

1. **Lean first.** Before writing anything, ask: does this code need to exist? Prefer delete > edit > stdlib > new code > dependency. Code the goal doesn't need is drift, even when it works.
2. **Start every workload by reading `_ppf/MISTAKES.md`.** Repeating a ledgered mistake is the one unforgivable bug.
3. **End every workload with the review cycle:**
   - Check the counter in `_ppf/WORKLOG.md` — every {3rd} workload, Nitpick joins.
   - Spawn each enabled reviewer with **fresh context**: give them the diff, `GOAL.md`, `MISTAKES.md` — never this conversation.
   - Feed all findings to the Planner (fresh context); apply its fix plan in order.
   - Update `WORKLOG.md` (counter, summary, findings, resolutions) and append new rules to `MISTAKES.md`. The cycle isn't done until both files are.
4. **Drift is a defect.** If the goal has genuinely changed, edit `GOAL.md` deliberately — never let the code quietly redefine it.

## Route by what just happened

| If | Then |
|---|---|
| starting a workload | read `_ppf/MISTAKES.md`, then work — lean |
| workload complete | run the review cycle (rule 3) |
| asked for status | read `_ppf/WORKLOG.md`, report counter + open deferrals |
| goal feels wrong | stop; propose an edit to `_ppf/GOAL.md` to the user |
