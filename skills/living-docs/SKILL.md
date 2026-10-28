---
name: living-docs
description: Capture durable, likely-to-repeat lessons as anti-patterns the same turn as the fix. Use when the user corrects you, the same class of mistake repeats, verify-work finds a gap, docs and code diverge, or when placing new agent rules/skills/launchers.
metadata:
  author: mattpua
  version: '1.6.0'
---

# living-docs

Keep agent-facing docs alive so the next session does not relearn the same lesson. Do not wait to be asked — “update the docs” from the user means this skill already failed.

## Process

1. Decide if the lesson will **probably repeat as an agent mistake class**. Default to **skip**. One-offs → short “no doc — won’t repeat” and stop.
2. Search matching + sibling anti-pattern topics for the same _class_. Prefer strengthen/merge over add.
2a. **Fix the class at the highest level that works**, prose last (from pstack `correct`). Assume the next agent sees only the files it opened and copies the nearest example:
    1. **Architecture** — one owner per piece of state, one supported way per task, shared modules instead of copies; delete the old way an agent would copy.
    2. **Types** — make the bad state unwritable.
    3. **Lint / CI check** whose error names the file or function to use instead.
    4. **Test** of the behavior.
    5. **Prose rule** (steps 3–4 below) only for judgment calls nothing can enforce.
    If a prose rule for this class already exists and the mistake happened again, that's the signal to move it up a level, not to reword it.
3. Write class-level “don’t X; prefer Y.” No post-mortems, magic numbers, or one-surface archaeology. Tuned values live in code.
4. Place the edit: anti-pattern topic, agent guide (invariants), or architecture map (topology). See `references/placement.md`.
5. If the ban is mechanical, codify it in the project linter same turn and cite the rule on the anti-pattern line.
6. Run the project’s near-dupe check if it has one. Reply: strengthened vs added (+ check passed). If skipped: say so.

**Skip (do not invent a rule) when any of these are true:**

- Upstream / dependency bug with a workaround already in code (pins, shims, named helpers) — the code is the doc; next agent won’t re-derive the forensics
- Mentions a specific GitHub issue, package version, stack frame, or one-time ceremony
- Would only fire if someone deletes the shipped fix — that’s not a repeating mistake class
- Explains *how we fixed it once* rather than a choice agents keep getting wrong in feature work

**Add / strengthen only when:** the next session is likely to make the same *product/code choice* again (wrong API shape, inventing a parallel helper, skipping a shared primitive, etc.).

**Done when:** the durable lesson is in the right artifact (or explicitly skipped as a one-off), and the reply names what changed.

## Pair with verify-work

Skipped / impossible proof / new evidence class → update the project’s acceptance/evidence table, and Call the Skill tool with `"living-docs"` when the miss will repeat as an anti-pattern.

## Setup

Copy `templates/anti-patterns-index.md` and topic templates into the project. Add `templates/standing-rule.md.example` if you want always-loaded paths.
