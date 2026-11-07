# Calibration examples

Real cases a human caught after the agent's own review passed. Short form: what the agent wrote → what it should have been. Add new ones under their family when a user has to point one out.

## Repeated move

- Each API route under `/resources/:id/**` loaded the resource, resolved the caller's role, and checked a permission — correct in all five. → One middleware that takes the permission and puts the resource and actor on context.
- Four routes built "changed field names for the audit log" with `Object.entries(body).filter(v !== undefined).map(k)` plus a per-route omit list. → One `getDefinedKeys(obj, omit)` in the shared utils package.
- A filter pair (resource type + resource id) hand-written in ~12 places. → One shared builder.
- The rule for "who becomes owner" written in both the admin list and the approval code. → One function both call.
- An integration file with an emails test, a notifications test, and an audit test that each rebuilt the same scenario. → One test, three asserts.
- Database constraint / unique-violation parsing repeated at each call site. → One shared helper.

## Reinvented wheel

- A hand-written `parseCsv` while `papaparse` was already a dependency.
- A new `CountrySelect` beside an existing one in another feature.
- An embedded iframe map on one page while every other page used the app's map component (pin the center for the empty case).
- A custom session/token scheme designed from first principles instead of reading how an established auth library and the relevant standard do it.
- A URL schema written inline on one form while other forms needed the same rule. → One shared schema.
- A "leave with unsaved changes?" dialog built again when the app already had one.
- New types whose shape matched an existing type exactly.

## Unnamed concept

- Permission strings typed inline in each role's grant list, with no list of all permissions. → `PERMISSIONS = [...] as const` + `type Permission`; the role map is checked against it.
- `sessionId` and `tokenId` passed side by side through several functions. → One named type.
- `lat?: number; lng?: number` repeated on several shapes. → A `LatLng` primitive.
- Several audit-action constants for the same operation under two naming schemes ("two sets"). → One set.

## Needless layer

- A one-type file plus a new package export for it. → Put the type with the code that uses it.
- A separate package for a data script. → A `scripts/` folder.
- Several new columns (and a migration) for loosely structured, optional data. → One JSON column for extras.
- A migration or backfill script for a one-time data move. → Run it once locally and copy the data.
- A server function that fetched a short, fixed list (footer links) from the database. → A hard-coded list.
- package.json scripts for commands run once.
- Two tables (a record + its pending-verification twin) where one table with a status would do.
- Migrations split across several files for one change set.
- A new types file (or package export) for types nothing outside the implementation imports. → Keep them unexported beside the code.

## Theoretical robustness

- A full lookup integration built before sampling whether the source had useful data. → Sample first.
- A multi-step verification flow where a link with a code was enough.
- Defensive handling for states the types and DB constraints already rule out.

## Type lie

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

## Dead branch / history

- "Group without members" branches in several services, after the model guaranteed every group has members. → Delete them.
- A dependency left in package.json after its tests were removed.
- An access-check helper with no remaining callers.
- A derived field duplicated on each row (`row.owner.kind`) next to the same field at the top level.
- After a data-model change, seeds and fixtures patched to keep their old shape, deriving the new entities as side effects. → Rewrite them around the new model: parents first, referenced by key, new shapes covered.

## Noise

- Docstrings like "Admin only: lists a user's sessions" over a function named `listAdminUserSessions`.
- Comments on schema columns restating the column name.
- Auditing "admin viewed a list" — nobody will read it.
- Card descriptions that repeat the card title.

## Disproportion

- 2,800 lines and 165 integration tests for one feature; the overlap was found only by reading the whole file.
- 113 sign-ups and many password changes in one test run to prove a handful of rules.
- A full end-to-end run after every small fix.

## Divergent siblings

- Two directory pages with different filter bars and different empty states. → One filter component.
- One option list shows subtitles; its sibling groups instead.

## Wrong layer

- A database error message shown to the user. → Map internal failures to a generic message where errors cross into the response.
- Feature-flag shapes null-checked in every reader. → Normalize once where the session is built.
