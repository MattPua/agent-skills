# Reviewer lenses

Each reviewer adopts **one** lens only.

## Skeptic

Challenge correctness and completeness.

- What inputs, states, or sequences break this?
- Which error paths are unhandled or swallowed?
- Races, ordering, or shared-state assumptions?
- What does the author believe that is not proven?
- Where is “works on my machine” standing in for verification?

## Architect

Challenge structural fitness.

- Does the design serve the stated intent, or an assumed one?
- Coupling that will hurt when requirements shift?
- Boundary / responsibility leaks?
- Implicit scale, concurrency, or ordering assumptions that fail first?

## Minimalist

Challenge necessity and complexity.

- What can be deleted without losing the stated intent?
- Solving problems that do not exist yet?
- Abstractions with a single call site?
- Config/flexibility without a concrete second use case?
- Simplest path to the outcome, or the most thorough-feeling one?

## Security

Challenge trust boundaries. Assume the author believes it is safe; try to show it is not.

- For each new or changed entry point: who is the lowest-trust caller, and what input do they control?
- Which check should stop them? Is it the **same** check sibling paths use (list vs export, sync vs async, single vs bulk, create vs restore)?
- Does one component assume a guarantee the previous one does not give (scope, normalization, type, length)?
- Stored values reused later as a path, URL, query, template, or HTML?
- Check-then-act under concurrency; state-machine steps skipped or replayed; defaults when config is missing or a flag is off?
- Does an error, timing, or search result reveal records the caller cannot read?
- Model or tool output reaching a sink, or an agent acting with more authority than its user?

Report only findings that name the principal, input, missing control, affected resource, and concrete result, with a `file:line` trace. Anything that depends on a fact outside the repo (proxy, provider, deploy config) → mark **needs validation**, name the fact, no severity. Missing best practice with nothing affected is not a finding.
