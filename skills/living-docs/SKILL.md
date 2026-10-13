---
name: living-docs
description: Capture durable, likely-to-repeat lessons as anti-patterns the same turn as the fix. Use when the user corrects you, the same class of mistake repeats, verify-work finds a gap, docs and code diverge, or when placing new agent rules/skills/launchers.
metadata:
  author: mattpua
  version: "1.3.0"
---

# living-docs

Keep agent-facing docs alive so the next session does not relearn the same lesson. **Do not wait to be asked** — “update the docs” from the user means this skill already failed.

Invoke by skill name (`living-docs`) however your harness triggers skills. Anti-pattern paths and lint commands live in the **project binding** / standing docs.

## When to write

Same turn as the code fix when any of:

- The user corrects copy / UX / behavior
- The same *class* of mistake repeats
- verify-work finds a skipped proof, cannot get concrete evidence, or learns a new acceptance class
- A review (e.g. critique-me) flags an agent-config follow-up / mis-placed artifact

## What to write

1. **Only if it will probably repeat.** Skip one-offs and rare ceremonies. Prefer class-level “don’t X; prefer Y.”
2. **Anti-patterns only** — not post-mortems, incident stories, chat history, magic numbers, timer values, or one-surface archaeology. Tuned values live in code.
3. **Prefer strengthen/merge** an existing bullet over adding a new line or topic.
4. **Put it in the matching topic file** — do not grow a flat mega-list.
5. **Architecture:** invariants → project agent guide; topology (how layers connect) → project architecture map — update same turn when those change.
6. **Mechanical bans:** if the lesson can be enforced by the project linter / AST plugin / import ban, codify it same turn and keep a one-line anti-pattern that cites the rule.
7. If code and living docs diverge on a product decision, fix one in the same change — do not invent a third design.

## Placement (when adding/changing agent artifacts)

Prefer **extend** an existing artifact over a new file.

| Intent | Prefer | Typical size |
| --- | --- | --- |
| Short principle / daily habit / decision guide | Project agent guide (`AGENTS.md`, `agents.md`, or equivalent) | short |
| Deep or path-scoped standing instructions | Project rules / instruction files your harness loads | medium |
| Repeating “don’t do X” for agents | Anti-patterns topic file | one-liners |
| Step-by-step how-to with clear when | Skill `SKILL.md` | thin checklist + links |
| Distinct isolated review / specialist role | Harness subagent / launcher config (if any) | specialized workflow |

Do **not** invent parallel doc types unless the user asked. Exact folders are project/harness-specific — use the project binding.

## Dedupe gate (required)

Before and after every anti-pattern edit:

1. **Search** the matching topic **and** sibling topics for the same *class*. Skim the agent guide / core rules so you don’t parallel-restate them.
2. **Prefer merge** if covered; add only if the class is new.
3. **Prove** with the project’s near-dupe check if it has one. In the reply: strengthened vs added (+ check passed when run).
4. A “add a doc” nudge is **not** a license to append a near-copy.

## Reply contract

When you edit docs: say whether you **strengthened** or **added**, and that the dedupe check passed (if the project has one). If the correction was a true one-off: a short “no doc — won’t repeat” is enough — don’t invent a rule.

## Pair with verify-work

- Skipped / impossible proof / new evidence class → update the project’s acceptance/evidence table **and** anti-patterns when the miss will repeat.
- verify-work proves behavior; living-docs captures the lesson so the next agent does not skip the same proof class.

## How projects wire this

| Piece | Role |
| --- | --- |
| **Standing project rule / binding** | Paths, lint command, anti-patterns dir |
| **This skill** | Procedure + placement + dedupe |
| **Optional guard nudge** | Reminder after corrections without a docs edit |

## Setup (once per project)

1. Copy `templates/anti-patterns-index.md` into the project.
2. Create topic files from `templates/topic-anti-patterns.md.example`.
3. Add `templates/standing-rule.md.example` as always-loaded instructions.
4. Add a near-dupe script if you want CI enforcement.
