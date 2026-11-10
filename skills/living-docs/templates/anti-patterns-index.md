# Anti-patterns index

Prose fallback only. A line belongs here when the same class was corrected **before** and no linter, type, test, or review/verify checklist step can carry it (see living-docs gate). Mechanical bans → project linter.

**Do not dump new lines into a flat mega-list.** Open the matching topic file. Search siblings → strengthen/merge → run `{{docs-dedupe-command}}` when available.

| Topic | File | When |
| --- | --- | --- |
| Composition / DRY | `{{anti-patterns-dir}}/composition.md` | Screens, components, shared styles |
| Shell / layout | `{{anti-patterns-dir}}/shell-layout.md` | App shell, gutters, scroll foot |
| UI primitives | `{{anti-patterns-dir}}/primitives.md` | Shared UI duplicates |
| Auth / data | `{{anti-patterns-dir}}/auth-data.md` | Session, ACL, datastore patterns |
| Env / ops | `{{anti-patterns-dir}}/env-ops.md` | Env, seed, deploy, tooling |
| Verify / ship | `{{anti-patterns-dir}}/verify-ship.md` | Always loaded — proof before done |

Add/remove rows to match the project. Keep verify-ship (or equivalent) always loaded by your harness.

## Dedupe gate

1. Search matching + sibling topics for the same *class*.
2. Already here and the mistake happened anyway → the line failed: move it to a check or delete it. Don't reword or extend it.
3. Add only if new and gate 1 (repeat evidence) holds.
4. Prove with `{{docs-dedupe-command}}` when the project has one.
5. Reply: moved / deleted / added, or no doc.
