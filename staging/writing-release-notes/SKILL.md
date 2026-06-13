---
name: writing-release-notes
description: Generates release notes from merged pull requests, grouping the changes into features, fixes, and chores so the notes read clearly. Use when the user asks to draft release notes, write a changelog, summarize merged PRs into release notes, or any time you need to write something describing recent code changes.
---

<!-- DEMO FIXTURE for the learnings-seeding path (improving-skills tests.md).
     This SKILL.md carries intentional slop — an over-broad description
     catch-all, a duplicated step, a duplicated section, and a filler intro —
     all flagged as `Status: open` candidate edits in references/learnings.md.
     /improving-skills should seed those entries and apply them. Do not "fix"
     this by hand; it is the input state for a reproducible test. -->

# Writing Release Notes

This skill helps you write release notes. Release notes are a document that
describes what changed in a release. Writing good release notes is important
because users want to know what changed in the new version of the software.

## When to use

When the user asks for release notes or a changelog assembled from a set of
merged pull requests.

## Steps

1. Collect the merged pull requests for the release.
2. Group each pull request into one of three categories: features, fixes, chores.
3. Write a one-line summary for each pull request.
4. Group the summaries by category (features, fixes, chores).
5. Assemble the grouped summaries into the final document.

## Output format

Emit markdown with an H2 heading per category and a bullet per change:

```
## Features
- <change>
## Fixes
- <change>
## Chores
- <change>
```

## Formatting the notes

Format the output as markdown. Use an H2 heading for each category (Features,
Fixes, Chores) and a bullet list of the changes under each heading.

## Learnings

Retro observations from real runs live in
[references/learnings.md](references/learnings.md).
