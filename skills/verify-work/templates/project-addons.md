# Verification addons — {{project}}

Project-specific additions to the base contract in the installed **verify-work** skill (`references/evidence-classes.md`). Prove every applicable base row **and** every applicable row here. Rows here add to or tighten a base row; they never loosen one.

Keep this file short: only what is true of this project and nowhere else. A row any project would need belongs in the base table — propose it upstream instead.

## Tools

| Need | This project |
| --- | --- |
| Lint touched files | `{{lint-command}}` |
| Typecheck / tests | `{{typecheck-command}}` / `{{test-command}}` |
| Run the app | `{{start-command}}` (ready when `{{ready-signal}}`) |
| Drive the UI | `{{browser-or-harness}}` |
| Test accounts | `{{seed-users-and-where-the-password-lives}}` |
| Local DB | `{{db-url-or-query-tool}}` |
| Dev inbox | `{{dev-email-inbox}}` |
| Verify skills | `{{verify-<app> / verify-<surface> skills and what each covers}}` |

## Tightened base rows

| Base row | Extra criteria / evidence here |
| --- | --- |
| `{{base row name}}` | `{{what this project additionally requires}}` |

## Project-only rows

| Kind of change | Acceptance criteria | Evidence (concrete proof) |
| --- | --- | --- |
| **{{project-specific change}}** | `{{criteria}}` | `{{evidence}}` |
