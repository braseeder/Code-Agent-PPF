# Scaling — sizing the roster to the project

The intake's job is to land the project on a tier. Over-installing is ceremony; under-installing is how drift and vulnerabilities get in. Recommend a tier from the answers, say why, and let the user override.

## The three tiers

| | **Minimal** | **Standard** | **Full** |
|---|---|---|---|
| For | throwaway scripts, prototypes, experiments, single-file tools | most real projects: apps, services, libraries someone will use | products handling user data, auth, payments, or public traffic |
| Goal-Alignment | optional | ✔ every workload | ✔ every workload |
| Security | — | ✔ every workload | ✔ every workload |
| Planner | — (fix findings directly) | ✔ every cycle with findings | ✔ every cycle with findings |
| Nitpick | — | ✔ every 3rd workload | ✔ every 2nd–3rd workload |
| `_ppf/` files | `GOAL.md` only | all three | all three |

Every tier keeps the lean-code doctrine and `GOAL.md` — those cost nothing and are the point.

## Placing a project

Signals for **Minimal**: you'd delete the repo without sadness; no other users; no secrets, no network input; expected life under a week. The discipline layer here is just a written goal — a Minimal project with `GOAL.md` still beats a folder of mystery scripts.

Signals for **Standard** (the default when unsure): the code will outlive the week; someone other than the author runs it; it has dependencies, I/O, or state worth protecting. Standard is the tier the loop was designed around.

Signals for **Full**: untrusted input, authentication, personal data, money, or anything internet-facing. Full is Standard with the paranoia turned up: Nitpick may run more often, and Security's Decisions section gets a real threat surface written into it, not a placeholder.

## Changing tier later

Tiers are a starting point, not a sentence. A prototype that survives becomes Standard: run PPF setup again, answer only the roster questions, copy in the missing agents. Record the change in `WORKLOG.md` so the history explains why reviews start appearing at workload 12. Downgrading works the same way — delete the agent files, note it in the log.
