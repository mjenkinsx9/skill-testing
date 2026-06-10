# Promotion Record — improving-skills

**Skill**: improving-skills (live tool skill; `disable-model-invocation: true`)
**Location**: `.claude/skills/improving-skills/` — ships with the repo rather
than being promoted from `staging/`. This record exists so the harness's
Check 15 has a value-add verdict to read and to document why the value-add
test does not apply.

> Mechanical gates (harness 0 FAIL, tests.md scenarios + freshness, naming,
> sidecar linking) are machine-enforced by `eval/check-skill.sh` and CI and
> are not re-attested here.

## Real-work evidence

The loop mechanics were derived from real autoresearch runs against this
repo's own skills (run audit logs lived in the gitignored `runs/`); the
references (`loop.md`, `scoring.md`) codify the stopping conditions and
composite that those runs converged on.

## Security review

No secrets. `allowed-tools` is scoped intentionally to the harness, the
skill's own scripts, and git/file operations — the loop cannot reach outside
the repo.

## Value-add verdict

- [x] **Value-add verdict**: N/A (non-generative) — this is a meta-tool that
      runs the modify→score→keep-or-revert loop; "beats cold model on rigor"
      is the wrong frame (see `eval/value-add-test.md` → applicability limits).

## Strip list

None — `scripts/`, `templates/`, and `references/` are all runtime material.

## Live test

Invoked explicitly via `/improving-skills` across multiple runs (see the
formerly-kept `runs/` audit logs); never auto-fires by design.
