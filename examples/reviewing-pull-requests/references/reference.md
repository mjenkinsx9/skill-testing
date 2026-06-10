# PR Review Rubric

The full criteria the skill applies. Loaded only when reviewing-pull-requests
runs.

## Correctness

- **Logic**: control flow handles the documented inputs; off-by-one and
  boundary conditions covered.
- **Error paths**: failures are surfaced (raised, returned, logged) rather
  than silently swallowed. Bare `except:` and ignored return values are
  blockers.
- **State**: shared/mutable state has clear ownership. No data races in
  concurrent code paths.
- **Idempotency**: operations that may be retried (network calls, queue
  consumers) handle being run twice.

## Tests

- New behavior has at least one test that would fail before the change.
- Bug fixes include a regression test reproducing the bug.
- Tests assert on observable behavior, not implementation details.
- No tests rely on wall-clock time, random seeds, or network unless those
  inputs are explicitly mocked or pinned.

## Security

- User input crosses a trust boundary → it must be validated or escaped
  before being interpolated into shell, SQL, HTML, or filesystem paths.
- Secrets (API keys, tokens, passwords) are never logged, committed, or
  emitted in error messages.
- Authentication and authorization checks happen at the right layer.
  A new endpoint without a permission check is a blocker.
- Dependencies introduced or upgraded: glance at the version, the package
  name (typosquats), and whether it was previously pinned.

## Performance

- Loops over user-controlled inputs have bounded size or pagination.
- New queries against large tables use an indexed column.
- Avoid quadratic scans of growing data structures unless the data is
  small and stays small.

## Style and maintainability

- New names follow the conventions already in the file.
- Comments explain why, not what. A comment that restates the code is a nit.
- Dead code, commented-out blocks, and TODOs without owners are nits unless
  they obscure intent (then important).

## Documentation

- Public APIs that changed shape have an updated docstring or README entry.
- Migration steps are documented if the change requires runtime action
  (env vars, DB migrations, manual data fixes).

## What NOT to flag

- Reformatting the reviewer would prefer but the linter accepts.
- Bikeshed-level naming when the existing name is clear enough.
- Suggestions to refactor unrelated code the PR didn't touch.
