# Investigation

Read-only. Answer how / why / are we sure with citations. No product code edits unless the user pivots to a build playbook.

1. Restate the question and scope in one line.
2. Explore: Glob/Grep for owners, then Read the real entrypoints. Trace input → effect. Prefer primary sources over guesses.
3. For “why was this built this way,” check git history (`git log` / blame) and nearby docs/PRs when available.
4. Synthesize a senior-onboarding answer: mental model, flow, non-obvious pitfalls. Cite files (and commits when relevant).
5. If the answer surfaces a docs gap that will probably repeat, note it; Call the Skill tool with `"living-docs"` only if the user wants it captured now.

**Reply:** direct answer first, then short walkthrough with citations. Open questions last.
