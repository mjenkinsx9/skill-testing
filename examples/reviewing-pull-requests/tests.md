# Tests for reviewing-pull-requests

Verification scenarios for the reference example. Each **Verified output** below
was captured by running the skill against a real `git diff` fixture (a scratch
repo built for the run), not transcribed from the SKILL.md.

Last verified: 2026-05-31

## Golden path: clean diff with one nit

**Input:** A diff adding a tested `percent()` helper to `utils.py` (handles the
zero-denominator boundary) plus `test_utils.py` with two passing tests. An
unused `import os` slipped into `utils.py`.

**Expected behavior:** Empty "Blockers", empty "Important", one entry under
"Nits" pointing at the unused import with file:line. Summary: "ship".

**Verified output:**
```
## Blockers
(none)

## Important
(none)

## Nits
- utils.py:2 — `import os` is unused; drop it.

## Summary
Ship — new helper covers the zero-denominator boundary and has tests that fail
before the change; only an unused import to remove.
```

## Blocker: hardcoded secret

**Input:** A diff that adds `STRIPE_API_KEY = "sk-live-…"` to `config.py`.

**Expected behavior:** Blocker entry citing the line, recommending the secret be
moved to an environment variable or secret manager. Summary: "needs-rework".

**Verified output:**
```
## Blockers
- config.py:5 — Live Stripe secret committed (`STRIPE_API_KEY = "sk-live-…"`).
  Remove it, load it from an environment variable / secret manager, and rotate
  the key — once it lands in git history it must be treated as compromised.

## Important
(none)

## Nits
(none)

## Summary
Needs-rework — a live secret is committed; rotate and remove before merge.
```

## Important: new endpoint, no auth check

**Input:** A diff adding a Flask route `/admin/users` with no auth decorator,
while every other admin route in the file uses `@require_admin`. The handler
calls `load_users()`, which isn't imported in the hunk.

**Expected behavior:** Important-priority entry flagging the missing auth check,
citing the new route's location AND the surrounding precedent. Not a blocker
only because the omission might be intentional (must be confirmed). The skill
also flags what it can't verify from the diff alone rather than guessing.

**Verified output:**
```
## Blockers
(none)

## Important
- routes.py:20 — `/admin/users` has no `@require_admin`, unlike every sibling
  admin route (`routes.py:8`, `routes.py:14`). If unintentional this exposes an
  admin endpoint unauthenticated — confirm intent; if it should be protected,
  this becomes a blocker.

## Nits
- routes.py:22 — `load_users` isn't imported in this diff; confirm it's defined
  in module scope (can't tell from the hunk alone).

## Summary
Hold — confirm whether the missing auth decorator is intentional before merge.
```

## Edge case: empty diff

**Input:** `git diff` returns no changes.

**Expected behavior:** Skill declines to produce a review rather than
fabricating findings. Output is a one-line message indicating there's nothing to
review.

**Verified output:**
```
No changes detected; nothing to review.
```
