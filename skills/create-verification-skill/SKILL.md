---
name: create-verification-skill
description: "Generate a project-local verify-<app> / verify-<surface> skill that launches, drives, and proves the real app, plus a feature map. Use when something needs runtime verification and no project verify-* skill covers that app or surface, or when the user asks for a verification harness."
metadata:
  author: mattpua
  version: "1.2.0"
  credit: Adapted from cursor/plugins pstack create-verification-skill
---

# create-verification-skill

Generate a **project-local** verification skill that drives the real app the way a user does. Output is for the next agent mid-task — cold-readable, no placeholders.

Pairs with **verify-work** (evidence policy / when done) and **maintain-verification-skill** (keep the map honest). This skill builds the harness + feature map; verify-work decides which proofs a change needs.

## 1. Interview the repo, not the user

Answer from the codebase; ask only what you cannot observe:

| Question | Look for |
| --- | --- |
| **Surface** | Primary user touch: web UI, CLI/TUI, desktop, API, mobile, library. Note secondary surfaces. |
| **Run** | Documented local start (package scripts, Makefile, README). Ports, env, seed, auth. |
| **Drive** | Existing harness first (Playwright, expect, PTY, curl, debug port). Else: browser/CDP for web/Electron, tmux/PTY for CLI/TUI, HTTP for services. |
| **Observe** | Screenshots, transcripts, response bodies, logs, exit codes, DB state. |
| **Isolate** | Can two instances run side by side? If not, generated skill must refuse double-driving a shared session. |

Broken checkout → fix or report precisely before generating. Verification scaffolding (sample config, empty static dir) allowed only when clearly marked and removed in cleanup.

## 2. Generate the skill

Write under the project skills root (prefer `.agents/skills/verify-<app>/`, else `.cursor/skills/verify-<app>/`):

`SKILL.md` with frontmatter `name: verify-<app>` and a description that names the app, surface, and when to use it.

Required sections (grounded in the interview — no leftover `{{…}}`):

1. **Launch** — exact start command; how to know ready (usable state, not merely process up); migrations/seed from empty data dir; teardown. Short-lived CLI: build once, each drive in its own isolated PTY/tmux.
2. **Doctor** — read-only “worth driving?” (process, version/build, port ownership, auth).
3. **Drive** — harness recipe with real selectors/commands from this repo. Prefer ARIA / data attrs / prompt strings / routes over coordinates.
4. **Evidence** — what to capture and where. Standards: real user path (not test-only setters); action + resulting state; side effects (files, rows, messages); mocks only at real prod boundaries; dry-run/test modes verified by observing what they skip.
5. **Cleanup** — tear down what **you** started (never kill by process name). Evidence survives teardown at a named path.
6. **Helpers** — any shipped script is executable; invocation shown in the skill body.

Copy shape from `templates/verify-app-skill.md.example` if helpful.

## 3. Seed the feature map

Create `features/README.md` plus one file per top user-facing feature (start with 3–5 from routes/commands/menus/docs). Follow `references/feature-map-example/`.

Each feature file: H1 + one paragraph, then exactly these H2s in order:

1. `Sub-features`
2. `How to get to it (user POV)`
3. `Driving it with <harness>`
4. `Gotchas`

User POV only. Missing seed preconditions → journey fails and the reason goes in Gotchas — do not weaken the journey to pass.

## 4. Prove before handover

Run the generated skill once end-to-end: launch → doctor → drive **one** mapped feature → evidence → cleanup. Confirm evidence still exists after cleanup. Fix failures; clean up after failed iterations too. Never-executed output is a draft, not a deliverable.

Stand-in targets (container vs real VM, etc.): list every setup step the stand-in did that the real target will not — those stay unverified until covered.

## 5. Wire verify-work

In the project's verify-work addons (Tools → Verify skills), name this `verify-<app>` skill and which base rows it drives (UI, forms, …).

## 6. Offer maintenance

Point at **maintain-verification-skill** for map drift. Suggest cadence only if asked.

**Done when:** skill + feature map exist, one feature proven live, evidence retained after cleanup, verify-work table/rule references the harness when applicable.
