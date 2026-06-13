# Tests for improving-skills

Verification scenarios. The learnings-seeding scenario below has been
exercised end-to-end (bounded run); the others remain illustrative — refresh
their `Last verified` and "Verified output" sections after their first real run.

Last verified: 2026-06-13

## Golden path: tighten an existing skill

**Input:** `/improving-skills examples/reviewing-pull-requests/SKILL.md`
where the target ships a populated `test-prompts.md`.

**Expected behavior:**
1. Copies the target into `runs/<run-id>/skill/`.
2. Scores baseline; appends iter 0 row to `results.tsv`.
3. Proposes one focused edit per iteration, scores via
   `scripts/score-skill.sh`, keeps improvements / reverts the rest.
4. Stops on iteration cap, no-improvement-for-5, or score ≥ 0.95.
5. Emits a final summary citing baseline score, best score, token delta,
   and the path to `results.tsv`.

**Verified output:** TBD — refresh after first end-to-end run.

## Learnings-seeding: open retro entries drive candidate edits

**Input:** `/improving-skills staging/writing-release-notes/SKILL.md`, where the
target ships a `references/learnings.md` with a "Retro log" of `Status: open`
entries (an over-broad description catch-all, a duplicated step + section, a
filler intro, and an empty-input quality bug).

**Expected behavior:**
1. Step 2 reads `references/learnings.md` and folds the open entries into
   `runs/<run-id>/program.md` as a "Candidate edits from … retro log" backlog.
2. The loop tries the seeded edits first, scores each, and keeps only the
   composite-raising ones — candidates, not mandates: a flagged edit that
   doesn't raise the composite is reverted, not blindly applied.
3. Emits a summary citing baseline vs best composite and the kept edits.

**Verified output (2026-06-13, run `2026-06-13-1554`; capped at 3 iters,
simulated trigger, scoring sub-agents on haiku):**
- `program.md` seeded with all 4 open entries as the candidate backlog.
- iter 0 baseline composite **0.893** (trigger 1.00, quality 0.73, eff 1.00).
- iter 1 *tighten description* (drop the "any code changes" catch-all) →
  **reverted**: the trigger judge already declined the commit-message /
  PR-review negatives, so removing the catch-all left the composite flat at
  0.893. Confirms seeded edits aren't applied blindly.
- iter 2 *add empty-PR handling* (learnings #4) → **kept**: Scenario 3 quality
  2/10→7/10, composite 0.893→**0.953**.
- iter 3 *remove duplicated Step 4 + "Formatting the notes" section*
  (learnings #2) → **kept**: efficiency 0.963→1.00 (tokens 406→356),
  composite 0.953→**0.960**.
- Filler-intro entry (learnings #3) left `open` — iteration cap reached; a
  follow-up run would try it next.

## Edge case: missing test-prompts.md

**Input:** `/improving-skills <skill-with-no-test-prompts>`.

**Expected behavior:** Skill detects the missing file and asks the user
to author `test-prompts.md` before continuing rather than running the
loop with zero discrimination data.

**Verified output:** TBD.

## Edge case: every mutation fails the harness

**Input:** A skill where small edits keep tripping the third-person or
description-length FAIL.

**Expected behavior:** Auto-revert each FAIL'd iteration, log them with
`harness=FAIL kept=no` in `results.tsv`, and stop on the
no-improvement-for-5 condition rather than looping indefinitely.

**Verified output:** TBD.

## Error case: target path does not exist

**Input:** `/improving-skills nonexistent/path/SKILL.md`.

**Expected behavior:** Reports the missing file plainly and stops before
creating a run directory. Does not silently fall back to a default.

**Verified output:** TBD.
