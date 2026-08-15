---
name: code-agent-ppf
description: Agent Pre-Project Framework — run at the start of a new project to scaffold its structure and install a lean-code review discipline. Installs fresh-context Security and Goal-Alignment reviewers that run after every workload, a fresh-context Planner that turns their findings into an ordered fix plan, a Nitpick breaker that hunts edge cases every third workload, and a mistakes ledger the coding agent must learn from. Use when the user says "PPF this", "set up this project", "start a new project with PPF", "install the review loop", or asks to scaffold a fresh repo with agent discipline.
---

# Code-Agent-PPF

Set up a project so the structure enforces discipline the agent would otherwise forget: code stays lean, every workload gets reviewed by fresh eyes, findings become fixes, and mistakes become rules. State lives in plain files a human can open — the project itself always knows what workload it is on, what has been caught, and what must never happen again.

## The five principles

1. **Does this code need to exist?** Ask before every addition. Prefer deleting over adding, editing over creating, the standard library over a dependency. The leanest implementation that meets the goal wins. Doctrine: [references/lean-code.md](references/lean-code.md).
2. **Fresh eyes or no eyes.** Review agents spawn with fresh context — they read the diff and the goal, never the conversation that produced the code. An agent reviewing its own reasoning rubber-stamps it; a fresh agent reads what is actually there.
3. **The filesystem is the state machine.** The workload counter, review findings, and lessons live in `_ppf/` as markdown. Any session — today's or next month's — can cold-start from the files alone.
4. **Iterate toward the goal.** `GOAL.md` is the contract. Every workload moves toward it or is drift, and drift is a defect the Goal-Alignment reviewer exists to catch.
5. **Mistakes are data.** Every caught defect becomes a one-line rule in `MISTAKES.md`. The ledger is required reading at the start of every workload — the same mistake twice means the rule was written badly, so rewrite it.

## Setup flow (first use in a project)

**1. Intake.** Ask the questions in [assets/templates/questionnaire.md](assets/templates/questionnaire.md), a few at a time, in dialogue. You are extracting: the goal and its success criteria, the project's complexity tier, and which review agents to enable. Do not skip the intake and guess — the whole point is that these decisions are made once, out loud, and written down.

**2. Size the roster.** Match complexity to agents using [references/scaling.md](references/scaling.md). A throwaway script does not need a security reviewer; a product handling user data needs the full roster. Recommend a tier, let the user override.

**3. Scaffold.** Create in the project root:

```
project/
├─ CLAUDE.md            entry file: routing + the loop rules   (from assets/templates/CLAUDE.md)
├─ _ppf/
│  ├─ GOAL.md           goal, success criteria, non-goals      (from assets/templates/GOAL.md)
│  ├─ WORKLOG.md        workload counter + findings per cycle  (from assets/templates/WORKLOG.md)
│  └─ MISTAKES.md       lessons ledger                         (from assets/templates/MISTAKES.md)
└─ .claude/agents/      one file per ENABLED agent             (from agents/)
```

Fill every `{placeholder}` from the intake answers. Do not scaffold folders for the project's own code — that is the project's business; PPF only installs the discipline layer.

**4. Install the agents.** Copy each *enabled* agent definition from [agents/](agents/) into the project's `.claude/agents/`. Each file ends with a **Decisions** section — fill it from the intake (threat surface for Security, cadence for Nitpick, etc.). Disabled agents are not copied; an agent that exists gets run.

**5. Cold-start test.** Validate the way ICM validates with a walk test: pretend to be an agent with no memory. From `CLAUDE.md` alone (plus at most two more reads) you must be able to answer — what is the goal, what workload are we on, which reviewers are enabled, what mistakes must not repeat, and what happens when the current workload ends. If any answer needs the conversation history, the files are wrong — fix the files.

## The workload loop

A **workload** is one coherent unit of work — a feature, a fix, a refactor — that ends with the project in a working state. The loop, fully specified in [references/review-loop.md](references/review-loop.md):

```
workload N complete
   │
   ├─ every 3rd workload: Nitpick agent (fresh context) — edge cases, breakage attempts
   ├─ Security agent (fresh context) — reviews the diff for vulnerabilities
   └─ Goal-Alignment agent (fresh context) — diffs the work against GOAL.md, flags drift
   │
   ▼
Planner agent (fresh context) — reads all findings, produces an ordered fix plan
   │
   ▼
fixes applied → WORKLOG.md updated (counter, findings, resolutions)
             → MISTAKES.md appended (one rule per caught defect)
   │
   ▼
workload N+1 begins by reading MISTAKES.md
```

Reviews run at workload boundaries, not mid-work. Findings without a fix plan are noise; a fix plan without logged lessons is amnesia — the loop is only complete when both files are updated.

## Guardrails

- **Don't over-install.** The roster scales down as well as up. Installing four reviewers on a 100-line script is ceremony, not discipline — that is what the intake tier question is for.
- **The loop reviews workloads, not keystrokes.** If the agent is pausing to run reviewers on every small edit, the workload boundary was drawn too tight.
- **Never skip the logging step.** Skipping `WORKLOG.md`/`MISTAKES.md` updates once breaks the counter and the learning loop silently — the third workload never triggers Nitpick, and the same mistakes recur.
- **Lean applies to PPF itself.** If a project outgrows a template file or an agent's Decisions, edit the project's copy. The framework is a starting point, not an authority.

## References

- [references/lean-code.md](references/lean-code.md) — the "does this code need to exist?" doctrine. Read when writing the project `CLAUDE.md`, or whenever a lean call is contested.
- [references/review-loop.md](references/review-loop.md) — the full loop spec: sequencing, fresh-context rules, finding format, planner duties, logging. Read at setup step 4 and whenever the loop is run.
- [references/scaling.md](references/scaling.md) — the three tiers and how to pick one. Read at setup step 2.
- [agents/](agents/) — the four agent definitions, in Claude Code subagent format.
- [assets/templates/](assets/templates/) — copyable starters for the scaffold.
