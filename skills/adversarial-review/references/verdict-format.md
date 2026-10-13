# Verdict format

```
## Intent
<what the author is trying to achieve>

## Verdict: PASS | CONTESTED | REJECT
<one-line summary>

## Reviewer plumbing
- lead_model: …
- reviewer_cli: codex | claude | other
- outputs: list files; note any missing/empty

## Findings
Numbered, high → medium → low. Each item:
- **[severity]** Description with file:line
- Lens: …
- Frame: which project principle / rule (if any)
- Recommendation: concrete action

## What went well
1–3 things reviewers did not contest

## Lead judgment
For each finding: accept or reject + one-line rationale
```

## Verdict logic

| Verdict | When |
| --- | --- |
| **PASS** | No high-severity findings |
| **CONTESTED** | High-severity findings, reviewers disagree |
| **REJECT** | High-severity findings with reviewer consensus |
