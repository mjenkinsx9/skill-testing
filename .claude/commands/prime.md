---
description: Load foundational context for the skill testing codebase
---

# Purpose

Orient yourself in the skill-testing repo: a playground for authoring, evaluating, security-reviewing, and autoresearch-improving Claude agent skills against Anthropic's published best-practices.

CLAUDE.md is already loaded automatically — only read what's additive to it.

## Instructions

1. Read `README.md` — the 4 workflows (author / import / security review / autoresearch) and the cross-platform setup table.
2. Read `eval/README.md` — the 20-check harness table with PASS/WARN/FAIL gradations.
3. Skim `examples/reviewing-pull-requests/` — read `SKILL.md`, `tests.md`, `test-prompts.md`, and `references/reference.md` to see what a passing skill and its sidecars look like in practice.
4. Read `.claude/skills/improving-skills/references/loop.md` and `references/scoring.md` — autoresearch iteration mechanics and composite scoring formula.

When actually authoring a skill, consult `templates/skill-skeleton/` for the scaffold — that's reference material, not orientation.
