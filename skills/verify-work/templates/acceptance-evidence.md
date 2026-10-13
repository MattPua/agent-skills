# Acceptance criteria / evidence

**This table is the contract.** Match every row your change touches. When a new class of change appears, or a gap shows the table is weak — update this table same turn. Do not leave lessons only in chat.

Replace `{{…}}` placeholders with project-specific tools and paths.

| Kind of change | Acceptance criteria | Evidence (concrete proof) |
| --- | --- | --- |
| **Any (lint / imports)** | Touched files clean of banned patterns; new mechanical bans codified | `{{lint-command}}` on touched files; ban + docs same turn when adding a rule |
| **Living docs / anti-patterns** | Lesson is class-level; not a near-dupe of an existing topic | Search matching + sibling topics → strengthen/merge prefer add → `{{docs-dedupe-command}}` green if the project has one. Reply names strengthened vs added |
| **New component / helper / style / hook** | No parallel of an existing shared surface | Search `{{reuse-index}}` / shared UI / sibling screens → extend prefer invent → reply names extended vs invented |
| **UI feature / chrome** | Affordance works as a user would experience it | Browser (or equivalent): open `{{app-base-url}}`, exercise control, confirm outcome, **screenshot in chat** |
| **UI layout / scroll** | Last content usable; no phantom scrollbar where none should exist | Same session: scroll-to-end; **screenshot in chat** |
| **UI copy** | Copy matches product rules; no leftover tutorial blurbs | Look at the screen; **screenshot in chat** |
| **Backend / data function** | Runs correctly with real authz for the path changed | Live call (`{{backend-smoke}}`) |
| **HTTP / API route** | Auth reject + happy path behave as specified | Call the route; record status + body shape |
| **Database / schema / seed** | Data layer works after the change | Exercise **running** DB (`{{db-smoke}}`) |
| **Email** | Message sent and renderable | Trigger send, then open real message (`{{mail-ui}}`). Assert subject, key body, CTA |
| **CI / deploy / infra** | Pipeline or deploy path succeeds | Green CI / successful deploy / script exit 0 |
| **Packages (pure helpers)** | Types/tests pass for what changed | Package typecheck/test — only when no UI/HTTP/DB/email/deploy surface |
| **Git commit / push of prior runtime diffs** | Commit/push ≠ acceptance for runtime surfaces | Prove matching rows in the same transcript, or name gap + update this table |

## Before “done”

1. Ship the ask.
2. Prove every applicable row.
3. Name the concrete evidence in the reply (UI: include screenshot).
4. Gaps: name them; strengthen this table same turn.
