---
name: debug-root-cause
description: >-
  Trace failures to root cause and fix there — not symptom patches. Use when
  debugging errors, unexpected behavior, flaky tests, restart-only bugs, or when
  stuck after a first guess. Triggers: debug, fix this, what's wrong, investigate,
  still failing, flaky, can't reproduce.
metadata:
  author: mattpua
  version: "1.0.0"
  credit: Inspired by poteto/noodle debugging (https://www.skills.sh/poteto/noodle/debugging)
---

# debug-root-cause

Trace every problem to its root cause. Symptom patches (extra guards, retries, bypass flags) leave the bug alive and make the next debug harder.

## Process

1. **Reproduce.** Run the failing path. Capture the full error, stack, exit code, and relevant logs. If you cannot reproduce, gather evidence of when/where it fails before changing code — you cannot verify a fix you cannot see fail.
2. **Read the evidence.** Treat the message, stack, line numbers, and nearby logs as data. Read all of it before hypothesizing. Open the cited files at the cited lines.
3. **Suspect state before code.** Especially for restart-only, env, cache, or “worked yesterday” bugs. Code did not change between runs; state often did. Check lock files, caches, generated artifacts, local DB/seed data, env files, process leftovers, and config validity before rewriting logic. See `references/state-before-code.md`.
4. **Isolate.** Narrow to file → function → line (or request → handler → dependency). Prefer binary search / minimal repro over reading the whole tree. Change one variable at a time.
5. **Find root cause.** Ask “why?” until a fix at that layer removes the class of failure — not just the symptom.
   - Test fails → mock wrong → interface changed → **align type/contract with runtime**
   - Crash on null → missing guard → source returns empty → **handle empty at the source (or make it impossible)**
   - Build/import fail → cycle or wrong export → **fix the module boundary**
6. **Fix the cause.** Prefer making the bad state unrepresentable over silencing it.
7. **Verify.** Call the Skill tool with `"verify-work"`. Compiling or “looks right” is not enough when a runtime path exists.
8. **Sweep the pattern.** Grep for the same mistake elsewhere. Fix instances or encode a structural guard (lint, type, test).
9. **Capture if it will repeat.** Call the Skill tool with `"living-docs"` when the miss is class-level and likely to recur.

**Done when:** root cause is named, fix addresses that layer, verify-work evidence is fresh (or gaps named), and same-pattern sites are checked.

## When stuck

See `references/when-stuck.md`. Default move: instrument real values; do not stack speculative edits.

## Anti-patterns

See `references/anti-patterns.md`.

## Scope

- In scope: technical failures in code, tools, env, data, or tests.
- Out of scope: product taste debates, rewriting unrelated modules “while here,” or claiming done without reproduction or proof.
