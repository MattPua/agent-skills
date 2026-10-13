# critique-me — project binding

Fill this in once per repo. The generic procedure lives in the installed `critique-me` skill.

**Agents must Read this file** (and the skill) before triage — do not rely on a thin launcher blurb alone.

## Paths

| Need | Where |
| --- | --- |
| Agent guide / invariants | `{{agents-guide}}` |
| Architecture map | `{{architecture-map}}` |
| Standing rules root | `{{rules-root}}` |
| Acceptance / evidence table | `{{verify-work-table}}` |
| Reuse index | `{{reuse-index}}` |
| living-docs skill | installed `living-docs` skill path |

## Skill triage

Only list skills that are **installed** in this repo (or mark `optional — skip if missing`).

| Skill / rule | Load when diff includes… |
| --- | --- |
| `living-docs` | Agent config / docs triage or config follow-up triage matched |
| `{{skill-a}}` | `{{when-a}}` |

## Security / authz checklist (in-process — always)

- `{{authz-rule-1}}`
- `{{authz-rule-2}}`
- `{{authz-rule-3}}`

## Threat model

| Signal | Match when… |
| --- | --- |
| `{{threat-signal-1}}` | `{{threat-when-1}}` |
| `{{threat-signal-2}}` | `{{threat-when-2}}` |

**Delegate to:** `{{security-agent}}` — or **`in-process only`** if that agent/skill is not installed.  
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

Application code under: `{{app-roots}}` (e.g. `apps/**`, `packages/**`, backend dir).

## Thin launcher (optional — adapt to your harness)

```markdown
# critique-me launcher

**Before triage:** Read the installed `critique-me` skill and `{{path-to-this-file}}`.
Do not review from this blurb alone.

Follow **critique-me**. Project binding: `{{path-to-this-file}}`.
Not the default verify gate — implementing agents must follow **verify-work**.
When agent-config follow-up triage matches: hand off `living-docs` to the invoking agent;
do not edit agent guide / standing rules / anti-patterns yourself.
```
