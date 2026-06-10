# Goal — generate-ai-ideas (Priority 2)

**Created:** 2026-05-31
**Priority:** 2 — Researcher Effectiveness in Low-Signal Verticals
**Target:** `staging/generating-ai-ideas/SKILL.md`

## End state

On professional services and creator verticals, the skill reduces the average number of tool calls by at least 20% while **maintaining** evidence quality (LLM- or human-judged) at a high bar (avg ≥ 9/10), compared to the before-state measured on the same fixed seeds.

> **Recalibration note (2026-05-30):** the original target ("+25% evidence quality") was unachievable. When the *current* skill's Researcher was measured on 6 low-signal seeds with real web research, baseline evidence quality already scored ~9.3/10 — at the practical ceiling — so a 25% gain (→ 11.6/10) is impossible, and the residual quality gap is access-bound (verbatim practitioner signal lives in Reddit/closed communities the tools can't reach), not prompt-fixable. The goal is therefore reframed to "cut tool calls ≥20% without degrading quality." **Achieved in run `runs/2026-05-30-g2-1242/`: avg tool calls 21.8 → 15.0 (−31%), quality held at 9.33 → 9.42/10.**

## How Claude proves it

- Run before/after versions on the same fixed set of 6–8 seeds from low-signal verticals (e.g. solo lawyers, independent teachers, executives, niche creators).
- Measure: (a) total tool calls, (b) evidence quality score (0–10) on specificity, verbatim quotes, and WTP signals.
- A kept iteration shows ≥20% reduction in tool calls **and** no degradation of average evidence quality (stays ≥ 9/10).
- All positive prompts in `test-prompts.md` continue to trigger the skill.
- The candidate still passes `eval/check-skill.sh` with zero FAILs.

## Must not change

- The Researcher role must remain evidence-first and friction-first.
- The mandatory use of the Hard Gate on all surfaced ideas must stay.
- Quality of Council Reports and Council Test Notes must not degrade.

## Iteration cap

20 iterations, or 5 consecutive iterations with no improvement on the combined efficiency + quality metric.

## Synthesized /goal condition

```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows at least 20% fewer tool calls while average evidence quality (LLM or human scored) stays at or above 9/10, on a fixed set of low-signal vertical seeds compared to the before-state measured on those same seeds. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. Researcher behavior remains evidence-first and the Hard Gate is still applied to every idea. Or stop after 20 iterations or 5 consecutive iterations with no improvement on the combined metric.
```