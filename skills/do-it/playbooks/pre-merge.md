# Pre-merge

Review the named diff or PR. Fix what is clear and local. Do not replace verify-work on the implementing turn.

1. Confirm scope (diff, PR, or files). Non-empty.
2. Call the Skill tool with `"critique-me"`. Follow its triage + auto-fix + report.
3. Change alters something other code reads by value (shared type, URL/slug, schema/migration, public API, flag) and critique-me didn't already run it: Call the Skill tool with `"blast-radius"`.
4. If critique-me hands off agent-config follow-up, Call the Skill tool with `"living-docs"` on the invoking turn.

**Reply:** critique-me report (plus blast-radius if run). What was fixed vs deferred.
