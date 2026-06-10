# Contributing

Thanks for your interest! This repo is a workshop for building and evaluating
Claude agent skills with evidence. Contributions that fit well: harness checks
and fixes, eval-script improvements, self-test fixtures, doc corrections, and
— with the caveat below — skills.

## Setup

```bash
git clone https://github.com/mjenkinsx9/skill-testing.git
cd skill-testing
pip install pyyaml pytest
```

Everything runs in `bash` (Git Bash on Windows) — see the README's
Requirements table.

## The bar (enforced by CI)

| What | Requirement |
|---|---|
| `examples/` skills | `eval/check-skill.sh` → **0 FAIL, 0 WARN** |
| `.claude/skills/` and `staging/` skills | **0 FAIL** (WARNs reviewed, not ignored) |
| Harness self-tests | `bash eval/tests/run-self-tests.sh` → all pass |
| Python eval scripts | `python -m pytest eval/tests -q` → all pass |

Run these locally before opening a PR — CI runs exactly the same commands.

## Three rules

1. **Run the harness on any skill you touch.** Zero FAILs, and read every WARN
   you introduce — WARNs are prompts to look, not noise to suppress.
2. **Never fabricate `tests.md` scenarios.** Scenarios document runs that
   actually happened. If you didn't run it, don't write it. This is the one
   unforgivable sin here.
3. **If you change the harness or eval scripts, pin the behavior.** A behavior
   change ships with a fixture in `eval/tests/fixtures/` or a pytest case that
   would fail without your change.

## Contributing a skill

Skills come from doing real work, not from imagination — read
[CLAUDE.md](CLAUDE.md) "Building skills" before writing one. A skill PR should
show its evidence: real `tests.md` scenarios from 2–3 manual runs, and a
filled-in promotion record (`PROMOTION-CHECKLIST.md`) if it targets
`.claude/skills/`. Imagination-built skills will be (kindly) declined.

## PRs

Keep them small and focused — one harness check, one skill, one fix. The PR
template's checklist mirrors the CI bar above. Doc changes that keep
README/CLAUDE.md/eval-README in sync with code behavior are always welcome;
doc-vs-code drift is treated as a bug here.

## Security

Found a way to sneak something past the harness? That's a security issue —
see [SECURITY.md](SECURITY.md) and report it privately.
