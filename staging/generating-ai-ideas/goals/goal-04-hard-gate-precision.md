# Goal — generate-ai-ideas (Priority 4)

**Created:** 2026-05-31
**Priority:** 4 — Hard Gate Precision
**Target:** `staging/generating-ai-ideas/SKILL.md`

## End state

On a curated set of 10 borderline ideas (5 that should pass and 5 that should be rejected or heavily caveated), the skill’s application of the Hard Gate agrees with expert judgment in at least 9 out of 10 cases.

> **Status (2026-05-30): MET at baseline — no change made.** On a fresh curated set of 10 borderline ideas (eval-seeds.md), the current skill scored **9/10**. All 5 REJECTs were correct and unanimous (clean firing on the maintenance-burden, service/high-touch, vanity-metrics, and medical-claim/regulatory red flags — i.e. no dangerous false-passes); 4/5 ADVANCEs correct. The single disagreement was the most ambiguous idea in the set (piano practice-triage), where the skill's REJECT was itself well-reasoned (thin moat + no *evidenced* WTP) and was partly an artifact of the no-web-research eval condition — a real run's Researcher would surface the known $10–30/mo teacher WTP and likely flip it. No SKILL.md edit was made: the target is met, and loosening the gate to win one contestable label would risk inducing false-passes on the reject side (net-negative). Run `runs/2026-05-30-g4-1333/` (baseline-scoring.md).

## How Claude proves it

- Create and maintain a fixed test set of 10 borderline AI product ideas.
- Run the candidate through the council + Hard Gate process on all 10.
- Score agreement with expert labels (PASS / borderline but viable / should be rejected).
- A kept iteration achieves ≥ 90% agreement (9/10 correct).
- All positive prompts in `test-prompts.md` continue to trigger the skill.
- The candidate still passes `eval/check-skill.sh` with zero FAILs.

## Must not change

- The 5 core Hard Gate criteria and the Quick Reject Red Flags (including the new harmful/illegal refusal flag) must remain.
- The requirement to apply the Hard Gate to every promising idea must stay.

## Iteration cap

20 iterations, or 5 consecutive iterations with no improvement in agreement rate.

## Synthesized /goal condition

```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows 90% or higher agreement with expert judgment on a fixed set of 10 borderline ideas when applying the Hard Gate. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. The core Hard Gate criteria and refusal policy are unchanged. Or stop after 20 iterations or 5 consecutive iterations with no improvement in agreement rate.
```