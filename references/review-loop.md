# The review loop — full spec

The loop runs at every workload boundary. It is the project's immune system: fresh-context reviewers find problems, a fresh-context planner orders the fixes, the files remember the lessons.

## What counts as a workload

One coherent unit of work that ends with the project in a working state: a feature, a bug fix, a refactor, a migration step. Not a keystroke, not a whole milestone. If reviews are firing constantly, the boundary is too tight; if a "workload" spans days of changes, it is too loose. When in doubt: one workload ≈ one meaningful commit.

## Sequence

1. **Workload ends.** Code compiles / tests pass / the thing works. Note the diff range (e.g. the commit or the files touched).
2. **Check the counter** in `_ppf/WORKLOG.md`. If this is the 3rd workload since Nitpick last ran (and Nitpick is enabled), Nitpick joins this round.
3. **Spawn reviewers in parallel, fresh context.** Each enabled reviewer gets: the diff (or list of changed files), `_ppf/GOAL.md`, `_ppf/MISTAKES.md`, and its own agent file. **Never** the conversation history, the reasoning behind the changes, or another reviewer's output. Fresh context is the mechanism — a reviewer that knows why the code was written will excuse it.
4. **Collect findings.** Each reviewer returns findings in the standard format (below), or explicitly "no findings."
5. **Spawn the Planner, fresh context.** Input: all findings, `GOAL.md`, `MISTAKES.md`. Output: an ordered fix plan — what to fix now, what to defer (with reason), what to reject (with reason). The planner resolves conflicts between reviewers and keeps the fixes lean (a finding does not license a rewrite).
6. **Apply the fixes** in the main session, in the planner's order. Fixing is itself work but not a new workload — it belongs to this cycle.
7. **Log.** Update `_ppf/WORKLOG.md`: increment the counter, record the workload's one-line summary, findings count, and resolutions. Append to `_ppf/MISTAKES.md`: one rule per caught defect (format in that file). A cycle without the logging step did not happen.
8. **Next workload begins by reading `MISTAKES.md`** — the ledger is the project's accumulated judgment, and it is only worth anything if it is actually read.

## Finding format

Every reviewer reports findings the same way, so the planner can merge them:

```
- [SEVERITY] file:line — what is wrong, in one sentence.
  Why it matters: concrete failure or drift scenario.
  Suggested direction: (optional, one line — the planner decides the actual fix)
```

Severity: `CRITICAL` (exploitable, data loss, goal actively harmed) / `MAJOR` (real defect or clear drift) / `MINOR` (lean violation, edge case, papercut). Reviewers report what they see and stop — no fixing, no redesigning.

## Planner duties

The planner exists because findings arrive from different reviewers with different priorities and no shared view. With fresh context it:

- Merges and de-duplicates findings; where two reviewers conflict, decides and says why.
- Orders fixes: CRITICALs always now; MAJORs now unless there's a stated reason to defer; MINORs batched, deferred, or rejected — but always dispositioned, never silently dropped.
- Keeps fixes lean: the smallest change that resolves the finding. A finding is not an invitation to rebuild the module.
- Flags patterns: three findings with the same shape are one structural problem — say so, and propose the single fix.

## Nitpick cadence

Nitpick runs every 3rd workload by default (set in its Decisions at setup). It is deliberately not in every round: its job — edge cases, malformed input, race windows, "what happens if I do this twice" — is deep and slow, and running it constantly would blur it into the per-workload reviewers. When it runs, it joins step 3 like the others, same finding format, same planner downstream.

## Skipping the loop

Allowed only when the workload touched no code paths — pure docs, comments, formatting. Log it in `WORKLOG.md` anyway (`reviews: skipped — docs only`) so the counter stays honest. Everything else gets reviewed, including "trivial" changes; trivial changes with skipped reviews are where regressions live.
