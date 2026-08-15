# Code-Agent-PPF

**Agent Pre-Project Framework** — a Claude skill you run at the start of a new project. It scaffolds the project's discipline layer and installs a review loop that keeps a coding agent honest over the long haul: lean code by default, fresh-context reviewers after every workload, and a mistakes ledger the agent has to learn from.

The premise: a coding agent left alone drifts. Code accretes that doesn't need to exist, the work slides away from the goal, and the same mistakes repeat because nothing remembers them. PPF makes the *project structure* enforce what the agent forgets — state lives in plain markdown files, so any session can cold-start from the files alone and a human can open any of them and see exactly where things stand.

## The loop

Every **workload** (one coherent unit of work — a feature, fix, or refactor) ends the same way:

```
workload complete
   ├─ every 3rd workload: Nitpick agent — edge cases, bugs, tries to break it
   ├─ Security agent — fresh context, reviews the diff for vulnerabilities
   └─ Goal-Alignment agent — fresh context, flags drift from GOAL.md
   ▼
Planner agent — fresh context, turns all findings into an ordered fix plan
   ▼
fixes applied → WORKLOG.md updated → lessons appended to MISTAKES.md
   ▼
next workload begins by reading MISTAKES.md
```

Reviewers always spawn with **fresh context** — they see the diff and the goal, never the conversation that produced the code. An agent reviewing its own reasoning rubber-stamps it; fresh eyes read what is actually there.

The roster scales to the project. At setup, PPF interviews you: what's the goal, how complex is this, do you want Security? Nitpick? A throwaway script might get zero reviewers; a product handling user data gets the full roster. Decisions are made once, written into each agent's file, and never re-asked.

## Install

**As a Claude Code skill:** copy this folder to `~/.claude/skills/code-agent-ppf/` (or `.claude/skills/code-agent-ppf/` inside a project), then ask Claude to "PPF this project" / "set up this project with PPF".

**Without installing:** clone the repo anywhere and tell your agent: *"Read SKILL.md in Code-Agent-PPF and set this project up with it."* Same result.

Either way, setup ends with your project containing a `CLAUDE.md` (routing + loop rules), a `_ppf/` folder (`GOAL.md`, `WORKLOG.md`, `MISTAKES.md`), and one `.claude/agents/` file per enabled reviewer.

## Layout

```
Code-Agent-PPF/
├─ SKILL.md               the method: principles, setup flow, the loop, guardrails
├─ references/
│  ├─ lean-code.md        the "does this code need to exist?" doctrine
│  ├─ review-loop.md      full loop spec: sequencing, finding format, logging
│  └─ scaling.md          the three tiers — sizing the roster to the project
├─ agents/                Claude Code subagent definitions, copied into projects
│  ├─ security-reviewer.md
│  ├─ goal-alignment.md
│  ├─ planner.md
│  └─ nitpick.md
└─ assets/templates/      scaffold starters: CLAUDE.md, questionnaire, GOAL,
                          WORKLOG, MISTAKES
```

Structure-as-architecture approach inspired by [ICM](https://github.com/RinDig/icm-architect) (Interpretable Context Methodology): folders carry process, files carry state, and routing files stay small and point at content instead of holding it.

MIT licensed.
