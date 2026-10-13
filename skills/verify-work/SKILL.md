---
name: verify-work
description: Prove changes with concrete runtime evidence before claiming done. Use when finishing a task, claiming a fix, shipping UI/API/DB/email/CI work, or when "verified" / "done" / "works" is about to be said without fresh proof.
metadata:
  author: mattpua
  version: "1.0.0"
---

# verify-work

**Principle:** Verification is concrete evidence that the change works — not a description of how it should work, and not static checks alone.

**Static checks** (typecheck / lint / unit tests) are **prerequisites, not proof**, when a runtime path exists.

## Before saying done

1. **Ship the ask** — behavior works.
2. **Match every applicable row** in the project's acceptance/evidence table (see Setup).
3. **Say what you proved** — name the concrete evidence in the reply. For UI: include a screenshot in the chat reply, not only a description.
4. **Do not imply** unverified layers are done.

## Evidence classes (default)

Use the project's table when present. If missing, use this baseline and add missing rows to the project table same turn:

| Kind of change | Evidence (concrete proof) |
| --- | --- |
| Lint / imports / mechanical bans | Lint clean on touched files; new bans codified in the project linter when they will keep mattering |
| New component / helper / style / hook | Dedupe gate: search existing shared surfaces → extend prefer invent → reply names extended vs invented |
| UI feature / chrome | Live browser (or equivalent): exercise the control; **screenshot in chat** |
| UI layout / scroll | Same session: scroll-to-end; **screenshot in chat** |
| UI copy | Look at the screen; **screenshot in chat** |
| Backend / data functions | Live call against the running backend with real authz for the path changed |
| HTTP / API route | Call the route; record status + body shape (auth reject + happy path when both matter) |
| Database / schema / seed | Exercise the **running** DB (read/write, seed/reset, or query of changed data) |
| Email | Trigger send, then **open the real message** (local mail UI or provider dashboard / inbox). Send OK or HTML snapshot alone ≠ proof |
| CI / deploy / infra | Green run, successful deploy/boot, or script exit 0 |
| Pure package helpers | Package typecheck/test — enough **only** when no UI/HTTP/DB/email/deploy surface |
| Git commit / push of prior runtime diffs | Commit/push is not acceptance — prove matching rows in the same transcript, or name the gap and update the table |

## Gaps

When you **cannot** get concrete proof:

1. Do not write it off.
2. Say what was not verified and why.
3. Same turn: strengthen the project's acceptance/evidence table (and living-docs / anti-patterns when the miss will repeat).
4. Prefer documenting the exact evidence steps to run once unblocked over hoping the human will QA.

## Anti-patterns

- Claiming done because a later review agent / critique pass will catch it
- Claiming done after typecheck/lint/unit only when a runtime path exists and was not evidenced
- “Verified” UI without live UI **or** without a screenshot in the chat reply
- “Verified” backend/HTTP without a live call
- “Verified” DB without exercising the running database
- “Verified” email because send returned OK — without opening the message
- “Verified” CI/deploy without a successful run
- Waiting for the user to paste an overlay, stack trace, screenshot, email, or failed deploy as QA
- Learning a new evidence class and leaving it only in chat

## Setup (once per project)

1. Copy `templates/acceptance-evidence.md` into the project's agent docs (e.g. `.cursor/rules/verify-work.mdc` or a section in `AGENTS.md`).
2. Optionally copy `templates/verify-rule.mdc.example` as an always-on Cursor rule.
3. Replace placeholders with project tools, routes, and mail/CI names.
4. Keep this skill generic — product-specific rows live in the project copy.

Optional enforcement: a stop hook that nudges once if runtime edits landed without transcript evidence of matching proof.
