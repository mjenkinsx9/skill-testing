# Improvement Goals for generating-ai-ideas

This directory contains focused, measurable goals for the `improving-skills` autoresearch loop.

Each file represents one priority area identified during the 20 council composition experiments + 9 live runs (May 2026).

## How to Use

1. Copy the desired goal file to `goal.md` in the skill root:
   ```bash
   cp goals/goal-01-smarter-mode-role-selection.md goal.md
   ```

2. Launch the improvement run:
   ```
   /goal <paste the synthesized /goal condition from the file>
   /improving-skills staging/generating-ai-ideas/SKILL.md
   ```

3. After the run completes (or you decide to stop), review `runs/<run-id>/results.tsv`.

4. If you want to switch to a different priority, repeat step 1 with the next goal file.

## Available Goals (in recommended order)

| File | Priority | Focus Area | Recommended First? |
|------|----------|------------|---------------------|
| `goal-01-smarter-mode-role-selection.md` | 1 (Top Tier) | Autonomous mode + role selection | **Yes – Highest leverage** |
| `goal-02-researcher-effectiveness.md` | 2 (Top Tier) | Research efficiency in hard verticals | Strong second choice |
| `goal-03-council-test-notes-quality.md` | 3 (Top Tier) | Quality of self-improvement logging | Very valuable for long-term health |
| `goal-04-hard-gate-precision.md` | 4 (Second Tier) | Reducing false positives/negatives on borderline ideas | Good once 1–3 are addressed |
| `goal-05-token-efficiency.md` | 5 (Second Tier) | Reducing token usage in Standard Strong Mode | Useful for cost/latency |
| `goal-06-refusal-policy-robustness.md` | 6 (Second Tier) | Hardening harmful/illegal idea refusal | Security hygiene |

## Notes

- All goals are designed to be run **one at a time**.
- Every goal requires that the skill continues to pass the harness with 0 FAILs.
- All goals preserve the mandatory Council Test Notes requirement and the Scope & Refusal Policy.
- After a successful improvement run on one goal, you can copy the next goal file over `goal.md` and continue.

## Current Status (as of 2026-05-31)

- `test-prompts.md` has been created with 10 positive and 10 negative examples.
- These 6 goal files have been prepared based on observed friction during development and live use.
- The skill is otherwise in a strong state (0 FAILs, 0 WARNs after rename to `generating-ai-ideas`).

Start with Goal 1 unless you have a strong reason to prioritize something else.