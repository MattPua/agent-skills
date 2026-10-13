# Agent Skills

Reusable skills for AI coding agents. Install once; use across every project.

[![skills.sh](https://skills.sh/b/MattPua/agent-skills)](https://skills.sh/MattPua/agent-skills)

## Install

```bash
npx skills add MattPua/agent-skills
```

Or a single skill:

```bash
npx skills add MattPua/agent-skills -s verify-work
npx skills add MattPua/agent-skills -s living-docs
```

Update later:

```bash
npx skills update
```

## Skills

### `verify-work`

Prove changes with concrete evidence before claiming done. Static checks are prerequisites, not proof, when a runtime path exists.

**Use when:** finishing a task, claiming a fix, shipping UI/API/DB/email/CI changes, or deciding what “verified” means in a repo.

**First-time setup:** copy `skills/verify-work/templates/` into the project (see skill body).

### `living-docs`

Capture likely-to-repeat lessons as anti-patterns the same turn as the fix. Prefer merge over invent; codify mechanical bans in the linter when possible.

**Use when:** the user corrects you, the same class of mistake repeats, or verify-work finds a gap / new evidence class.

**First-time setup:** copy `skills/living-docs/templates/` into the project (see skill body).

## Project adoption

1. Install the skills (command above).
2. Copy templates into each repo once — usually under `.cursor/rules/` or `AGENTS.md`-adjacent docs.
3. Fill in project-specific paths, tools, and evidence rows. Keep skills generic; put product detail in the project copies.

## Layout

```
skills/
  verify-work/
    SKILL.md
    templates/
  living-docs/
    SKILL.md
    templates/
skills.sh.json
```

Skills follow the [Agent Skills](https://agentskills.io/) / [skills.sh](https://www.skills.sh/docs) format.
