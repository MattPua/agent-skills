# Agent Skills

Small, composable skills for real engineering with coding agents — not vibe-coding process packs.

Works with any agent that reads [Agent Skills](https://agentskills.io/) / [skills.sh](https://www.skills.sh/docs) `SKILL.md` files.

[![skills.sh](https://skills.sh/b/MattPua/agent-skills)](https://skills.sh/MattPua/agent-skills)

Craft bar: [`docs/crafting-skills.md`](docs/crafting-skills.md) (inspired by [mattpocock/skills](https://github.com/mattpocock/skills)).

## Install

```bash
npx skills add MattPua/agent-skills
```

Single skill:

```bash
npx skills add MattPua/agent-skills -s verify-work
npx skills add MattPua/agent-skills -s living-docs
npx skills add MattPua/agent-skills -s debug-root-cause
npx skills add MattPua/agent-skills -s do-it
npx skills add MattPua/agent-skills -s create-verification-skill
npx skills add MattPua/agent-skills -s maintain-verification-skill
npx skills add MattPua/agent-skills -s critique-me
npx skills add MattPua/agent-skills -s blast-radius
npx skills add MattPua/agent-skills -s adversarial-review
npx skills add MattPua/agent-skills -s unslop
```

```bash
npx skills update
```

## Model-invoked

The model may reach for these on its own, and you can also call them by name.

| Skill | What it does |
| --- | --- |
| [`verify-work`](skills/verify-work/SKILL.md) | Prove changes with concrete runtime evidence before claiming done |
| [`living-docs`](skills/living-docs/SKILL.md) | Capture repeating lessons as anti-patterns the same turn as the fix |
| [`debug-root-cause`](skills/debug-root-cause/SKILL.md) | Trace failures to root cause; fix there, not symptoms |
| [`critique-me`](skills/critique-me/SKILL.md) | Pre-merge review and fixer for a diff or PR |
| [`blast-radius`](skills/blast-radius/SKILL.md) | Find what a change breaks outside its diff; prove the one safety fact by running code |
| [`adversarial-review`](skills/adversarial-review/SKILL.md) | Opposite-model challenge; verdict only, no edits |
| [`create-verification-skill`](skills/create-verification-skill/SKILL.md) | Generate project-local `verify-<app>` harness + feature map |
| [`maintain-verification-skill`](skills/maintain-verification-skill/SKILL.md) | Keep that harness/map honest (source + live pass) |
| [`unslop`](skills/unslop/SKILL.md) | Remove AI writing patterns and restore human voice in prose |

## User-invoked

Reach this by name (slash / picker). The model will not auto-fire it.

| Skill | What it does |
| --- | --- |
| [`do-it`](skills/do-it/SKILL.md) | Single entry — pick a playbook; leaf skills run as steps |

Default for nontrivial work: **`/do-it`**. It routes bug-fix / feature / investigation / pre-merge / docs / verification-harness / figure-it-out and calls leaf skills for you.

## Project adoption

1. Install the skills.
2. Copy each skill’s `templates/` into your standing agent docs; fill `{{…}}` placeholders.
3. Product detail stays in the project. Skill bodies stay generic.

## Layout

```
skills/
  do-it/                        # user — entry / playbooks
  verify-work/                  # model
  living-docs/                  # model
  debug-root-cause/             # model
  create-verification-skill/    # user — generate verify-<app>
  maintain-verification-skill/  # user — upkeep verify-<app>
  critique-me/                  # user
  adversarial-review/           # user
  unslop/                       # user
docs/crafting-skills.md
skills.sh.json
```
