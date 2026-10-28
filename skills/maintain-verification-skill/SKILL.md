---
name: maintain-verification-skill
description: Keep a project verify-<app> skill and its feature map honest with a source pass and a live pass. Use when a verify-* skill's recipe no longer matches the app, its map is missing a feature you need to verify, or after features are added, renamed, or removed.
metadata:
  author: mattpua
  version: "1.1.0"
  credit: Adapted from cursor/plugins pstack maintain-verification-skill
---

# maintain-verification-skill

Upkeep for a skill from **create-verification-skill** (or any project-local verify skill with a feature map). Unit of rigor = **feature**, not every sentence.

## Outcomes

Say which:

| Outcome | Meaning |
| --- | --- |
| **clean** | Every feature got source + live coverage; nothing to ship |
| **changed** | One PR of proven doc/harness/map corrections |
| **blocked** | Coverage or safe ship failed — name the blocker |

## Edit scope

Only the verification skill directory (`SKILL.md`, `features/`, its helpers). Never product code. App behavior wrong vs map → product gap (report); map wrong vs app → doc drift (fix map).

## Pass

0. **Locate** — skill with launch/drive + feature map (usually `verify-*` under `.agents/skills/` or `.cursor/skills/`). Several → ask; none → stop and point at **create-verification-skill**.
1. **Index hygiene** — README vs sibling feature files; fix missing/extra/dead entries.
2. **Source wave** — one read-only explorer per feature (parallel). Each: how it works from source, likely drift with citations, one live recipe. No driving, no edits.
3. **Reconcile** — every feature has a summary; merge recipes; spot-check cited drift; note user-facing surfaces missing from the map (need a concrete source path).
4. **Live pass** — required even if source looks clean. Follow the verify skill’s launch model. Exercise every feature at least once. Hold:
   - Doctor before first drive; after surprising failure; on fresh sessions when sessions are the unit.
   - Evidence captured so far survives every cleanup.
   - Nothing a drive started outlives its usefulness (clean residue; don’t kill shared instances you didn’t start unless the skill says so).
5. **Triage** — wrong user-POV → fix docs. Harness can’t drive working behavior → fix harness (helpers executable + documented). Real product break → report, keep out of this PR.
6. **Ship or stop** — changed: one PR of proven corrections after re-read. clean/blocked: no PR; report coverage honestly.

Scratch run notes stay uncommitted.
