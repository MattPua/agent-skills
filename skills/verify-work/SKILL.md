---
name: verify-work
description: "Prove changes with concrete runtime evidence before claiming done. Use when finishing a task, claiming a fix, shipping UI/API/DB/email/CI work, or when \"verified\" / \"done\" / \"works\" is about to be said without fresh proof."
metadata:
  author: mattpua
  version: "1.6.0"
---

# verify-work

Concrete evidence that the change works — not a description of how it should work. Static checks (typecheck / lint / unit tests) are prerequisites, not proof, when a runtime path exists.

## Process

1. Ship the ask (behavior works).
2. Open the project’s acceptance/evidence table if it exists; otherwise use `references/evidence-classes.md` and add missing rows to the project table same turn.
3. For every applicable row, run the evidence. UI needs live UI plus visual proof in the reply. When a project-local `verify-<app>` skill exists, Call the Skill tool with that skill for app/UI drives (feature map recipes beat ad-hoc clicks).
4. Name what you proved in the reply. Do not imply unverified layers are done.
5. If you cannot get proof: say so, strengthen the project table same turn, and Call the Skill tool with `"living-docs"` when the miss will repeat.

**Done when:** every applicable row has fresh evidence named in the reply, or gaps are explicitly named and the table updated.

## Gaps

Do not write gaps off. Prefer documenting the exact evidence steps to run once unblocked over hoping the human will QA.

### Missing harness → prompt the user

Before driving anything, check what covers the surface you're asked to verify (an app, a kind of flow such as forms or checkout, a CLI): a project-local `verify-<app>` / `verify-<surface>` skill, or a feature-map entry inside one.

- **Nothing covers it** → still get the best ad-hoc proof you can this turn, then **ask the user** whether to build a harness for that surface. Name the surface and what was ad-hoc about this proof. Use the harness's question tool if it has one (one question, "create a verify-<surface> skill" as the suggested answer); otherwise end your reply with the question. On yes, call the Skill tool with `"create-verification-skill"`. Don't build one unasked (it's a multi-step task), and never stay silent about the gap.
- **A skill exists but its map lacks this feature, or a mapped recipe no longer matches the app** → call the Skill tool with `"maintain-verification-skill"` for that feature, then drive it.
- Ask once per surface per session. If the user declines, don't ask again for that surface.

## Anti-patterns

See `references/anti-patterns.md`.

## Setup

Copy `templates/acceptance-evidence.md` into standing project docs; fill tools/paths. Optionally add `templates/standing-rule.md.example` as an always-loaded pointer. For a full launch/drive/feature-map harness, use **create-verification-skill**.
