# Verification harness

Project has no (or broken) scripted way to prove app behavior. Build or refresh the local `verify-<app>` skill.

1. If no `verify-*` skill with launch/drive/feature map: Call the Skill tool with `"create-verification-skill"`. Follow it through interview → generate → seed map → prove one feature.
2. If a harness exists but looks stale/wrong: Call the Skill tool with `"maintain-verification-skill"`.
3. Name the harness in the project's verify-work addons (Tools → Verify skills) when missing.
4. Call the Skill tool with `"verify-work"` only if this turn also claimed product behavior done.

**Reply:** path to `verify-<app>`, features seeded/covered, proof artifact location, outcome (created / maintained clean|changed|blocked).
