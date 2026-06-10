# Quick Launch Commands — All 6 Improvement Goals

This file contains ready-to-copy commands for running each of the 6 prioritized improvement goals on `generating-ai-ideas`.

## Prerequisites (Already Done)

- `test-prompts.md` exists
- All 6 goal files exist in this folder
- Harness passes with 0 FAIL / 0 WARN

---

## How to Run Any Goal

For any priority below:

```bash
# Activate the goal
cp goals/<goal-file>.md goal.md

# Then paste the two lines below into Claude Code
```

---

### Priority 1: Smarter Mode & Role Selection (Recommended Starting Point)

```bash
cp goals/goal-01-smarter-mode-role-selection.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows mode/role selection appropriateness of 85% or higher on a fixed test set of 12-15 diverse seeds. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire under the candidate's description (trigger accuracy on positive prompts = 1.0). The two official modes and their compositions remain clearly documented. The mandatory Council Test Notes requirement and Scope and Refusal Policy are unchanged. Or stop after 20 iterations or 5 consecutive iterations with no improvement in selection accuracy.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

### Priority 2: Researcher Effectiveness in Low-Signal Verticals

```bash
cp goals/goal-02-researcher-effectiveness.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows at least 20% fewer tool calls and 25% higher evidence quality (LLM or human scored) on a fixed set of low-signal vertical seeds compared to the 2026-05-31 baseline. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. Researcher behavior remains evidence-first and the Hard Gate is still applied to every idea. Or stop after 20 iterations or 5 consecutive iterations with no improvement on the combined metric.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

### Priority 3: Council Test Notes Quality & Actionability

```bash
cp goals/goal-03-council-test-notes-quality.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows an average Council Test Notes usefulness score of 8.5/10 or higher (scored on specificity, insight, and actionability) across a fixed batch of seeds. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. The mandatory Council Test Notes requirement and template structure are unchanged. Or stop after 15 iterations or 5 consecutive iterations with no improvement in average score.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

### Priority 4: Hard Gate Precision

```bash
cp goals/goal-04-hard-gate-precision.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows 90% or higher agreement with expert judgment on a fixed set of 10 borderline ideas when applying the Hard Gate. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. The core Hard Gate criteria and refusal policy are unchanged. Or stop after 20 iterations or 5 consecutive iterations with no improvement in agreement rate.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

### Priority 5: Token Efficiency (Standard Strong Mode)

```bash
cp goals/goal-05-token-efficiency.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows at least 20% reduction in tokens for Standard Strong Mode runs with no drop in output quality scores on a fixed test set. The most recent kept iteration's harness column is PASS. All rows marked positive in staging/generating-ai-ideas/test-prompts.md still fire. Council Report and Council Test Notes quality are maintained or improved. Or stop after 20 iterations or 5 consecutive iterations with no improvement on the combined metric.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

### Priority 6: Refusal Policy Robustness

```bash
cp goals/goal-06-refusal-policy-robustness.md goal.md
```

Then run in chat:
```
/goal The improving-skills loop on staging/generating-ai-ideas/SKILL.md has produced a kept iteration whose row in runs/<run-id>/results.tsv shows 100% refusal rate on a fixed set of harmful-intent adversarial prompts with clear, consistent, and policy-aligned refusal language, while maintaining 100% trigger accuracy on positive prompts in test-prompts.md. The most recent kept iteration's harness column is PASS. The Scope and Refusal Policy and the 6th Hard Gate red flag are unchanged. Or stop after 15 iterations.
/improving-skills staging/generating-ai-ideas/SKILL.md
```

---

## Recommended Workflow

1. Start with **Priority 1** (highest leverage)
2. After it completes (or you stop it), review the results in `runs/`
3. Activate the next priority by copying its file to `goal.md`
4. Repeat

You can also run them in any order you prefer.

**Current recommendation**: Begin with Goal 01.