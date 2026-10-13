---
name: critique-me
description: Review and fix changed code before merge. Use when asked to critique, review a PR/diff, deepen before merge, or run /critique-me — auto-fix clear bugs/rules/reuse issues; hand living-docs follow-ups to the invoking agent; not a substitute for verify-work.
metadata:
  author: mattpua
  version: "1.1.0"
---

# critique-me

Pre-merge **review and fixer**. Deepen before merge — **not** the default verify gate. Implementing agents must already follow **`verify-work`** before claiming done.

**Auto-fix is the default** for clear, local fixes in changed application code. Report what you fixed vs deferred.

**Less code is better.** Prefer simplify over abstract. Do not praise layers for their own sake.

## Load first (required)

Before triage or edits:

1. **Read this skill** (if invoked via a thin agent that only points here).
2. **Read the project binding** if it exists (path from the thin agent, or search `.cursor/rules/**/critique-me*` / `**/critique-me*binding*`). If none exists, use this skill’s generics only and note “no project binding” in the report.
3. Do **not** start reviewing from the thin agent blurb alone.

## Relationship to other skills / agents

| Role | Who |
| --- | --- |
| Same-turn proof | **`verify-work`** (implementing agent) |
| Living agent docs | **`living-docs`** — load for Cursor/doc diffs; hand config follow-ups to the **invoking** agent (do not edit agent docs yourself when running as a review subagent) |
| Security deep-dive | Project security agent/skill **if installed** — else in-process authz from the binding |
| Tests | Project test agent/skill **if installed** — else recommend tests in the report only |

**Missing delegates / skills:** If the binding names an agent or skill that is not present under `.cursor/agents/` or `.agents/skills/` / `.cursor/skills/`, **do not invent it**. Run the in-process checklist from the binding, note `delegate skipped — not installed` in **Skills applied**, and escalate only via report text.

## When invoked

1. **Load first** — skill + project binding (above).
2. **See what changed** — git diff or files the user indicates. Ask scope if unclear (uncommitted / branch / paths).
3. **Review triage** — run all triage sections below; load matching skills that exist; delegate only when the target exists.
4. **Read full context** — full files for changed areas (diffs alone are not enough).
5. **Apply project rules + agent guide** — flag violations; cite the rule.
6. **Check DRY / YAGNI / simplification** — reuse existing helpers; inline single-use noise.
7. **Security / authz / middleware** — binding checklist (in-process always).
8. **Apply fixes** — per Auto-fix.
9. **Verify** — typecheck/tests on touched paths when feasible; if a fix fails, revert and report.
10. **Report** — format below.

**Only review changed code** — do not flag or edit pre-existing code that wasn’t modified.

## Review triage (automatic)

Run before deep review. List applied skills in the report (include skips for missing installs).

### Skill triage

Read each matching skill under `.agents/skills/<name>/` or `.cursor/skills/<name>/` (or cited rule) **only if that path exists**. Apply checklists to **changed code only**.

Use the project’s skill-triage table. Always load **`living-docs`** when Cursor docs / config follow-up triage matches (if living-docs is installed).

### Cursor docs triage → load living-docs

When the diff includes agent config / docs (`.cursor/agents|skills|rules/**`, `AGENTS.md`, architecture map, etc.):

- Apply living-docs: correct placement; no thin duplicates; prefer extend over new file.
- Flag mis-placement as Warnings.

### Cursor config follow-up triage → load living-docs (systemic gaps)

When the diff introduces or repeats a pattern that **should** be a standing rule but isn’t clearly covered — even with no `.cursor/**` paths:

- Draft **Cursor config follow-up** for the invoking agent (Gap + `/living-docs` prompt + Action required).
- **Do not** edit `.cursor/**` or `AGENTS.md` yourself when acting as the review subagent.

### Reuse triage

Deepen when the diff adds helpers/components/queries that may parallel existing ones (unless quick review / skip reuse). Grep for equivalents; promote concrete reuse targets to Warnings and **apply** when clear. If a reuse-auditor agent exists, may delegate; otherwise stay in-process.

### Threat model triage

When the diff adds or changes auth surfaces, public API entry points, tokens/keys, uploads, SSRF-ish fetches, or expanded authorization (unless quick review / skip threat model):

1. Always run **in-process** authz checks from the project binding.
2. If a security agent/skill is installed → delegate for deep-dive.
3. If not installed → deepen in-process using the binding checklist; put ship posture in the report. Do not claim a missing agent ran.

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

**Tests** — Prefer integration/e2e/smoke for authz and critical paths; skip thin pass-through unit tests. If no test-writer agent: recommend only, don’t pretend tests were generated.

## Before you flag something

- Be certain — don’t invent hypotheticals.
- Don’t be a style zealot when the simplest option violates lightly.
- Don’t recommend abstraction for its own sake — propose consolidation with a **named** target.

## Output format

- **Fixes applied** — lead when you edited code
- **Critical** / **Warnings** / **Suggestions** — file:line; **Fixed** / **Deferred** / **Report-only**
- **Skills applied** — why each matched; note skips for missing installs
- Triage sections that matched (or one-line skip reason each)
- **Test recommendations** (optional)
- **Cursor config follow-up** — **required** when config follow-up triage matched
- Matter-of-fact; no flattery; don’t overstate severity

## Setup (once per project)

1. Install this skill (`npx skills add MattPua/agent-skills -s critique-me`).
2. Copy `templates/project-binding.md` into the project (e.g. `.cursor/rules/**/critique-me.mdc`).
3. Fill skill-triage rows, authz checks, product/perf signals. Leave delegate names blank or mark `in-process only` until those agents exist.
4. Keep a thin `.cursor/agents/critique-me.md` that **requires Read** of this skill + the binding before triage (see template).
