# Example verification map

Illustrative Notes app. Copy the **shape**, not the product.

## Baseline preconditions

- Launch at a disposable URL/data dir.
- Doctor must pass before any drive.
- Never drive an instance this run did not start (unless the skill documents a shared-instance rule).

## Driving conventions

- Prefer ARIA roles / accessible names over CSS or coordinates.
- Treat commands as literal.
- Restore seeded data after mutations; keep proof artifacts.

## Proof and skip reporting

- Capture action + resulting state.
- UI: snapshot and/or screenshot with app identity visible.
- CLI: command, stdout, stderr, exit code.
- Unreachable path: attempted route + unmet precondition — not “verified via another path.”

## Feature entry contract

Each feature file: H1 + one paragraph, then exactly:

1. `Sub-features`
2. `How to get to it (user POV)`
3. `Driving it with <harness>`
4. `Gotchas`

## Features

- [Create a note](./create-note.md)
- [Search notes](./search.md)
