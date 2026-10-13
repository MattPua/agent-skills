# Reviewer prompt template

Each reviewer gets one prompt containing:

1. Stated intent (from the lead)
2. Assigned lens (full text from `reviewer-lenses.md`)
3. Relevant project principles (file contents or short excerpts — not vague summaries)
4. The code or diff to review (or precise paths + how to read them)
5. Instructions:

```
You are an adversarial reviewer. Find real problems; do not validate the work.
Be specific — cite files, lines, and concrete failure scenarios.
Rate each finding: high (blocks ship), medium (should fix), low (worth noting).
Write findings as a numbered markdown list.
Do not modify files unless explicitly told to run tests.
```

Spawn all reviewers in parallel.
