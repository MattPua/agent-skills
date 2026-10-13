# When stuck

1. **Instrument, don’t guess.** Log or inspect actual values at the boundary you distrust. Read real state (DB row, response body, env value) — not what the code “should” see.
2. **Diff what changed.** `git log` / `git bisect` / compare last known good. Most regressions are recent.
3. **Minimal repro.** Strip to the smallest input/path that still fails. Drop unrelated features from the scene.
4. **One change at a time.** Parallel speculative edits hide causality.
5. **Challenge the “sure” part.** Re-verify the assumption you have not questioned yet.
6. **Upstream first.** For third-party oddness: package docs, then GitHub issues/PRs for the same symptom, before inventing a local workaround.
7. **Write the hypothesis.** One sentence: “X fails because Y.” If you cannot write it, gather more evidence before editing.
