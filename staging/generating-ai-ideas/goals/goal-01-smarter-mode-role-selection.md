# Goal — generate-ai-ideas (Priority 1)

**Created:** 2026-05-31
**Priority:** 1 — Smarter Mode & Role Selection
**Target:** `staging/generating-ai-ideas/SKILL.md`

## End state

The skill correctly chooses between Fast Mode and Standard Strong Mode (and selects the appropriate specialist roles) on the first attempt in at least 85% of cases across a diverse set of test seeds.

## How Claude proves it

- Run the current version of the skill on a fixed test set of 12–15 seeds (balanced across verticals: creator, professional services, hardware hybrids, marketplace/platform, operational SaaS).
- Score each run for whether the initial mode + role selection was appropriate.
- A kept iteration in `runs/<run-id>/results.tsv` shows mode/role appropriateness ≥ 85%.
- All positive prompts in `test-prompts.md` continue to trigger the skill.
- The candidate still passes `eval/check-skill.sh` with zero FAILs.

## Must not change

- The two official modes (Fast Mode and Standard Strong Mode) and their documented compositions must remain clearly defined.
- The mandatory Council Test Notes logging requirement must stay in place.
- The Scope and Refusal Policy (harmful/illegal ideas) must not be weakened.
- All kept iterations must pass the harness with 0 FAILs.

## Iteration cap

20 iterations, or 5 consecutive iterations with no improvement in mode/role selection accuracy.

## Synthesized /goal condition

```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows mode/role selection appropriateness of 85% or higher on a fixed test set of 12-15 diverse seeds. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire under the candidate's description (trigger accuracy on positive prompts = 1.0). The two official modes and their compositions remain clearly documented. The mandatory Council Test Notes requirement and Scope and Refusal Policy are unchanged. Or stop after 20 iterations or 5 consecutive iterations with no improvement in selection accuracy.
```

## Baseline (as of 2026-05-31)

From the 9 live runs, mode and role selection was done manually in most cases and was estimated at roughly 60% "first-try correctness" without additional human guidance. The current decision guide in reference.md is helpful but not yet encoded strongly enough in SKILL.md itself for autonomous high-accuracy selection.