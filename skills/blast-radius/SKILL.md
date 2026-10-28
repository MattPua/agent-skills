---
name: blast-radius
description: "Find what a change could break outside its own diff before it ships, and prove the one fact it's safe because of by running real code. Use before merging a change that touches a shared type, a URL or slug scheme, a DB schema or migration, a public API or response shape, a config or feature flag, or anything other code reads by value rather than by symbol; also for \"what could this break\" or a small diff you don't trust."
metadata:
  author: mattpua
  version: "1.0.0"
  credit: Adapted from cursor/plugins pstack blast-radius (https://github.com/cursor/plugins/tree/main/pstack/skills/blast-radius)
---

# blast-radius

Find what a change breaks somewhere else, before it ships. Listing callers is not the job: grep does that in a second. The job is the breakage grep won't show you.

## When it's worth running

Run it when the change alters something other code depends on **by value**, not just by name:

- a shared type or response shape (fields added, removed, or made optional)
- how a URL, slug, key, or ID is built
- a DB column, table, migration, or backfill
- a config value, env var, or feature flag
- a public or internal API that another app or service calls

Skip it for changes whose effects stay inside the diff (copy, styling, a private helper with one caller).

## Don't trust your own writeup

A writeup that sounds right is worthless: it reads as convincing whether or not it's true. Find the one or two facts the change's safety depends on and get each as far down this list as is cheap. Say where it stopped.

1. You said so. Worthless on its own.
2. You pointed at the line: a real `file:line`, or the library's own source.
3. You showed the bad case can't happen: walked the failure step by step and it doesn't reach.
4. You ran it: a script or test that calls the real code and fails loudly if you're wrong.
5. You reproduced it in the running app.

Step 4 is usually one small script that imports what the app ships and calls the exact function you're worried about.

## Steps

1. **Read the change.** The diff, the symbols it adds, changes, and deletes, and what it now does differently, including what the diff doesn't spell out. Read the PR description and commits.
2. **Find the one fact it's safe because of.** Most risky-looking changes are safe because of a single fact ("every existing row gets a primary location in the backfill", "this only drops cache entries that are already dead"). Find it. If it holds, most risks clear at once. Spend your time here, not on a long list of maybes.
3. **Look where grep stops.** Read the source of the library you call, at its pinned version and any local patch. Work out when things run (teardown, retries, SSR vs client). Follow what a symbol search misses: JSON an API returns, a DB column another app reads, a URL already indexed by search engines or stored in emails, a sitemap, a wire format, a feature flag, code three hops downstream.
4. **Be honest about each risk.** Give it a real chance of happening and a real cost if it does. Keep the ones you confirmed; list the ones you checked and cleared separately. Cite a real `file:line`. A search that finds nothing is still an answer. Never invent a caller or an API.
5. **Prove the one fact.** Write a script or test that runs the real code, run it, and paste what happened.
6. **Big or wide change** → also call the Skill tool with `"adversarial-review"` and merge its findings. A second model catches different real bugs.

## What to hand back

- **What it does.** What changed, including the part that isn't obvious.
- **The one fact it's safe because of.** State it, say which step you got it to, and show the proof. If you couldn't prove it, write *unproven*.
- **Risks.** Each: how it breaks, `file:line`, how likely and how bad, how to check. Paste proof for the ones that matter.
- **Cleared.** What you checked and why it's fine.
- **Before you merge.** The cheapest test or repro that catches the real bug, including the script you wrote.

Cite real code. Strip anything private before it goes anywhere public.
