# verify-work anti-patterns

- Claiming done because a later review pass will catch it
- Claiming done after typecheck/lint/unit only when a runtime path exists and was not evidenced
- “Verified” UI without live UI or without visual proof in the reply
- “Verified” backend/HTTP without a live call
- “Verified” DB without exercising the running database
- “Verified” email because send returned OK — without opening the message
- “Verified” CI/deploy without a successful run
- Waiting for the user to paste an overlay, stack trace, screenshot, email, or failed deploy as QA
- Learning a new evidence class and leaving it only in chat
