# Suspect state before code

Use this when the failure is intermittent, only after restart, env-specific, or “nothing changed.”

## Check in order

1. **Process / locks** — leftover servers, watchers, lockfiles, port holders, stale agents.
2. **Caches / build artifacts** — `.turbo`, `node_modules/.cache`, compiled output, storybook/vite caches. Wipe only what you can name; re-run the failing command after.
3. **Config / env** — wrong `.env*`, missing keys, pointing at the wrong deployment/DB, stale generated env.
4. **Persistent data** — local DB rows, seed/reset residue, corrupted JSON/SQLite, migrated-but-old schema assumptions.
5. **Generated code** — out-of-date clients, route trees, API types; regenerate before “fixing” callers.
6. **Time / clock / locale** — flaky date tests, TZ mismatches.

## Rule of thumb

If deleting or resetting a state file restores behavior, the fix is validation, migration, or clearer ownership of that state — not a silent retry in application code.
