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
npx skills add MattPua/agent-skills -s critique-me
npx skills add MattPua/agent-skills -s unslop
```

```bash
npx skills update
```

## Model-invoked

The model may reach for these on its own.

| Skill | What it does |
| --- | --- |
| [`verify-work`](skills/verify-work/SKILL.md) | Prove changes with concrete runtime evidence before claiming done |
| [`living-docs`](skills/living-docs/SKILL.md) | Capture repeating lessons as anti-patterns the same turn as the fix |

## User-invoked

Reach these by name (slash / picker). The model will not auto-fire them.

| Skill | What it does |
| --- | --- |
| [`critique-me`](skills/critique-me/SKILL.md) | Pre-merge review and fixer for a diff or PR |
| [`unslop`](skills/unslop/SKILL.md) | Remove AI writing patterns and restore human voice in prose |

## Project adoption

1. Install the skills.
2. Copy each skill’s `templates/` into your standing agent docs; fill `{{…}}` placeholders.
3. Product detail stays in the project. Skill bodies stay generic.

## Layout

```
skills/
  verify-work/     # model
  living-docs/     # model
  critique-me/     # user
  unslop/          # user
docs/crafting-skills.md
skills.sh.json
```
