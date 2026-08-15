# Worklog

Workload counter: **0**
Nitpick last ran: workload 0 (cadence: every {3rd})

The counter is the loop's clock — it decides when Nitpick fires and lets any session cold-start. Update it every cycle, no exceptions; a skipped update silently breaks the cadence.

## Cycles

Newest first. One entry per workload, written at the end of its review cycle:

### Workload {N} — {date} — {one-line summary}
- Changed: {files or commit range}
- Reviews: {who ran — e.g. security, alignment, nitpick / "skipped — docs only"}
- Findings: {count by severity, or "none"}
- Fixed now: {what}
- Deferred: {what + trigger for revisit, or "—"}
- Rejected: {what + reason, or "—"}
- Ledger: {rules added to MISTAKES.md, or "—"}

<!-- template for the next entry — copy, fill, keep newest first -->
