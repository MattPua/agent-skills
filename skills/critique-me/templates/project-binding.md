# critique-me — project binding

Fill this in once per repo. The generic procedure lives in the installed `critique-me` skill.

## Paths

| Need | Where |
| --- | --- |
| Agent guide / invariants | `{{agents-guide}}` |
| Architecture map | `{{architecture-map}}` |
| Cursor rules root | `{{rules-root}}` |
| Acceptance / evidence table | `{{verify-work-table}}` |
| Reuse index | `{{reuse-index}}` |
| living-docs skill | `.agents/skills/living-docs` |

## Skill triage

| Skill / rule | Load when diff includes… |
| --- | --- |
| `living-docs` | Cursor docs triage or Cursor config follow-up triage matched |
| `{{skill-a}}` | `{{when-a}}` |
| `{{skill-b}}` | `{{when-b}}` |

## Security / authz checklist (in-process)

- `{{authz-rule-1}}`
- `{{authz-rule-2}}`
- `{{authz-rule-3}}`

## Threat model — delegate when

| Signal | Match when… |
| --- | --- |
| `{{threat-signal-1}}` | `{{threat-when-1}}` |
| `{{threat-signal-2}}` | `{{threat-when-2}}` |

**Delegate to:** `{{security-agent}}`  
**Skip when:** docs/copy/style/tests only; refactors with no new entry points.

## Product completeness — add section when

- `{{product-signal-1}}`
- `{{product-signal-2}}`

## Performance — add section when

- `{{perf-signal-1}}`
- `{{perf-signal-2}}`

## Reuse — deepen when

- New helper/component/query that may parallel `{{reuse-search-roots}}`

## Auto-fix roots

Application code under: `{{app-roots}}` (e.g. `apps/**`, `packages/**`, `convex/**`).

## Thin Cursor agent (optional)

```markdown
---
name: critique-me
model: inherit
description: Review and fix changed code before merge. Follows installed critique-me skill.
---

Follow the installed **critique-me** skill (`.agents/skills/critique-me`).
Project binding: `{{path-to-this-file}}`.
```
