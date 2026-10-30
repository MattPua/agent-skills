---
name: critique-me
description: "Pre-merge review and fixer for a diff or PR: triage, apply clear fixes, report what's left. Use before merging or committing a non-trivial change, after finishing a feature or multi-file fix, or when the user asks for a review or critique."
metadata:
  author: mattpua
  version: "1.12.0"
---

# critique-me

Pre-merge **review and fixer**. Not the default verify gate — implementing agents must already follow **verify-work**.

Auto-fix clear, local issues in changed application code. Prefer simplify over abstract.

Opposite-model challenge with **no edits** → invoke **adversarial-review** (separate skill). Do not fold cross-model CLI review into this pass.

## Process

1. **Load first** — this skill, then the project binding (search for `critique-me` binding / review notes). No binding → generics only; note that in the report.
2. **Scope** — git diff or files the user named. Ask if unclear. Confirm non-empty.
3. **Triage** — follow `references/triage.md`; decide which **specialists** (below) match. Call the Skill tool with `"living-docs"` when agent-config / docs triage matches.
4. **Spawn specialists** — every matched specialist runs, in parallel, as a read-only subagent (see Specialists). While they run, **read full files** for the changed areas yourself (diffs alone are not enough).
4a. **Blast radius** is a specialist: it runs when the diff changes something other code reads by value (a shared type or response shape, how a URL/slug/key is built, a DB schema/migration/backfill, a public API, a config or flag) and proves its one safety fact with the `blast-radius` skill.
5. **Authz / standards** — run the project binding checklist in-process always.
6. **Auto-fix** — clear bugs, rule violations, obvious reuse/YAGNI/trims and speculative robustness (see Right-sized scope) in app code. Do not edit agent docs; hand those to the invoking agent via living-docs.
7. **Verify** — typecheck/tests on touched paths when feasible; revert failed fixes.
8. **Report** — Fixes applied; Critical / Warnings / Suggestions (Fixed / Deferred / Report-only); Skills applied; triage skip reasons; Agent config follow-up when required.

**Done when:** the report covers every triage that matched, auto-fixes that were safe are applied, and handoffs are explicit.

## Specialists

Matched specialists are **required**, not optional. A matched specialist that didn't run, or came back without evidence, is a **Warning** — never a footnote.

- **Read-only.** Specialists return findings; they never edit. critique-me is the only writer, applies fixes once, then verifies once. Several agents editing the same files lose fixes.
- **Evidence, not verdicts.** Each report lists what it searched or compared (commands, files, symbols) and what it found. "Looks fine" with no evidence → incomplete → Warning.
- **Parallel, scoped.** Give each one the diff, the changed-file list, and only its checklist.
- **No subagents available** (or already inside one) → run each matched checklist in-process, one after another, and say so in the report.
- **Skip all** for docs-only, copy/style-only, tests-only, or isolated one-line changes; say so in the report.

| Specialist | Runs when the diff… | Checklist |
| --- | --- | --- |
| Reuse | adds/moves any type (named or inline), component, helper, query, schema, constant, parser | triage → Reuse (name **and** structural search) |
| Blast radius | changes a shared type/response shape, URL/slug/key scheme, schema/migration/backfill, public API, flag | `blast-radius` skill |
| Security | touches auth, public endpoints, keys/tokens, uploads, outbound fetches, data exposure | triage → Threat model |
| Tests | changes branching logic, or adds/changes tests | triage → Tests |
| Sibling parity | adds a page/flow/component that has a same-role sibling, or behaviour that varies by role/mode/state | triage → Sibling parity |

The project binding maps each specialist to an installed agent or skill; unmapped ones use the checklist above in a generic subagent.

## Right-sized scope

Over-built is as wrong as under-built. For each piece of handling in the diff (retries, backoff, caches, dedupe, staleness caps, fallback tiers, edge-case branches, config knobs), ask: **will this realistically happen here, and what breaks if we don't handle it?** If the answer is “rarely, and nothing serious,” remove it (auto-fix when the removal is local). Don't suggest new hardening for theoretical cases either — only for failures that will actually occur at this project's scale.

**Subtract before you add** (from pstack `subtract-before-you-add` / `laziness-protocol`): prefer the change that removes something — a dead path, a helper column, a duplicate formatter, a second file — over one that adds a layer. One source of truth per decision. A smaller diff beats elegant boilerplate. Flag additions that a deletion would have made unnecessary.

## Only changed code

Do not flag or edit pre-existing code that wasn’t modified. Moved or extracted code counts as changed. Reuse checks still search the whole repo: new code that duplicates pre-existing code is a finding on the new code.

## Setup

Copy `templates/project-binding.md` into standing project docs. Optional thin launcher: require Read of this skill + binding before triage.
