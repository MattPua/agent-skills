# Agent Skills

Reusable skills for AI coding agents. **Harness-agnostic** — works with any agent that reads [Agent Skills](https://agentskills.io/) / [skills.sh](https://www.skills.sh/docs) `SKILL.md` files (Cursor, Claude Code, Codex, Gemini CLI, Windsurf, OpenCode, …).

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

The CLI installs into whatever skill directories your tools use (commonly `.agents/skills/`). Update later:

```bash
npx skills update
```

## Skills

### `verify-work`

Prove changes with concrete evidence before claiming done. Static checks are prerequisites, not proof, when a runtime path exists.

### `living-docs`

Capture likely-to-repeat lessons as anti-patterns the same turn as the fix. Prefer merge over invent; codify mechanical bans in the linter when possible.

### `critique-me`

Pre-merge review **and** fixer. Auto-fix clear bugs/rules/reuse issues; hand living-docs follow-ups to the invoking agent. Not a substitute for verify-work.

## Project adoption

1. Install the skills (above).
2. Copy templates from each skill into **your** project’s agent-doc layout (agent guide, standing rules, anti-pattern topics, review launcher — whatever your harness loads).
3. Fill placeholders (`{{…}}`) with project tools and paths. Keep skills generic; put product detail in project copies.

Skills describe **behavior**. Templates are plain markdown — adapt frontmatter / paths for Cursor (`.mdc`), Claude, Codex, or others. Do not assume a single IDE.

## Layout

```
skills/
  verify-work/
  living-docs/
  critique-me/
skills.sh.json
```
