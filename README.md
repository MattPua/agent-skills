# Agent Skills

Reusable skills for AI coding agents. Install once; use across every project.

[![skills.sh](https://skills.sh/b/MattPua/agent-skills)](https://skills.sh/MattPua/agent-skills)

## Install

```bash
npx skills add MattPua/agent-skills
```

Or single skills:

```bash
npx skills add MattPua/agent-skills -s verify-work
npx skills add MattPua/agent-skills -s living-docs
npx skills add MattPua/agent-skills -s critique-me
```

Update later:

```bash
npx skills update
```

## Skills

### `verify-work`

Prove changes with concrete evidence before claiming done. Static checks are prerequisites, not proof, when a runtime path exists.

**Use when:** finishing a task, claiming a fix, shipping UI/API/DB/email/CI changes, or deciding what “verified” means in a repo.

### `living-docs`

Capture likely-to-repeat lessons as anti-patterns the same turn as the fix. Prefer merge over invent; codify mechanical bans in the linter when possible.

**Use when:** the user corrects you, the same class of mistake repeats, or verify-work finds a gap / new evidence class.

Formerly the project-local “manage-docs” workflow — same idea, portable name.

### `critique-me`

Pre-merge review **and** fixer. Auto-fix clear bugs/rules/reuse issues; hand living-docs config follow-ups to the invoking agent. Not a substitute for verify-work.

**Use when:** `/critique-me`, “review this PR”, deepen before merge.

## Project adoption

1. Install the skills (command above).
2. Copy templates from each skill into the project once — usually under `.cursor/rules/` or beside a thin Cursor agent.
3. Fill in project-specific paths, tools, triage rows, and evidence. Keep skills generic; put product detail in the project copies.

## Layout

```
skills/
  verify-work/
  living-docs/
  critique-me/
skills.sh.json
```

Skills follow the [Agent Skills](https://agentskills.io/) / [skills.sh](https://www.skills.sh/docs) format.
