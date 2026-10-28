---
name: verify-work
description: "Prove changes with concrete runtime evidence before claiming done. Use when finishing a task, claiming a fix, shipping UI/API/DB/email/CI work, or when \"verified\" / \"done\" / \"works\" is about to be said without fresh proof."
metadata:
  author: mattpua
  version: "1.7.0"
---

# verify-work

Concrete evidence that the change works — not a description of how it should work. Static checks (typecheck / lint / unit tests) are prerequisites, not proof, when a runtime path exists.

## Process

1. Ship the ask (behavior works).
2. Read the base contract, `references/evidence-classes.md`, then the project's **addons** file if it has one (its standing verify rule names the path). Apply every matching base row plus every matching addon row; an addon adds to or tightens a base row, never loosens it.
3. For every applicable row, run the evidence. UI needs live UI plus visual proof in the reply. When a project-local `verify-<app>` skill exists, Call the Skill tool with that skill for app/UI drives (feature map recipes beat ad-hoc clicks).
4. Name what you proved in the reply. Do not imply unverified layers are done.
5. If you cannot get proof: say so. Tighten or add a project addon row the same turn; if the gap would hit any project, propose a base-table row for this skill instead of keeping it local. Call the Skill tool with `"living-docs"` when the miss will repeat.

**Done when:** every applicable base and addon row has fresh evidence named in the reply, or gaps are explicitly named and the addons updated.

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

Don't copy the base table into the project. Copy `templates/project-addons.md` into standing project docs and fill in only what's specific to the project (tools, accounts, verify skills, project-only rows). Add `templates/standing-rule.md.example` as an always-loaded pointer to it. For a full launch/drive/feature-map harness, use **create-verification-skill**.
