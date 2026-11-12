# Calibration examples

Real cases a human caught after the agent's own review passed. Short form: what the agent wrote → what it should have been. Add new ones under their family when a user has to point one out.

## Repeated move

- Each API route under `/resources/:id/**` loaded the resource, resolved the caller's role, and checked a permission — correct in all five. → One middleware that takes the permission and puts the resource and actor on context.
- Four routes built "changed field names for the audit log" with `Object.entries(body).filter(v !== undefined).map(k)` plus a per-route omit list. → One `getDefinedKeys(obj, omit)` in the shared utils package.
- A filter pair (resource type + resource id) hand-written in ~12 places. → One shared builder.
- The rule for "who becomes owner" written in both the admin list and the approval code. → One function both call.
- An integration file with an emails test, a notifications test, and an audit test that each rebuilt the same scenario. → One test, three asserts.
- Database constraint / unique-violation parsing repeated at each call site. → One shared helper.
- Breadcrumbs or a two-column layout copied per entity type with small drifts. → One shared component, data passed in.
- A new "move item" helper and a restore path that also moves, added beside the existing move endpoint: three ways to do one job. → Both call the existing move.
- A cross-cutting step (timing, instrumentation, "do X after N seconds") written by hand at each call site, so every new caller must remember it. → One wrapper, or put it in the layer every call already goes through.

## Reinvented wheel

- A hand-written `parseCsv` while `papaparse` was already a dependency.
- A new `CountrySelect` beside an existing one in another feature.
- An embedded iframe map on one page while every other page used the app's map component (pin the center for the empty case).
- A custom session/token scheme designed from first principles instead of reading how an established auth library and the relevant standard do it.
- A URL schema written inline on one form while other forms needed the same rule. → One shared schema.
- A "leave with unsaved changes?" dialog built again when the app already had one.
- New types whose shape matched an existing type exactly.
- A hand-made `ids.join(',')` to get value equality in a memoized selector, when the library has an equality option (`resultEqualityCheck`). → Use the option.

## Unnamed concept

- Permission strings typed inline in each role's grant list, with no list of all permissions. → `PERMISSIONS = [...] as const` + `type Permission`; the role map is checked against it.
- `sessionId` and `tokenId` passed side by side through several functions. → One named type.
- `lat?: number; lng?: number` repeated on several shapes. → A `LatLng` primitive.
- Several audit-action constants for the same operation under two naming schemes ("two sets"). → One set.
- Routes, files, or keys named for one variant of a dimension that will vary (`/connect-google`, `/signup-google`, `googleStartHref`). → Make the dimension a path segment or parameter (`/oauth/google/connect`, `/oauth/<provider>/signup`) so the next variant slots in beside it.
- A general shape named after its first consumer (`adminDirectoryCoordinateSchema`, clinic-only opening-hours schema, a per-form address shape, a local `'women' | 'men'` union) while other features need the same thing. → Name it for what it is (`coordinateSchema`, `addressSchema`, `Gender`) in the shared package; search for an existing one first.
- One column name and enum on two tables after a bulk rename (a `memberType` on both the member and its organization), though the second table's values drive different rules. → A name per table (`organizationType`); after any bulk rename, re-read each hit and check the new name fits there.
- A padding value set separately per card type. → One shared inset token all cards read.
- The same optional field added to each sibling type's modal logic. → Put it on their common parent.

## Needless layer

- A one-type file plus a new package export for it. → Put the type with the code that uses it.
- A separate package for a data script. → A `scripts/` folder.
- Several new columns (and a migration) for loosely structured, optional data. → One JSON column for extras.
- A migration or backfill script for a one-time data move. → Run it once locally and copy the data.
- A server function or lookup that fetched a short, fixed list (footer links, countries) from the database. → A hard-coded list.
- A feature flag with one value everywhere and no plan to flip it. → Delete the flag and keep the live branch.
- package.json scripts for commands run once.
- Two tables (a record + its pending-verification twin) where one table with a status would do.
- Migrations split across several files for one change set.
- A new types file (or package export) for types nothing outside the implementation imports. → Keep them unexported beside the code.
- A feature flag on the safety net (undo) while the risky change it protects shipped to everyone. → Ship them together, or not at all.
- A security check behind an env var that defaults to off. → Turn it on; delete the switch.

## Theoretical robustness

- A full lookup integration built before sampling whether the source had useful data. → Sample first.
- A multi-step verification flow where a link with a code was enough.
- A cache in front of small or rarely-read data. → Read it each time; cache only after a measured cost.
- A step the user performs that the system could do itself (click "send me a code" on a screen whose only job is entering that code; re-enter the password alongside a one-time code; confirm again after clicking a confirm link). → Do it automatically; ask for each factor once.
- A debounce, a save queue, and a "newest choice wins" override so a user can change a setting while its save is in flight. → Disable the control until the save returns.
- A post-processing normaliser that changed the output in 1 of 48 trials. → Drop it.
- Undo history persisted server-side (whole snapshots) when undo within the session was all that was asked. → Keep it in memory.

## Type lie

- A field typed `startedAt?: string | Date | null` plus an `instanceof Date` branch, when its only producer is a JSON API that always sends `string | null`. → Type it `string | null` at the response schema, drop the branch.
- `serialized as never` to get past a mismatch. → Fix the declared type.
- `onValueChange={(v) => v != null && setX(v)}` when the component's type could carry `undefined` straight through.
- `typeof raw === 'number' ? … : typeof raw === 'string' && raw !== '' ? Number(raw) : …` on a value with a known type. → Parse once at the boundary with a schema.
- `(await getUser(id)) as { firstName?: string … }` → Use the function's real return type.
- A callback param typed `unknown` when the library already types it (`Error`), then re-narrowed with `typeof`/`instanceof`. → Use the library's type; runtime checks only for truly untyped values.

## Wrong shape

- An array of granted permissions checked only with `.includes`. → A set (or a lookup).
- `useEffect` that resets a form from props on every refetch. → `defaultValues` + a `key` that remounts on identity change.
- A whole table column for a status most rows share. → Show an icon only for the exception.
- `useEffect(() => setState(prop))` to mirror props or query data. → Derive in render, or remount with a `key`.
- A behavior every caller of a shared component needs (fallback link, label, loading state) made an opt-in prop; some callers never pass it. → Make it the default; a prop only for real exceptions.
- One `useState` per lazily imported module or loaded piece (each set once, in the same async block) plus an `!a || !b || …` readiness guard that grows with every addition. → One loader that returns an object, one state slot, `if (!lib)`; the state type comes from the loader instead of `any`.
- A lookup that resolves a path step by step (state → city → item) but returns `null` on any miss, so callers run it again, or fetch more data, to learn which step failed. → Return the match with how far it got (`{ match, stateName, cityName }`); callers never re-resolve.

## Dead branch / history

- "Group without members" branches in several services, after the model guaranteed every group has members. → Delete them.
- A dependency left in package.json after its tests were removed.
- An access-check helper with no remaining callers.
- Defensive handling for states the types and DB constraints now rule out. → Delete it.
- A derived field duplicated on each row (`row.owner.kind`) next to the same field at the top level.
- After a data-model change, seeds and fixtures patched to keep their old shape, deriving the new entities as side effects. → Rewrite them around the new model: parents first, referenced by key, new shapes covered.

## Noise

- Docstrings like "Admin only: lists a user's sessions" over a function named `listAdminUserSessions`.
- Comments on schema columns restating the column name.
- Auditing "admin viewed a list" — nobody will read it.
- Card descriptions that repeat the card title.
- A toast saying "change undone" after an undo whose result is already on screen.
- An analytics event for each micro-step of an interaction nobody asked to measure.

## Disproportion

- 2,800 lines and 165 integration tests for one feature; the overlap was found only by reading the whole file.
- 113 sign-ups and many password changes in one test run to prove a handful of rules.
- A full end-to-end run after every small fix.

## Divergent siblings

- Two directory pages with different filter bars and different empty states. → One filter component.
- One option list shows subtitles; its sibling groups instead.
- The same field (a location, a date) formatted differently in each table. → One formatter all tables use.
- Page and sort kept in the URL on most list pages but in local state on one. → Match the siblings.
- Sortable columns marked one way in one table and unmarked in another. → One header component.

## Wrong layer

- A database error message shown to the user. → Map internal failures to a generic message where errors cross into the response.
- Feature-flag shapes null-checked in every reader. → Normalize once where the session is built.
- Domain logic (token signing, email rendering, shared constants, generic helpers) written in the app when a package owns that domain. → Move it to that package and have the app pass only what it owns (secrets, ids, config).
- A ResizeObserver added to a hook shared by every tab and segmented control, to fix a case only one of them hits. → Keep it in that one component.
- Table formatting "fixed" several times in prompts and output, while the renderer joined each row with a blank line. → Fix the renderer.
- An "about 15 words" rule only in a field's description, while the schema allows 400 characters. → Enforce it in the schema.

## Unmeasured number

- A 1500px unmount margin chosen as "3× the mount margin", untested. → Measure a few values, or label it a guess.
- A 200-character cap on a tab name, set without looking at real names. → Check the real distribution first.

## Stray change

- A lockfile bump from a local install in a UI change. → Revert it.
- A fix for an unrelated injection bug folded into a review-fix commit. → Its own change.
