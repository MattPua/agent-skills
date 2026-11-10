---
name: living-docs
description: "Decide whether a correction earns a durable fix, and put it where it will fire (lint, types, test, review checklist) — prose last. Default is to write nothing. Use when the same class of mistake has been corrected before, the user asks to record a lesson, verify-work finds a gap, docs and code diverge, or when placing new agent rules/skills/launchers."
metadata:
  author: mattpua
  version: "2.0.0"
---

# living-docs

Most corrections are fixed in code and need no doc. Prose lists that agents are expected to read before writing code rarely prevent the next mistake; they mostly get written, not used. Write less, and put what you do write where it runs.

## Gate (default: write nothing)

Fix the code. Then write a durable lesson **only if all three hold**:

1. **Evidence it repeats.** The same *class* was corrected before (earlier session, commit, review finding), or the user asks you to record it. "It will probably repeat" is a guess, not evidence. First-time corrections → no doc.
2. **There is a place it will fire.** Use the first that works — assume the next agent sees only the files it opened and copies the nearest example:
    1. **Architecture** — one owner per piece of state, one supported way per task; delete the old way an agent would copy.
    2. **Types** — make the bad state unwritable.
    3. **Lint / CI check** whose error names what to use instead.
    4. **Test** of the behavior.
    5. **A step in a checklist that runs at the moment** — a review skill (e.g. critique-me, code-smells) or a verify skill's done-check. Not a list read "before writing code".
    6. **Prose rule** — only for a judgment call none of the above can carry, and only if no prose rule for the class exists yet.
3. **No existing prose rule for this class.** If one exists and the mistake happened anyway, the prose failed: move it up a level (2.1–2.5) or delete it. Do not reword it, extend it, or add a narrower sibling.

Reply in one line either way: what changed and where, or "no doc — <reason>".

## Route by kind

- **Code shape** any project would hit (duplication, reinvention, unnamed concept, needless layer, cast, dead branch, comment noise, disproportion) → `code-smells` → `references/examples.md` under its family (upstream). Code-smells runs at review time, which is where these get caught.
- **Mechanical ban** (import, package, JSX tag, identifier) → the project linter.
- **Project/product decision** (behavior, copy, policy, contracts, platform quirks) → the project's review or verify checklist first; anti-pattern topic file only as 2.6.
- **How the agent works** (scope, asking first, research before proposing) → one line in the agent guide (`AGENTS.md`), not a list.

## Never write

- Upstream / dependency bugs with a workaround already in code — the code is the doc.
- Anything naming a specific issue, package version, stack frame, file, feature, or copy from the case at hand.
- Rules that only fire if someone reverses a shipped fix.
- Post-mortems: how we fixed it once, tuned values, chat archaeology.
- One-off taste calls (a rejected color, a removed subtitle) unless gate 1 holds.

## Pair with verify-work

Skipped / impossible proof / new evidence class → tighten or add a row in the project's verify-work addons (or propose a base row upstream when any project would hit it). That is a checklist that runs at the moment — it passes gate 2.

## Placement

See `references/placement.md`. Prefer extending an existing artifact; prefer deleting a failed rule over adding a new one.

## Setup

Copy `templates/anti-patterns-index.md` and topic templates into the project if it wants a prose fallback. Add `templates/standing-rule.md.example` for an always-loaded pointer. Do not add a stop hook that forces a doc edit after corrections — it inflates writes without improving hit rate.
