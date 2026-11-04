# Security review

Load when triage → Threat model matches. Scope is the changed code and the paths that reach it. The goal is a real boundary violation with a fix, not a checklist score.

Adapted from [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) (MIT, Copyright (c) 2025-2026 Cloudflare, Inc.), cut down to diff review.

## Candidate gate

A finding names all six. Missing one → it is not a finding yet.

1. **Principal** — the lower-trust actor and what they can already do (anonymous, member, other-tenant user, API key, webhook sender, retrieved document, tool output).
2. **Input** — the value, action, state change, or resource selector they control.
3. **Control** — the check that should reject, scope, limit, or revoke it.
4. **Boundary** — what that control protects (tenant, role, owner, secret, cost, shared availability).
5. **Affected** — the other principal or resource hit.
6. **Result** — the concrete wrong outcome: a record read or written, a wrong return value, a request sent, a quota spent.

Not findings: a missing best practice with nothing affected, self-impact, intended same-principal authority, a crash stated as code execution, guessed proxy/provider/deployment behavior.

## Method

Work one invariant at a time:

1. Pick an entry point the diff adds or changes. Name the principal and starting capability.
2. Follow the input through parsing, identity, authorization, normalization, storage, derived copies, and the final sink.
3. Find the strongest control on that path. Read the real code: decorators, serializers, query scoping, middleware.
4. **Compare sibling controls for equivalence, not presence.** The list endpoint scopes by owner; does the new export, bulk, retry, or async path apply the same scope? Read the legacy, batch, cancel, migration, and error paths that produce the same effect.
5. Compare what one component guarantees with what the next one assumes (truncation, type coercion, normalization, tenant scope).
6. Stop when the invariant is settled either way. Record the fix as the narrowest change at the last trusted decision point, plus the regression test that proves it.

**Sad paths** — try only those the interface accepts: absent, empty, zero, negative, max, over-limit, duplicate, mixed encoding, stale, revoked, reordered, concurrent, partially migrated, dependency failed, rolled back.

**Variants** — a confirmed high or critical root cause → search the diff and its siblings for the same pattern before reporting. Report each variant with its own conditions.

## Where bugs hide

### Business logic

- **State machines** — skip a step, go backwards, replay a finished flow. Step 2 of 3 fails: is step 1 undone?
- **Check-then-act** — non-atomic checks under concurrency (double approve, double spend, lost update, limit exceeded).
- **Numbers** — negative, zero, overflow, precision loss, string/number coercion.
- **Wrong check** — not "is there a permission check" but "is it the right one for this rule?" Can a different operation reach the same effect without it?
- **Trusted storage** — data assumed safe because it was validated on the way in. Could another path have written it?
- **Time** — expiry at the exact boundary, clock skew, timezone mismatch between components.
- **Defaults** — posture when config is missing, a flag is off, a dependency is down, or a migration is half done.

### Feature abuse

- **Export / backup** — includes data above the caller's access, other users' data, deleted or draft content.
- **Import / restore** — overwrites existing data, skips the normal validation or permission model.
- **Search / filter / sort** — reveals that hidden records exist, or leaks a hidden field through ordering.
- **Enumeration** — different status, message, timing, or size for "not found" vs "no access"; invite, reset, and signup flows.
- **Preview / share tokens** — scoped to one item or unlock more; drafts reachable through search, feeds, or listing endpoints; cache headers that let a CDN serve private content.
- **Webhooks / callback URLs** — server fetches a user URL; checked against internal targets, and still checked after a redirect?

### Chained and second-order

- **Stored, then reused** — a slug becomes a path, a field name a JSON path, a stored string a URL, regex, template, or raw HTML.
- **Capability growth** — a token, key, OAuth grant, plugin, or agent tool gains scope after refresh, role change, caching, or composition.
- **Revocation windows** — delete, revoke, or role removal not seen by a cache or a token already issued.
- **Restore / undelete / rollback** — re-applies current ownership and authz, or restores an old invalid state?

### Off the map

- What can the API do that the UI never calls?
- Half-finished, experimental, compatibility, or fallback code.
- A comment that claims the code is safe — verify the claim.
- Names like `temp`, `hack`, `legacy`, `unsafe`, `skip`, `bypass`.
- Edge cases the tests skip that the code still accepts.
- Irreversible or costly actions: is approval bound to the final principal, action, and resource?

### Obvious, still check

Secrets or keys in the diff; security TODOs (`TODO: add auth`); debug or admin routes reachable in prod; `eval` / `exec` / shell / dynamic import with input; wildcard CORS with credentials; open redirects (`next`, `redirect`, `return`, `url`); stack traces or SQL errors in responses. Trace each one to impact before reporting — a flag alone is not a finding.

### AI / agent surfaces

When the diff touches prompts, retrieval, memory, or tool calls:

- **Indirect injection** — retrieved or ingested content steers tool calls.
- **Context bleed** — another session's or tenant's data enters the prompt or memory.
- **Tool arguments** — model output reaches a sink (SQL, shell, URL, path) without the same validation user input gets.
- **Confused deputy** — the agent acts with more authority than the user who prompted it.

## Status and severity

| Status | Means | Severity |
| --- | --- | --- |
| **Confirmed** | Gate complete, path traced, result shown (test, local call, or unambiguous source) | Yes |
| **Needs validation** | Source-grounded, but one decisive fact is outside the repo (proxy, provider setting, deploy config) | No — name the missing fact and the check that settles it |
| **Rejected** | Source disproves it | — (drop, or list under triage skips if it was close) |

Severity, confirmed only:

- **Critical** — unauthenticated code execution, full data-store access, or takeover of arbitrary accounts.
- **High** — fully defeats an explicit control with real consequences: auth bypass, cross-tenant read/write, stored XSS against other users, authenticated code execution.
- **Medium** — a real boundary violation with limited blast radius or uncommon preconditions.
- **Low** — non-secret internals disclosed, or high effort for little gain.

High vs medium: does it **fully defeat** the control, or only weaken it? If you can't state the concrete damage, it is lower than it feels.

Map to the critique-me report: Critical/High → **Critical**, Medium → **Warning**, Low and needs-validation → **Suggestion (Report-only)**.

## Report each finding

- Principal → input → missing control → affected resource → result (one line).
- Trace: entry point to sink, `file:line` per hop.
- Evidence: test, command, or the source lines that settle it.
- Fix: the invariant, the narrowest change, the regression test.

Auto-fix only a local, one-obvious-fix authz or validation gap. Hand off the rest.
