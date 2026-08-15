# Setup intake — decide once, write it down, never re-ask

Ask in dialogue, a few at a time. Answers land in the files named below; after setup, no agent should ever re-ask them.

## The goal (→ `_ppf/GOAL.md`)

1. In one or two sentences: what is this project, and what does "done" look like?
2. What are the concrete success criteria — the checks that prove it works? (aim for 3–5, testable)
3. What is this project explicitly **not**? (non-goals — the tangents to refuse later)

## The tier (→ roster recommendation, see `references/scaling.md`)

4. How long should this code live — days, months, or indefinitely?
5. Who runs it besides you? Does it take untrusted input, touch the network, or handle user data, secrets, or money?

From the answers, recommend Minimal / Standard / Full and say why. Then confirm the roster:

## The roster (→ which files land in `.claude/agents/`)

6. Security reviewer after every workload — yes/no? *(If yes → its Decisions: threat surface, secrets in play, data sensitivity, extra attention?)*
7. Goal-Alignment reviewer after every workload — yes/no? *(If yes → its Decisions: strictness, known temptations?)*
8. Nitpick breaker on a cadence — yes/no? *(If yes → its Decisions: every 3rd workload or tighter, standing targets, allowed to execute code?)*
9. Planner for cycles with findings — yes/no? *(Skip only on Minimal; without it, findings are fixed directly.)*

## Confirm before scaffolding

Restate: the goal in one line, the tier, the enabled roster with cadences. On approval, scaffold and fill every `{placeholder}` — a template left with placeholders is a setup bug.
