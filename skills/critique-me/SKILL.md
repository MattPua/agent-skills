---
name: critique-me
description: Pre-merge review and fixer for a diff or PR.
disable-model-invocation: true
metadata:
  author: mattpua
  version: "1.6.0"
---

# critique-me

Pre-merge **review and fixer**. Not the default verify gate — implementing agents must already follow **verify-work**.

Auto-fix clear, local issues in changed application code. Prefer simplify over abstract.

Opposite-model challenge with **no edits** → invoke **adversarial-review** (separate skill). Do not fold cross-model CLI review into this pass.

## Process

1. **Load first** — this skill, then the project binding (search for `critique-me` binding / review notes). No binding → generics only; note that in the report.
2. **Scope** — git diff or files the user named. Ask if unclear. Confirm non-empty.
3. **Triage** — follow `references/triage.md`. Call the Skill tool with `"living-docs"` when agent-config / docs triage matches. Skip missing skills/agents; note skips.
4. **Read full files** for changed areas (diffs alone are not enough).
5. **Authz / standards** — run the project binding checklist in-process always.
6. **Auto-fix** — clear bugs, rule violations, obvious reuse/YAGNI/trims and speculative robustness (see Right-sized scope) in app code. Do not edit agent docs; hand those to the invoking agent via living-docs.
7. **Verify** — typecheck/tests on touched paths when feasible; revert failed fixes.
8. **Report** — Fixes applied; Critical / Warnings / Suggestions (Fixed / Deferred / Report-only); Skills applied; triage skip reasons; Agent config follow-up when required.

**Done when:** the report covers every triage that matched, auto-fixes that were safe are applied, and handoffs are explicit.

## Right-sized scope

Over-built is as wrong as under-built. For each piece of handling in the diff (retries, backoff, caches, dedupe, staleness caps, fallback tiers, edge-case branches, config knobs), ask: **will this realistically happen here, and what breaks if we don't handle it?** If the answer is “rarely, and nothing serious,” remove it (auto-fix when the removal is local). Don't suggest new hardening for theoretical cases either — only for failures that will actually occur at this project's scale.

## Only changed code

Do not flag or edit pre-existing code that wasn’t modified.

## Setup

Copy `templates/project-binding.md` into standing project docs. Optional thin launcher: require Read of this skill + binding before triage.
