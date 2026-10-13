---
name: critique-me
description: Review and fix changed code before merge. Use when asked to critique, review a PR/diff, deepen before merge, or run /critique-me — auto-fix clear bugs/rules/reuse issues; hand living-docs follow-ups to the invoking agent; not a substitute for verify-work.
metadata:
  author: mattpua
  version: "1.0.0"
---

# critique-me

Pre-merge **review and fixer**. Deepen before merge — **not** the default verify gate. Implementing agents must already follow **`verify-work`** before claiming done.

**Auto-fix is the default** for clear, local fixes in changed application code. Report what you fixed vs deferred.

**Less code is better.** Prefer simplify over abstract. Do not praise layers for their own sake.

## Relationship to other skills / agents

| Role | Who |
| --- | --- |
| Same-turn proof | **`verify-work`** (implementing agent) |
| Living agent docs | **`living-docs`** — load for Cursor/doc diffs; hand config follow-ups to the **invoking** agent (do not edit agent docs yourself when running as a review subagent) |
| Security deep-dive | Project **security** agent / skill when threat-model triage matches |
| Tests | Project **test** agent / skill when recommending non-trivial coverage |

## When invoked

1. **See what changed** — git diff or files the user indicates. Ask scope if unclear (uncommitted / branch / paths).
2. **Review triage** — run all triage sections below; load matching skills; delegate when required.
3. **Read full context** — full files for changed areas (diffs alone are not enough).
4. **Apply project rules + agent guide** — flag violations; cite the rule.
5. **Check DRY / YAGNI / simplification** — reuse existing helpers; inline single-use noise.
6. **Security / authz / middleware** — per project binding (see Setup).
7. **Apply fixes** — per Auto-fix.
8. **Verify** — typecheck/tests on touched paths when feasible; if a fix fails, revert and report.
9. **Report** — format below.

**Only review changed code** — do not flag or edit pre-existing code that wasn’t modified.

## Review triage (automatic)

Run before deep review. List applied skills in the report.

### Skill triage

Read each matching skill under `.agents/skills/<name>/` or `.cursor/skills/<name>/` (or cited rule). Apply checklists to **changed code only**. Load **all** matches.

Use the project’s skill-triage table (Setup). Always load **`living-docs`** when Cursor docs / config follow-up triage matches.

### Cursor docs triage → load living-docs

When the diff includes agent config / docs (`.cursor/agents|skills|rules/**`, `AGENTS.md`, architecture map, etc.):

- Apply living-docs: correct placement; no thin duplicates; prefer extend over new file.
- Flag mis-placement as Warnings.

### Cursor config follow-up triage → load living-docs (systemic gaps)

When the diff introduces or repeats a pattern that **should** be a standing rule but isn’t clearly covered — even with no `.cursor/**` paths:

- Draft **Cursor config follow-up** for the invoking agent (Gap + `/living-docs` prompt + Action required).
- **Do not** edit `.cursor/**` or `AGENTS.md` yourself when acting as the review subagent.

### Reuse triage

Deepen when the diff adds helpers/components/queries that may parallel existing ones (unless quick review / skip reuse). Grep for equivalents; promote concrete reuse targets to Warnings and **apply** when clear.

### Threat model triage

Delegate to the project security agent/skill when the diff adds or changes auth surfaces, public API entry points, tokens/keys, uploads, SSRF-ish fetches, or expanded authorization (unless quick review / skip threat model). Still do in-process checks from the project binding; escalate when Critical or multiple security Warnings.

### Product completeness / Performance / Build simplicity

Add report sections when the project binding’s signals match. Skip for docs-only, copy/style-only, tests-only, or isolated one-line fixes.

- **Product completeness** — missing realistic variants for changed user-facing behavior.
- **Performance** — unbounded queries, missing indexes/pagination, cold-path bundle weight, client waterfalls.
- **Build simplicity** — multi-file over-split, single-use helpers, narrative comments, parallel implementations. Promote Trim/Remove to Warnings and **apply** obvious trims.

## Auto-fix (default — application code)

**Apply** in changed app code before the final report — unless the user asked for **review only**, **no fixes**, or **quick review**.

| Apply when… | Examples |
| --- | --- |
| Bug with clear correct behavior | Wrong conditional, missing guard |
| Rule violation with one obvious fix | Missing authz helper, missing validators, wrong middleware |
| Reuse/YAGNI with named target | Use existing util; inline single-use constant |
| Build simplicity with obvious merge | Fold single-use component into parent |
| Perf with local fix | Index instead of filter; paginate |
| Authz gap with standard project pattern | Missing membership check; wrong deny shape |

| **Do not auto-fix** — report + hand off | |
| --- | --- |
| Product decision | Missing variant / “Decision needed” |
| Cursor config | → Cursor config follow-up + `/living-docs` |
| Ambiguous refactor | Multi-file consolidation that may change behavior |
| Threat-model posture | Conditional/blocked without clear mechanical fix |
| Uncertain finding | Investigate first; don’t guess |

## What to look for (generic)

**Bugs** — Logic errors, missing guards, races, edge cases in changed paths.

**Security / authz** — Follow the project binding (ACL helpers, deny shapes, no secrets in logs, no client private-file URL leaks, SSRF controls, validators on public functions).

**Structure / YAGNI** — Reuse existing modules; edit over new file; no “future-proof” machinery without need.

**Performance** — Prefer indexed/bounded data access; lean cold paths.

**Types** — Prefer validators + discriminated unions over defensive `typeof` sniffing / `as any`.

**Tests** — Prefer integration/e2e/smoke for authz and critical paths; skip thin pass-through unit tests.

## Before you flag something

- Be certain — don’t invent hypotheticals.
- Don’t be a style zealot when the simplest option violates lightly.
- Don’t recommend abstraction for its own sake — propose consolidation with a **named** target.

## Output format

- **Fixes applied** — lead when you edited code
- **Critical** / **Warnings** / **Suggestions** — file:line; **Fixed** / **Deferred** / **Report-only**
- **Skills applied** — why each matched
- Triage sections that matched (or one-line skip reason each)
- **Test recommendations** (optional)
- **Cursor config follow-up** — **required** when config follow-up triage matched
- Matter-of-fact; no flattery; don’t overstate severity

## Setup (once per project)

1. Install this skill (`npx skills add MattPua/agent-skills -s critique-me`).
2. Copy `templates/project-binding.md` into the project (e.g. beside a thin Cursor agent, or under `.cursor/rules/`).
3. Fill skill-triage rows, authz checks, product/perf signals, and delegate agent names.
4. Optional: keep a thin `.cursor/agents/critique-me.md` that sets model/permissions and says “follow the critique-me skill + project binding.”
