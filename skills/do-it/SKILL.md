---
name: do-it
description: Single entry for rigorous work — pick a playbook and run the leaf skills for you.
disable-model-invocation: true
metadata:
  author: mattpua
  version: "1.0.0"
  credit: Entry-point pattern inspired by cursor/plugins pstack poteto-mode
---

# do-it

One entry point. Match the task to a playbook, copy its steps into a todolist **verbatim**, then run them. Leaf skills fire from the steps — you do not need to invoke `verify-work`, `debug-root-cause`, etc. by hand.

## Start

1. Match a playbook (table below). Ambiguous → state the best-fit choice in one line and proceed; user can redirect.
2. Open the playbook file. Copy its numbered steps into a todolist **before** task-specific todos.
3. A step you skip stays on the list with `skip: <reason>`. Silent drops are not allowed.
4. When a step names a skill, Call the Skill tool with that skill name (one skill per call). If a skill is not installed, do the step’s inline fallback and note the skip.
5. Large / cross-cutting / “trust it when I’m back” with no good fit → `playbooks/figure-it-out.md`.

## Playbooks

| Playbook | When | File |
| --- | --- | --- |
| Bug fix | Defect to reproduce, root-cause, fix, prove | `playbooks/bug-fix.md` |
| Feature | New or changed behavior | `playbooks/feature.md` |
| Investigation | Read-only: how/why/are we sure | `playbooks/investigation.md` |
| Pre-merge | Review (and fix) a diff or PR before merge | `playbooks/pre-merge.md` |
| Docs lesson | Capture a repeating lesson into agent docs | `playbooks/docs-lesson.md` |
| Figure it out | No narrower playbook fits; design then run | `playbooks/figure-it-out.md` |

## Non-negotiables

- **Prove it.** Before claiming done on a runtime path, Call the Skill tool with `"verify-work"`.
- **Root cause on bugs.** Call the Skill tool with `"debug-root-cause"` — no symptom patches.
- **Lessons that will repeat.** Call the Skill tool with `"living-docs"` the same turn.
- **Prefer simple.** Smallest change that meets the ask. Delete before adding layers.
- **Irreversible pause.** Force-push to shared branches, prod deploys, data deletion, customer messages — stop and confirm. Reversible work proceeds.

## Done

Playbook steps completed or explicitly skipped with reasons; any required leaf skills ran; reply names playbook + what was proved (or investigation answer with citations).
