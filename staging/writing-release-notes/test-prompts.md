# Test prompts for writing-release-notes

Format: `should-fire | prompt`

## Positive (should fire)

```
y | Draft release notes for v3.2 from these merged PRs
y | Write a changelog for this release
y | Summarize the merged pull requests into release notes
y | Turn this list of merged PRs into release notes grouped by type
y | Generate the release notes for the 2.0 milestone
```

## Negative (should NOT fire)

```
n | Write me a git commit message for this staged diff
n | Review this pull request for correctness bugs
n | Explain what this function does
n | Summarize this news article into three bullets
n | Write a docstring for this Python class
```
