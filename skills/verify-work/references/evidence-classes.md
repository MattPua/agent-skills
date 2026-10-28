# Evidence classes

The base contract for every project: how to prove each kind of change. A project's **addons** file (see `templates/project-addons.md`) supplies its tools and paths, adds rows that exist only there, and may tighten a row below. Addons never loosen a row.

Match every row your change touches.

| Kind of change | Acceptance criteria | Evidence (concrete proof) |
| --- | --- | --- |
| **Any (lint / imports / mechanical bans)** | Touched files clean of banned patterns; new mechanical bans codified | Project lint on touched files; when adding a rule that will keep mattering, codify it in the linter the same turn |
| **Living docs / anti-patterns** | Lesson is class-level and not a near-dupe of an existing topic | Search matching and sibling topics → strengthen or merge before adding. Reply names strengthened vs added |
| **New component / helper / style / hook** | No parallel of an existing shared surface | Search shared packages and sibling features → extend before inventing. Reply names extended vs invented |
| **UI feature / chrome** | The affordance works as a user would experience it | Live UI: exercise the control, confirm the outcome, **screenshot (or other visual proof) in the reply** |
| **Family of related pages** | A new or changed page matches its siblings (columns, value formats, actions, sheets/dialogs) | Open it next to its closest sibling in the browser and compare; **screenshot of both** |
| **Form / sheet that saves data** | Every submit ends visibly: saved (success message + data) or blocked (field error **and** a message). A silent click is a bug | Drive the form in the browser: empty submit, happy-path create then reopen, clear an optional field; for each, the on-screen result plus the stored row. Use the project's form/surface verify skill when one exists. **Screenshot of a blocked and a successful save** |
| **Page transitions / navigation** | Navigating paints the new page without waiting on its data; data loads after (skeleton OK) | From another page, click the nav; record requests between click and paint (target: none blocking); **screenshot** |
| **UI layout / scroll** | Last content reachable; no stray scrollbars; works at phone width | Same session: scroll to the end, check a narrow viewport; **visual proof in the reply** |
| **UI copy** | Copy matches product rules; no leftover placeholder or tutorial text | Look at the screen; **visual proof in the reply** |
| **Backend / data function** | Runs correctly with real authz for the path changed | Live call against the running backend with a real authenticated session |
| **HTTP / API route** | Auth reject and happy path behave as specified | Call the route; record status and body shape for both |
| **Database / schema / seed / migration** | Data layer works after the change; migrations produce the intended data | Exercise the **running** DB: apply the migration, query the changed data (counts, a sample row), read or write through the app path |
| **Email** | Message is sent and renders | Trigger the send, then **open the real message** (dev inbox or provider). Assert subject, key body, CTA. A send OK or HTML snapshot alone is not proof |
| **CI / deploy / infra** | Pipeline or deploy path succeeds | Green run, successful deploy or boot, or script exit 0 |
| **Pure package helpers** | Types and tests pass for what changed | Package typecheck/test — only when there is no UI/HTTP/DB/email/deploy surface |
| **Git commit / push of prior runtime diffs** | Commit/push is not acceptance for runtime surfaces | Prove the matching rows in the same turn, or name the gap |

## Before "done"

1. Ship the ask.
2. Prove every applicable row (base + project addons).
3. Name the concrete evidence in the reply (UI: include a screenshot).
4. Gaps: name them. If the gap is project-specific, add or tighten an addon row the same turn. If any project would hit it, propose the row for this base table.
