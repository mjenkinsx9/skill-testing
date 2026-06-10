# Promotion Checklist — improving-skills

**Skill**: improving-skills (live tool skill; `disable-model-invocation: true`)
**Location**: `.claude/skills/improving-skills/`

## Pre-promotion verification

- [x] `eval/check-skill.sh .claude/skills/improving-skills` → 0 FAIL
- [x] `tests.md` present with scenarios + `Last verified:` date
- [x] Sidecars (`loop.md`, `scoring.md`, `value-add` references, `scripts/`, `templates/`) referenced one level deep
- [x] No secrets; `allowed-tools` scoped intentionally
- [x] Name is gerund form, matches folder
- [x] **Value-add verdict**: N/A (non-generative) — this is a meta-tool that runs the
      modify→score→keep-or-revert loop; "beats cold model on rigor" is the wrong frame
      (see `eval/value-add-test.md` → applicability limits)

## Notes

This skill ships with the repo rather than being promoted from `staging/`. The
checklist exists so the harness's Check 15 has a value-add verdict to read and
to document why the value-add test does not apply here.
