# Lean code — the doctrine

The question is always the same: **does this code need to exist?** Every line is a liability — it must be read by every future agent, reviewed by every reviewer, and maintained forever. Code earns its place by being necessary for the goal in `_ppf/GOAL.md`; nothing else earns a place.

## The order of preference

When work needs doing, reach for these in order. Each step down is a cost increase — take it only when the step above genuinely cannot carry the work:

1. **Delete.** The best fix removes code. If a feature, branch, or abstraction no longer serves the goal, deleting it *is* the workload.
2. **Edit what exists.** Extend a function that already does 80% of the job before writing its sibling.
3. **Use the standard library / platform.** The language and its stdlib already solve most solved problems.
4. **Write new code.** Smallest thing that meets the goal. No scaffolding for futures that may not come.
5. **Add a dependency.** Last resort. A dependency is code you now own but didn't write and can't shrink. It must replace substantial code you would otherwise have to write *and* maintain.

## Tests before adding anything

- Does the goal fail without it? If the goal is met without this code, the code is drift.
- Is this solving a problem we have, or a problem we imagine? Speculative generality — plugin systems with one plugin, config for values that never change, interfaces with one implementation — is the most common bloat. Build for today's requirement; refactor when the second case actually arrives.
- Could this be a smaller diff? Prefer the change that touches fewer files and adds fewer concepts, even if the bigger one is more "elegant."
- Is there already a home for this? A second helper doing almost the same thing as an existing one is a bug in slow motion. One home per behavior.

## What lean is not

- **Not code golf.** Fewer *concepts*, not fewer characters. A clear five-line function beats a clever one-liner.
- **Not skipping error handling.** Handling failure that can actually occur is part of the requirement. What's banned is handling failures that cannot occur in this system.
- **Not refusing structure.** When the second or third real case arrives, extracting the abstraction is exactly the right move — lean means not extracting it before the cases exist.

## For reviewers

Every reviewer in the loop shares this doctrine. "This works but doesn't need to exist" is a valid, reportable finding — file it like a bug, severity by how much weight the dead code adds.
