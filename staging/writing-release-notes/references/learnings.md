# Learnings for writing-release-notes

## Distilled lessons

<none yet — fill from the retro log as patterns repeat across runs>

## Retro log

### 2026-06-13 — drafted v3.2 notes from 18 merged PRs

- Worked: the features/fixes/chores grouping matched what the team expected.
- Inefficient / friction: the description's "or any time you need to write
  something describing recent code changes" catch-all made it fire on a
  "write me a git commit message" request that is not release-notes work.
  Candidate edit: drop that clause; keep only the release-notes / changelog /
  merged-PR triggers.
  New test? Add a "should NOT fire on commit-message requests" negative prompt.
  Status: open
- Inefficient / friction: the body says the same thing twice — Step 4 repeats
  the grouping already done in Step 2, and the "## Formatting the notes"
  section repeats "## Output format".
  Candidate edit: delete Step 4 and the "## Formatting the notes" section.
  Status: open
- Inefficient / friction: the opening "This skill helps you write release
  notes..." paragraph is filler that restates the title.
  Candidate edit: cut the intro paragraph.
  Status: open
- Bug: on a release with zero merged PRs the skill emitted an empty,
  malformed document instead of saying so. The Steps never cover the
  empty-input case.
  Candidate edit: add a step — if there are no merged PRs, emit a short
  "No changes to report." note rather than an empty document.
  Status: open
