---
name: verify-work
description: Prove changes with concrete runtime evidence before claiming done. Use when finishing a task, claiming a fix, shipping UI/API/DB/email/CI work, or when "verified" / "done" / "works" is about to be said without fresh proof.
metadata:
  author: mattpua
  version: "1.4.0"
---

# verify-work

Concrete evidence that the change works — not a description of how it should work. Static checks (typecheck / lint / unit tests) are prerequisites, not proof, when a runtime path exists.

## Process

1. Ship the ask (behavior works).
2. Open the project’s acceptance/evidence table if it exists; otherwise use `references/evidence-classes.md` and add missing rows to the project table same turn.
3. For every applicable row, run the evidence. UI needs live UI plus visual proof in the reply.
4. Name what you proved in the reply. Do not imply unverified layers are done.
5. If you cannot get proof: say so, strengthen the project table same turn, and Call the Skill tool with `"living-docs"` when the miss will repeat.

**Done when:** every applicable row has fresh evidence named in the reply, or gaps are explicitly named and the table updated.

## Gaps

Do not write gaps off. Prefer documenting the exact evidence steps to run once unblocked over hoping the human will QA.

## Anti-patterns

See `references/anti-patterns.md`.

## Setup

Copy `templates/acceptance-evidence.md` into standing project docs; fill tools/paths. Optionally add `templates/standing-rule.md.example` as an always-loaded pointer.
