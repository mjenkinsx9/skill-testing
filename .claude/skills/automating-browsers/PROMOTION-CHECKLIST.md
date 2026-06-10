# Promotion Checklist — automating-browsers

**Skill**: automating-browsers
**From**: `staging/automating-browsers/`   **To**: `.claude/skills/automating-browsers/`
**Date prepared**: 2026-05-31

> Recorded retroactively: this skill was already promoted and in live use.
> The checklist captures the gates as actually verified, to satisfy Check 15
> and document the skill's standing.

## Pre-promotion verification

- [x] `eval/check-skill.sh .claude/skills/automating-browsers` → **0 FAIL**
      (the lone WARN was this missing PROMOTION-CHECKLIST.md; also green in CI on 2026-05-31)
- [x] `tests.md` has 6 real scenarios (golden / form / login / edge / enterprise /
      virtualized-table) — `Last verified: 2026-05-15`; the heading-extraction golden
      path and the-internet login flow were re-verified live 2026-05-31
- [x] Real-work evidence exists — the tests.md scenarios are drawn from live runs
      (Anthropic docs, the-internet.herokuapp.com, Acumatica ERP, Pure Storage SAN);
      "the patterns this skill teaches come directly from these runs"
- [x] All sidecars present and referenced from SKILL.md, one level deep:
      `references/reference.md`, `tests.md`, `test-prompts.md`
- [x] Security reviewed — no harness security smells, no secrets. Credential handling
      is constrained by the SKILL.md "Credentials" section (never hardcode/echo/commit;
      stop at unexpected login walls). Dual-use browser-driving tool for interactive,
      user-authorized use.
- [x] Name is lowercase-hyphen, gerund form (`automating-browsers`), matches the folder
- [x] **Value-add verdict**: **N/A (non-generative)** — a procedural/tool-driving skill
      (snapshot→act→verify against a live browser), not a generative/judgment skill, so
      the "beats the cold model" frame doesn't apply (per CLAUDE.md). Its value is the
      Playwright-MCP tool-selection table and hard-won pitfalls (ref drift, virtualized
      tables, link-click non-redirects) — mechanical aids, not judgment to A/B test.

## Promote

```bash
cp -r staging/automating-browsers .claude/skills/
eval/check-skill.sh .claude/skills/automating-browsers   # expect: 0 FAIL
```

## Post-promotion

- [x] Skill auto-loads in a fresh Claude Code session (repo as cwd) — confirmed; appears
      in the available-skills list
- [x] Live-tested with real requests — 2026-05-31: heading extraction + the-internet
      login flow exercised via Playwright MCP
- [ ] (Optional) development-only artifacts archived out of the promoted copy — none;
      all sidecars are production
