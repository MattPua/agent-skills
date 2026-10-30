---
name: write-tests
description: "Write or review tests that catch real regressions, at the cheapest layer that proves the behavior. Use when adding, changing, or deleting tests, doing TDD, reviewing test coverage for a change, or when asked to test something; also when a suite feels padded or slow."
metadata:
  author: mattpua
  version: "1.0.0"
---

# write-tests

Every test earns its place by naming the breakage it catches. Count of tests, coverage %, and "a test per function" are not goals.

## Steps

1. **Load the project binding** (search standing docs for a `write-tests` binding). It names the test runners, layers, mocking helpers, and budgets. No binding → use the repo's existing tests as the pattern and say so.
2. **Read the code under test.** List its branches (if/else, early returns, switch arms), side effects (writes, emails, notifications, outbound calls), failure paths, and inputs (params, env, state).
3. **List candidate behaviors**, one line each: "when X → Y".
4. **Value gate — per behavior.** Write the realistic regression it would catch and what a user or the business loses if it breaks (wrong data shown, broken link, leaked record, bad write, lost money). No answer → drop it. Drop it too if another test already runs the same code path.
5. **Pick the cheapest layer that proves it** — unit for logic and mapping, component for UI states and form validation, integration for real persistence/transactions, end-to-end only for a critical path no lower layer can prove. Name the layer per behavior.
6. **Write the tests.** Name each "scenario → outcome" in business terms. Assert outcomes and side effects (called with the right args, or not called), not internals.
7. **Run them** — the new tests alone, then the suite they belong to. 100% pass, run twice for anything timing- or state-sensitive.
8. **Report** each test with the regression it guards, and each behavior you dropped with the reason.

**Done when:** every remaining test has a named regression, the suite passes, and dropped behaviors are listed.

## Worth a test

- A branch: different inputs → different outcomes.
- A side effect: the right call with the right data — or no call when there shouldn't be one.
- A multi-step write: order, and no partial state after a failure.
- Validation and mapping at the edges: empty, missing, boundary, invalid.
- A conflicting or already-done state ("already accepted", "already deleted").
- A bug you just fixed: the test that would have failed before the fix.

## Not worth a test

- One dependency call returned as-is (mock returns X, assert X).
- Constants or config in isolation — test the code that uses them.
- The simplest/default case alone when branch tests already pass through it.
- Cosmetic order or formatting nobody depends on.
- A branch another test already reaches through different input.

## Reliable tests

- Deterministic: fixed clocks and ids, no real network, no reliance on run order.
- Isolated: reset shared state and mocks per test; each test builds what it needs.
- Shared fixtures only when two or more test files use them.

## Deleting tests

Deleting is part of the job. When reviewing a suite, apply the value gate to existing tests too and remove the ones that fail it.

## Setup

Copy `templates/project-binding.md` into standing project docs and fill it in.
