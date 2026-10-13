# Evidence classes (default)

Use when the project has no acceptance/evidence table, or to extend that table.

| Kind of change | Evidence (concrete proof) |
| --- | --- |
| Lint / imports / mechanical bans | Lint clean on touched files; new bans codified in the project linter when they will keep mattering |
| New component / helper / style / hook | Dedupe gate: search existing shared surfaces → extend prefer invent → reply names extended vs invented |
| UI feature / chrome | Live UI: exercise the control; **screenshot (or visual proof) in the reply** |
| UI layout / scroll | Same session: scroll-to-end; **visual proof in the reply** |
| UI copy | Look at the screen; **visual proof in the reply** |
| Backend / data functions | Live call against the running backend with real authz for the path changed |
| HTTP / API route | Call the route; record status + body shape (auth reject + happy path when both matter) |
| Database / schema / seed | Exercise the **running** DB (read/write, seed/reset, or query of changed data) |
| Email | Trigger send, then **open the real message**. Send OK or HTML snapshot alone ≠ proof |
| CI / deploy / infra | Green run, successful deploy/boot, or script exit 0 |
| Pure package helpers | Package typecheck/test — only when no UI/HTTP/DB/email/deploy surface |
| Git commit / push of prior runtime diffs | Commit/push is not acceptance — prove matching rows in the same turn, or name the gap and update the table |
