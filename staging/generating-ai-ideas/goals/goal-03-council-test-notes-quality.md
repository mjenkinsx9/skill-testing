# Goal — generate-ai-ideas (Priority 3)

**Created:** 2026-05-31
**Priority:** 3 — Council Test Notes Quality & Actionability
**Target:** `staging/generating-ai-ideas/SKILL.md`

## End state

The average usefulness score of generated Council Test Notes (scored on specificity, insight, and actionability for future improvements) is at 8.5/10 or higher across a batch of runs.

> **Status (2026-05-30): ALREADY MET at baseline — no change made.** Measured on 8 fixed seeds, the *current* skill's Council Test Notes score **~9.9/10 (Opus-driven)**; a weaker-model probe (Haiku) still scored **~8.7/10** — both above the 8.5 target. The original "~6.5 baseline" was stale (it predates the current template + the body's "not generic praise" rule + reference.md's note-writing guidance). The metric is at ceiling, so no evidence-supported SKILL.md edit exists (one could not be "kept" on a strict-improvement rule). Run `runs/2026-05-30-g3-1322/` (baseline-scoring.md). Note: the baseline notes independently converged on the same concrete reference.md/tests.md content edits (seat Legal for data-exposed agent seeds; a split-incentive Hard Gate check; new tests.md fixtures) — real optional future work, but it is content improvement, not note-*quality* work.

## How Claude proves it

- Generate Council Test Notes on a fixed set of 8–10 seeds using the candidate SKILL.md.
- Have an LLM judge (or human) score each note on a 1–10 scale for: (a) specificity, (b) genuine insight vs generic praise, (c) concrete recommendations for SKILL.md / reference.md changes.
- A kept iteration shows average score ≥ 8.5/10.
- All positive prompts in `test-prompts.md` continue to trigger the skill.
- The candidate still passes `eval/check-skill.sh` with zero FAILs.

## Must not change

- The mandatory requirement to always emit Council Test Notes after every Council Report must remain.
- The exact template structure must stay (Seed Topic, Council Mode, What worked, What didn’t, etc.).

## Iteration cap

15 iterations, or 5 consecutive iterations with no improvement in average Test Notes score.

## Synthesized /goal condition

```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows an average Council Test Notes usefulness score of 8.5/10 or higher (scored on specificity, insight, and actionability) across a fixed batch of seeds. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. The mandatory Council Test Notes requirement and template structure are unchanged. Or stop after 15 iterations or 5 consecutive iterations with no improvement in average score.
```