# Goal — generate-ai-ideas (Priority 5)

**Created:** 2026-05-31
**Priority:** 5 — Token Efficiency (Standard Strong Mode)
**Target:** `staging/generating-ai-ideas/SKILL.md`

## End state

Average tokens used when running in Standard Strong Mode are reduced by 20–25% compared to the 2026-05-31 baseline, with no measurable drop in output quality (Council Report quality + Council Test Notes usefulness) on the same test seeds.

## How Claude proves it

- Run before/after versions on a fixed set of 8–10 seeds using Standard Strong Mode.
- Measure token count + quality scores (Council Report structure, evidence strength, and Test Notes usefulness).
- A kept iteration shows ≥20% token reduction while maintaining or improving quality scores.
- All positive prompts in `test-prompts.md` continue to trigger the skill.
- The candidate still passes `eval/check-skill.sh` with zero FAILs.

## Must not change

- The richness and structure of Council Reports and Council Test Notes must not be sacrificed for brevity.
- The two official modes and their documented behavior must remain clear.

## Iteration cap

20 iterations, or 5 consecutive iterations with no improvement on the combined efficiency + quality metric.

## Synthesized /goal condition

```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows at least 20% reduction in tokens for Standard Strong Mode runs with no drop in output quality scores on a fixed test set. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. Council Report and Council Test Notes quality are maintained or improved. Or stop after 20 iterations or 5 consecutive iterations with no improvement on the combined metric.
```