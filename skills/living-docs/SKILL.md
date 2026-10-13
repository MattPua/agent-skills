---
name: living-docs
description: Capture durable, likely-to-repeat lessons as anti-patterns the same turn as the fix. Use when the user corrects you, the same class of mistake repeats, verify-work finds a gap, docs and code diverge, or when placing new agent rules/skills/launchers.
metadata:
  author: mattpua
  version: "1.4.0"
---

# living-docs

Keep agent-facing docs alive so the next session does not relearn the same lesson. Do not wait to be asked — “update the docs” from the user means this skill already failed.

## Process

1. Decide if the lesson will **probably repeat**. One-offs → short “no doc — won’t repeat” and stop.
2. Search matching + sibling anti-pattern topics for the same *class*. Prefer strengthen/merge over add.
3. Write class-level “don’t X; prefer Y.” No post-mortems, magic numbers, or one-surface archaeology. Tuned values live in code.
4. Place the edit: anti-pattern topic, agent guide (invariants), or architecture map (topology). See `references/placement.md`.
5. If the ban is mechanical, codify it in the project linter same turn and cite the rule on the anti-pattern line.
6. Run the project’s near-dupe check if it has one. Reply: strengthened vs added (+ check passed).

**Done when:** the durable lesson is in the right artifact (or explicitly skipped as a one-off), and the reply names what changed.

## Pair with verify-work

Skipped / impossible proof / new evidence class → update the project’s acceptance/evidence table, and Call the Skill tool with `"living-docs"` when the miss will repeat as an anti-pattern.

## Setup

Copy `templates/anti-patterns-index.md` and topic templates into the project. Add `templates/standing-rule.md.example` if you want always-loaded paths.
