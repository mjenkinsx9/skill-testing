# Test prompts for reviewing-pull-requests

Fixture for `improving-skills` autoresearch runs. Each row: a prompt and
whether the `reviewing-pull-requests` skill should fire.

Format: `should-fire | prompt`

## Positive (should fire)

```
y | Can you review this PR before I merge it?
y | Look over my diff and tell me what's risky.
y | Audit the staged changes — anything broken?
y | I have a pull request open. Walk through it and flag issues.
y | Review the diff in #423 and prioritize blockers vs nits.
y | Check my git diff for security issues before I push.
y | Run a code review on the changes in this branch.
y | What should I fix in this PR before requesting review?
y | Eyeball this diff and tell me if the tests are adequate.
y | Critique this pull request like a senior engineer would.
```

## Negative (should NOT fire)

```
n | Write me a function that reverses a linked list.
n | What does this codebase do?
n | Help me debug a failing test — here's the stack trace.
n | Summarize the README.
n | Create a new git branch and commit my changes.
n | Why is my CI pipeline failing?
n | Explain how Python decorators work.
n | Generate a commit message for these changes.
n | Refactor this function to be cleaner.
n | Open a pull request for me from this branch.
```
