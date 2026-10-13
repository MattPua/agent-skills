---
name: living-docs
description: Capture durable, likely-to-repeat lessons as anti-patterns the same turn as the fix. Use when the user corrects you, the same class of mistake repeats, verify-work finds a gap, or docs and code diverge on a product decision.
metadata:
  author: mattpua
  version: "1.0.0"
---

# living-docs

Keep agent-facing docs alive so the next session does not relearn the same lesson. **Do not wait to be asked** — “update the docs” from the user means this skill already failed.

## When to write

Same turn as the code fix when any of:

- The user corrects copy / UX / behavior
- The same *class* of mistake repeats
- verify-work finds a skipped proof, cannot get concrete evidence, or learns a new acceptance class

## What to write

1. **Only if it will probably repeat.** Skip one-offs and rare ceremonies. Prefer class-level “don’t X; prefer Y.”
2. **Anti-patterns only** — not post-mortems, incident stories, chat history, magic numbers, timer values, or one-surface archaeology (“on the sidenav…”). Tuned values live in code.
3. **Prefer strengthen/merge** an existing bullet over adding a new line or topic.
4. **Put it in the matching topic file** — do not grow a flat mega-list.
5. **Architecture:** invariants → project agent guide; topology (how layers connect) → project architecture map — update same turn when those change.
6. **Mechanical bans:** if the lesson can be enforced by the project linter / AST plugin / import ban, codify it same turn and keep a one-line anti-pattern that cites the rule. Do not leave a recurring ban as prose-only when tooling can catch it.
7. If code and living docs diverge on a product decision, fix one in the same change — do not invent a third design.

## Dedupe gate (required)

Before and after every anti-pattern edit:

1. **Search** the matching topic **and** sibling topics for the same *class* (not just the same words). Skim core rules / agent guide so you don’t parallel-restate them.
2. **Prefer merge** if covered; add only if the class is new.
3. **Prove** with the project’s near-dupe check if it has one (`{{docs-dedupe-command}}`). In the reply: strengthened vs added (+ check passed when run).
4. A stop-hook “add a doc” nudge is **not** a license to append a near-copy.

## Reply contract

When you edit docs: say whether you **strengthened** or **added**, and that the dedupe check passed (if the project has one). If the correction was a true one-off: a short “no doc — won’t repeat” is enough — don’t invent a rule.

## Pair with verify-work

- Skipped / impossible proof / new evidence class → update the project’s acceptance/evidence table **and** anti-patterns when the miss will repeat.
- verify-work proves behavior; living-docs captures the lesson so the next agent does not skip the same proof class.

## Setup (once per project)

1. Copy `templates/anti-patterns-index.md` into the project (e.g. under `.cursor/rules/` or reuse docs).
2. Create topic files from `templates/topic-anti-patterns.mdc.example` (one file per topic you need).
3. Optionally install `templates/living-docs-rule.mdc.example` as an always-on Cursor rule.
4. Point the index at real paths; add a near-dupe script if you want CI/lint-staged enforcement.
5. Keep this skill generic — product paths and topic lists live in the project copies.

Optional enforcement: a stop hook that auto-continues once if a user correction landed without a docs edit (or without a linter ban when the lesson is mechanical).
