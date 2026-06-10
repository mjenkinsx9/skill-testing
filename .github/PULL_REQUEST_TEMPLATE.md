## Summary

<!-- What does this PR change, and why? -->

## Checklist

- [ ] `eval/check-skill.sh` reports **0 FAIL** on every skill this PR touches
      (**0 WARN** for anything in `examples/`)
- [ ] If `eval/` changed: `bash eval/tests/run-self-tests.sh` and
      `python -m pytest eval/tests -q` pass, and the behavior change is pinned
      by a new fixture or test
- [ ] Any `tests.md` scenarios added/changed document runs that actually
      happened (no fabricated scenarios)
- [ ] Docs (README / CLAUDE.md / eval/README.md) updated if behavior changed
