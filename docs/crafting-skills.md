# Crafting skills

Learnings distilled from [mattpocock/skills](https://github.com/mattpocock/skills) (philosophy + `writing-for-agents` / invocation rules), applied to this repo.

## Bar

- Skills fix **real agent failure modes**, not hypothetical process theater.
- Prefer **small, composable, editable** skills over owning the whole workflow.
- Work with **any model / harness**. Harness-specific naming or branches belong in a project fork, not here.
- Do not add a skill that is only a composition of existing skills unless the glue itself is the failure mode.

## Description = context pointer

The YAML `description` decides when the skill is reached.

- **Model-invoked** (default): rich triggers — “Use when…”, synonyms that are **distinct branches**, not the same branch twice.
- **User-invoked** (`disable-model-invocation: true`): human-facing one-liner; no “use when the user says…” trigger lists. Only the human reaches it.

Front-load the leading word. Cut identity the body already carries.

## Body = steps + reference

1. **In-file steps** first — ordered actions with clear done criteria.
2. **In-file reference** — rules consulted on demand (short).
3. **Disclosed reference** — long catalogs in `references/`, loaded only when a step needs them.

Push what only some branches need behind a pointer. Keep co-located: definition + rules + caveats under one heading.

## Positive instructions

Prefer “do X” over “don’t do Y”. Negation activates the banned behavior. Use bans only as hard guardrails, paired with the positive target.

## Cross-skill calls

Operative steps name the **Skill tool** and the skill: `Call the Skill tool with "living-docs"`. One skill per call. Do not rely on `/skill` slash syntax (harness-specific).

## Invocation split in this repo

| Skill | Invocation | Why |
| --- | --- | --- |
| `verify-work` | Model | Agents should reach for proof before claiming done |
| `living-docs` | Model | Agents should capture repeating lessons without being asked |
| `debug-root-cause` | Model | Agents default to symptom patches; force root-cause process |
| `critique-me` | User | Explicit pre-merge review + auto-fix; deep, expensive |
| `adversarial-review` | User | Opposite-model challenge; review-only (must not merge into critique-me) |
| `unslop` | User | Explicit prose edit pass |

## README

List every skill with a one-line description. Group **User-invoked** vs **Model-invoked**. Link the skill name to its `SKILL.md`.
