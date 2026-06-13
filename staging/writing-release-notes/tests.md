# tests — writing-release-notes

Last verified: 2026-06-13

## Scenario 1: golden path

Input: a list of 6 merged PRs (3 features, 2 fixes, 1 chore). Expected: markdown
with Features/Fixes/Chores H2 sections, one bullet per PR, no empty sections.

## Scenario 2: edge case — only one category

Input: 4 merged PRs that are all bug fixes. Expected: a single Fixes section;
no empty Features or Chores headings are emitted.

## Scenario 3: error case — no merged PRs

Input: an empty PR list. Expected: a readable "no changes to report" note
rather than an empty or malformed document.
