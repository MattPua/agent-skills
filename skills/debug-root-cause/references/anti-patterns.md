# Anti-patterns

| Move | Why it fails | Do instead |
| --- | --- | --- |
| Bypass flags (`--no-verify`, `SKIP_*`, force push) | Silences the signal | Fix the check or the cause it caught |
| Broad `try/catch` or empty catch | Hides the failure | Catch only what you handle; surface the rest |
| Null/undefined guard without asking why | Symptom patch | Fix the producer or make the type honest |
| Retry / sleep loops for flakes | Probability theater | Find the race or shared state |
| Shotgun edits (change three things) | Unknown causality | One change, re-run, observe |
| “It typechecks” as done | Misses runtime | Call verify-work on the failing path |
| Fix only the reported instance | Same bug nearby | Grep the pattern; structural guard if possible |
| Label “pre-existing” and skip | Leaves landmines | If you touch the file and see a clear bug in the path, fix or name it |
