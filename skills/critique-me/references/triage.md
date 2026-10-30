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

Deepen when the diff adds any helper, component, query, schema, constant, type, or parser — new or moved. Grep the whole repo and installed dependencies for equivalents (not just other files in the diff); apply when clear. Never describe new code as “removing a duplicate” without that search.

Name and behaviour searches miss shapes. Also search **by structure**:

- **Unnamed shapes count.** Inline object types (props, params, column/JSON types, type fields) are inventory, not just named symbols.
- **Same fields, new name.** Grep a new type's distinctive field combination across shared types, the schema, and every app. Equal → reuse; superset/subset → extend (`&`, `Pick`, `Omit`), don't redeclare.
- **Repeated inline shape** in two or more places → one named type in the shared types module.
- **Consumer-named shape.** A type named for one component whose fields describe a general entity (address, hours, contact) → name it for the entity; consumers derive from it.
- **Implementation-derived entity type** (`ReturnType<typeof fn>`, `typeof value`) standing in for a domain entity → give the entity its own type.
- **Hand-rolled utility types** → look for an exported equivalent first (e.g. a transaction type from the DB package).
- **Copy-only component.** Diff a new component against same-role siblings (same file-name pattern). Differs only in strings, ids, or row type → one parameterized component.

A reuse pass that reports no structural search is incomplete; list it under triage skips.

## Tests

For every test added or changed, and every changed branch without one:

- **Value gate.** Name the realistic regression the test catches and what a user or the business loses if it breaks. No answer → the test shouldn't exist.
- **Not worth a test:** the simplest/default case alone, cosmetic order or formatting, a branch another test already reaches, a mock-in/same-out pass-through, constants.
- **Missing:** changed branches, side effects, and failure paths with real consequences and no test → list them.

## Sibling parity

When the diff adds a page, flow, or component that has a same-role sibling elsewhere in the app:

- Open the sibling and the new one side by side (code, and the running UI when available). List every difference: sections, controls, copy patterns, formatters, empty states, child components (filters, maps, popups, items).
- Each difference is either forced by the data or a finding. Shared pieces should be shared modules, not copies.
- Behaviour that varies by role, mode, or lifecycle state: list each variant and whether the diff handles it.

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
