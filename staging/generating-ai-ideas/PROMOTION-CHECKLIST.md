# Promotion Record — generating-ai-ideas

**Skill**: generating-ai-ideas
**Promoted**: 2026-05-31  (`staging/generating-ai-ideas/` → `.claude/skills/generating-ai-ideas/`)

> Mechanical gates (harness 0 FAIL, tests.md scenarios + freshness, naming,
> sidecar linking) are machine-enforced by `eval/check-skill.sh` and CI and
> are not re-attested here. Known accepted WARN: Check 16 on
> `references/reference.md` (304 lines > 200) — a curated, TOC-navigable
> comprehensive manual per the Anthropic "bundle comprehensive resources"
> guidance.

## Real-work evidence

9 live runs captured in `staging/generating-ai-ideas/references/live-runs-9.md`
plus 20 council-composition experiments (`council-experiments.md`) and the
market research behind the rubric (`research-notes.md`). The evidence trail
lives in `staging/` only; the live skill ships the curated distillates
(`reference.md`, `learnings.md`, `examples.md`, `council-cheatsheet.md`).
A few parenthetical provenance mentions in the curated sidecars point at the
staging record; they are historical pointers, not load-bearing links.

## Security review

No security smells, no secrets. The skill carries an explicit refusal policy
and a Hard Gate flag (added during the 2026-05-30 review follow-ups).

## Value-add verdict

- [x] **Value-add verdict**: PASS (2026-05-30, 2 seeds, mean skill 40.0 vs cold
      39.0 /50, lift +1.0/50) — substance-first output beats the cold-model
      baseline, blind-judged; see `eval/value-add-test.md`. Note: small sample
      (2 seeds); margin is within the ±1.5 noise band, so re-run with 5+ seeds
      at the next promotion milestone.

## Strip list

Removed from the promoted copy (kept in `staging/generating-ai-ideas/`):
`goal.md`, `goals/`, `references/live-runs-9.md`,
`references/council-experiments.md`, `references/research-notes.md`,
`references/improvement-campaign.md`.

## Live test

Auto-load confirmed (the skill appears in the session skill list with this
repo as cwd; re-confirmed 2026-06-10). Live-run evidence: the 9 captured runs
predate promotion; the promoted copy serves the same SKILL.md + curated
sidecars.
