---
name: code-smells
description: "Find code that passes review piece by piece but looks wrong as a whole: repeated moves, reinvented helpers, unnamed concepts, needless layers, type lies, noise, and size out of proportion. Use after finishing a feature or multi-file change, inside a pre-merge review, when the user asks what's sus or over-complicated, or when the user asks you to fix something that was working but badly shaped."
metadata:
  author: mattpua
  version: "1.0.7"
---

# code-smells

A smell is code that works and passes a per-line review, but that a careful human reads and fixes anyway. Agents produce them because they write and review one piece at a time; the smell lives *between* pieces. So this pass reads wide first, then asks questions — it is judgment, not a rule list.

## Steps

1. **Widen the view.** Read every changed file in full, not the hunks. For each new symbol (type, helper, component, file, export, table/column, script, dependency) open its nearest siblings and callers. List: new symbols, new files, new deps, new casts, new comments, lines added vs. size of the feature.
2. **Ask each family's question** (below) against that whole view. Read `references/examples.md` for calibration when a family is close.
3. **Prove each finding.** A finding names its evidence: the existing symbol it duplicates, the N other sites that make the same move, the caller that proves a branch dead, the library already in the lockfile. No evidence → drop it.
4. **Confirm with the user before fixing.** Every finding, even a clear one, goes to the user first — through the harness's question tool (`AskUserQuestion` / `AskQuestion`) when it has one, prose otherwise. One question per finding (up to 4 per call): name the family, the evidence, and the proposed fix; options like **Fix as proposed** (first), **Different fix**, **Leave it**. Don't edit code for a finding until the user picks a fix. Running as a subagent (no question tool) → return the findings unfixed, marked **Needs confirmation**, and the caller asks.
5. **Fix** what the user approved (smallest change, usually a deletion or a move to an existing home). Report the declined ones in a line each.
6. **Codify.** When the user had to point out a smell, add the before/after to `references/examples.md` under its family (or the project's copy). A new family only when no existing question would have caught it.

**Done when:** every family was asked, each finding has evidence, was confirmed with the user, and is fixed or declined, and user-caught smells are added as examples.

## Families

| Family | Tell | Question to ask |
| --- | --- | --- |
| **Repeated move** | Same few lines or same check in 3+ places; each correct alone | "Is this X, again, for Y?" → one helper, middleware, table, or shared component |
| **Reinvented wheel** | New helper, parser, component, type, or flow beside an existing one, or beside a dependency that does it | "What already does this — in this repo, its packages, its dependencies, or a well-known library?" Search before keeping |
| **Unnamed concept** | Literals or ad-hoc shapes that mean one domain thing, spread with no list or type; paired fields that always travel together | "What would a person call this?" → one list / type / primitive as the source of truth |
| **Needless layer** | New file, export, package, table, column, migration, script, config, or abstraction with one consumer or one use | "What breaks if this is folded into its caller or an existing home?" Nothing → fold it |
| **Theoretical robustness** | Handling, retries, options, or fetches for cases that won't happen at this scale; dynamic lookups for static data | "Will this realistically happen here, and what breaks if we don't handle it?" |
| **Type lie** | A type looser than the values that really flow through it: union members, `null`/`undefined`/optional the producer never emits (or both `null` and `undefined` for one absence), `unknown`/`any` where a real type exists. Its symptoms: casts (`as never`, `as unknown as`, `!`), `typeof`/`instanceof` checks and guards on already-typed values | "What does the producer actually send?" → make the type as explicit and slim as that, at the boundary; then delete the guards |
| **Wrong shape** | Data structure or API that fights how it's used: array for membership, list where a map is read, many columns where one structured field fits, side-effect sync for derived state | "How is this read?" → pick the shape for that read |
| **Dead branch / history** | Special cases, flags, fields, or fallbacks whose reason isn't in today's model; leftovers after a removal | "Can I justify this branch from current code alone?" |
| **Noise** | Comments restating code, labels that repeat the heading, logging/auditing of things nobody will read | "Does deleting it lose anything for a reader or operator?" |
| **Disproportion** | Line or test count far above the feature's weight; many tests with the same arrangement | "Would I be comfortable saying the size out loud?" → look for the families above inside it |
| **Divergent siblings** | Two things that do the same job look or work differently | "Why are these different?" No reason → make one match the other |
| **Wrong layer** | Logic in the place that sees it last: internal errors shown to users, validation in handlers, normalization repeated at every read instead of at the boundary | "Where does this belong so every caller gets it for free?" |

## Rules

- Prefer the fix that **deletes** (a file, a branch, a type, a copy) over the one that adds a layer.
- Extract only when the repeat is real (3+ sites, or 2 that must change together). Two similar lines are not a smell.
- Smells are generic. Project or product decisions (copy tone, what to audit, which users see what) belong in the project's own rules, not here.
