# Anti-patterns index

For mistakes that will **probably repeat** — not every possible wrong turn. Mechanical bans → project linter; cite the rule on the line when adding one. Judgment / UX stays prose.

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
2. Prefer merge; add only if new.
3. Prove with `{{docs-dedupe-command}}` when the project has one.
4. Reply: strengthened vs added (+ check passed).
