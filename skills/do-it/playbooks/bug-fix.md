# Bug fix

Reproduce → root cause → fix → prove. No speculative “might help” patches.

1. Reproduce on the real surface yourself. Capture full error/output. If it will not fire, instrument or synthesize the trigger until it does. Ask the user only with a specific reason you cannot reach the surface.
2. Call the Skill tool with `"debug-root-cause"`. Follow it through isolate → why? → fix at the cause.
3. Call the Skill tool with `"verify-work"`. Original repro must pass on the same surface. Typecheck alone is not enough when a runtime path exists.
4. Grep for the same pattern elsewhere; fix or encode a guard.
5. If the miss will probably repeat, Call the Skill tool with `"living-docs"`.

**Reply:** broken behavior, root cause, fix, proof (failing → passing evidence).
