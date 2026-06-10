---
name: reviewing-pull-requests
description: Reviews a pull request diff against a structured rubric covering correctness, tests, security, and style, then produces a prioritized comment list. Use when the user asks to review a PR, audit a diff, or check changes before merging.
---

# Reviewing Pull Requests

## When to use

The user explicitly asks to review, audit, or critique a pull request, a diff,
or staged changes — including phrasing like "review this PR", "look over my
diff", "anything risky in these changes".

## Steps

1. Fetch the diff with whatever tool fits the context (`git diff`,
   `gh pr diff`, file contents the user pasted).
2. Apply the rubric in [reference.md](references/reference.md) section by section.
3. For each finding, classify priority: **blocker**, **important**, or
   **nit**. Include `file:line` references.
4. Emit the report in the output format below. Lead with blockers; nits go
   last and may be omitted if the diff is small.

## Output

```
## Blockers
- file:line — issue + suggested fix

## Important
- file:line — issue + suggested fix

## Nits (optional)
- file:line — minor issue

## Summary
<one sentence: ship / hold / needs-rework>
```

## Notes

- Don't restate what the diff already shows. A reviewer's job is to flag
  what's wrong or missing, not narrate the change.
- If you can't tell whether something is correct without more context (the
  caller, the schema, the test that exercises it), say so — don't guess.
