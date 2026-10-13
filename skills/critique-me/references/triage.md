# Review triage

Run before deep review. List applied skills (and skips) in the report.

## Skill triage

Read each matching **installed** skill (or cited standing rule). Apply checklists to **changed code only**. Always Call the Skill tool with `"living-docs"` when agent-config / docs triage matches (if living-docs is installed).

## Agent config / docs triage → living-docs

When the diff includes agent guide, standing rules, skills, anti-patterns, architecture map, or launcher config:

- Correct placement; no thin duplicates; prefer extend over new file.
- Flag mis-placement as Warnings.

## Agent config follow-up → living-docs

When the diff introduces a pattern that should be a standing rule but isn’t covered:

- Draft **Agent config follow-up** for the invoking agent (Gap + Call the Skill tool with `"living-docs"` + Action required).
- Do not edit agent guide / standing rules / anti-patterns yourself as the review specialist.

## Reuse

Deepen when new helpers/components/queries may parallel existing ones. Grep for equivalents; apply when clear.

## Threat model

When auth surfaces, public APIs, tokens/keys, uploads, SSRF-ish fetches, or expanded authorization change:

1. In-process authz from the project binding always.
2. Delegate to a security agent/skill only if installed.
3. Otherwise deepen in-process; put ship posture in the report.

## Product / performance / build simplicity

Add sections when the project binding’s signals match. Skip docs-only, copy/style-only, tests-only, or isolated one-line fixes.

## Auto-fix vs hand off

**Apply:** clear bugs, one-obvious-fix rule violations, named reuse targets, obvious trims, local perf/authz fixes.

**Hand off:** product decisions, agent-config docs, ambiguous refactors, threat posture without a mechanical fix, uncertain findings.
