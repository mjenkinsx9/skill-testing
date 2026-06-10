# Test prompts for automating-browsers

Fixture for the improving-skills loop. Each row: a prompt and whether the
skill should fire.

The positives are real or close-paraphrase prompts from live sessions
exercising this skill. The negatives are browser-adjacent prompts where
a less precise trigger description would over-fire — knowledge fetches
(WebFetch is the right tool), code/git/local-file tasks, and write-code
prompts that mention "browser" or "Playwright" but want a script rather
than interactive driving.

Format: `should-fire | prompt`

## Positive (should fire)

```
y | Log into our Acumatica ticketing system and find tickets assigned to me
y | Browse to san1.storage.example.com and figure out where most of the space is being used
y | Find the changelog for Claude Code in the browser and tell me what's new
y | Open ticket 123456 and pull the activity log
y | Search the Anthropic docs for "progressive disclosure" and open the top result
y | Fill out my W-4 update on the HR portal
y | Test the signup flow on staging end-to-end and tell me if anything breaks
y | Scrape the pricing page on competitor.com and list every tier
y | Click through my Salesforce dashboard and grab today's pipeline number
y | Verify the new deploy is live by hitting the homepage and 3 key links
```

## Negative (should NOT fire)

```
n | What's the rate limit for the Anthropic Messages API?
n | Read the README in this repo and summarize it
n | Search the codebase for usages of parseAuth
n | Open a pull request from my current branch with these commits
n | Fix the failing test in tests/auth.test.ts
n | Generate a commit message for my staged changes
n | Refactor the login middleware to use async/await
n | Explain how OAuth PKCE works at a high level
n | Write a Playwright script that tests the checkout flow
n | Check what the latest @anthropic-ai/sdk version is on npm
```
