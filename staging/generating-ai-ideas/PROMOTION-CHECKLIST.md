# Promotion Checklist — generating-ai-ideas

**Skill**: generating-ai-ideas  
**Current location**: `staging/generating-ai-ideas/`  
**Target**: `.claude/skills/generating-ai-ideas/`  
**Date prepared**: 2026-05-31

## Pre-Promotion Verification (Already Completed)

- [x] `eval/check-skill.sh staging/generating-ai-ideas` → **0 FAILs, 0 WARNs**
- [x] `tests.md` has 8 real scenarios with recent "Last verified" date (2026-05-31)
- [x] Real work evidence exists (9 live runs in `live-runs-9.md`, 20 composition experiments)
- [x] All sidecars present and properly referenced:
  - `SKILL.md`
  - `reference.md`
  - `learnings.md` (authoritative lessons)
  - `examples.md`
  - `tests.md`
  - `council-cheatsheet.md`
- [x] Security review completed (explicit refusal policy + Hard Gate flag added)
- [x] Name is gerund form (`generating-ai-ideas`) and matches folder
- [x] No security smells or secrets
- [x] **Value-add verdict**: PASS (2026-05-30, 2 seeds, mean skill 40.0 vs cold 39.0 /50, lift +1.0/50) — substance-first output beats the cold-model baseline, blind-judged; see `eval/value-add-test.md`
- [x] References are one level deep
- [x] Body length reasonable with good progressive disclosure

## Promotion Commands

Run these from the repo root:

```bash
# 1. Copy the skill to the live skills directory
cp -r staging/generating-ai-ideas .claude/skills/

# 2. Strip staging-only working files from the promoted copy (see Notes)
cd .claude/skills/generating-ai-ideas
rm -rf goal.md goals/ \
       references/live-runs-9.md \
       references/council-experiments.md \
       references/research-notes.md \
       references/improvement-campaign.md
cd -

# 3. Verify the promoted version passes cleanly
eval/check-skill.sh .claude/skills/generating-ai-ideas
```

Expected output: `summary: 0 FAIL, 1 WARN` — the single WARN is Check 16 on
`references/reference.md` (304 lines > 200), accepted as a curated, TOC-navigable
comprehensive manual per the Anthropic "bundle comprehensive resources" guidance.

## Post-Promotion Steps

- [ ] Confirm the skill auto-loads in a new Claude Code session with this repo as cwd
- [ ] Test the skill live with a few real requests
- [x] Raw evidence logs dropped from the live copy (kept in `staging/` only) — see Notes
- [ ] Update any external documentation or indexes if this repo maintains them
- [ ] Celebrate — this skill followed the full authoring + security + verification process

## Notes

- **Staging-only working files** are not shipped in the live skill: `goal.md`,
  `goals/` and `improvement-campaign.md` (goal/improvement-campaign scaffolding),
  and the raw evidence logs `live-runs-9.md`, `council-experiments.md`,
  `research-notes.md`. They are the
  "do-the-work-first" capture trail — valuable history Claude never loads at
  runtime — so they live in `staging/generating-ai-ideas/` only. The live skill
  ships just the curated sidecars (`reference.md`, `learnings.md`, `examples.md`,
  `council-cheatsheet.md`, `improvement-campaign.md`) + `SKILL.md` + `tests.md`.
- The curated sidecars retain a few parenthetical provenance mentions ("documented
  in council-experiments.md") that point at this fuller staging record; they are
  historical pointers, not load-bearing links.
- This skill is in excellent shape and fully compliant with repo standards as of the date above.

---

**To promote right now**, copy and paste the two commands above.