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
