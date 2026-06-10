# CLAUDE.md

Instructions for working in this repo. Specific rules override general ones.

## About this repo

This is a skill-building/testing/evaluating playground:

- `.claude/skills/` — live skills auto-loaded by Claude Code when this repo is cwd
- `.claude/skills/improving-skills/` — autoresearch loop scoped to SKILL.md improvement
- `staging/` — WIP / under-review / security-audit skills (NOT auto-loaded)
- `examples/` — exemplary reference skills (must always pass the harness cleanly)
- `templates/skill-skeleton/` — best-practices-aligned scaffold for new skills (incl. `PROMOTION-CHECKLIST.md.template`)
- `eval/` — the evaluation stack: `check-skill.sh` (static 21-check harness, run before committing any skill), `behavioral-check.py` (runs a skill's tests.md commands live), `trigger-accuracy.py` (observes real skill firing), `value-add-test.md` (dynamic: does a generative skill beat a no-skill baseline?), and `tests/` (self-tests for the harness itself)

Read `README.md` for the four workflows (author, import, security review, autoresearch-improve).

Verification here is manual runs → `tests.md` scenarios → `eval/check-skill.sh` → (for generative skills) the value-add test — *not* unit-test TDD. When the `test-driven-development` skill fires, map "write a test" onto a `tests.md` scenario, not a unit test.

## Building skills (most important rule)

**Never write a skill from imagination. Skills come from doing real work first.**

The process:

1. **Pick one task.** Not "handle operations" — one specific task with one clear outcome. Optionally run `/goal-new-skill <gerund-form-name>` to interview yourself into a measurable end state and persist it as `staging/<name>/goal.md`.
2. **Do it live.** Walk through it together in a Claude Code session. Correct mistakes, add missed steps, confirm what works. You're doing the work, not writing instructions.
3. **Do it again.** Run the same task 2–3 more times with different inputs. Catch edge cases — empty results, missing data, weird formats. This is where the skill gets real.
4. **Codify it.** After 2–3 successful runs: `cp -r templates/skill-skeleton staging/<gerund-form-name>` and write `SKILL.md` + `tests.md` from experience, not theory. Strip any optional sidecar templates you don't need (`references/`, `scripts/`).
5. **Test as a skill.** Fill in `PROMOTION-CHECKLIST.md`, then promote to `.claude/skills/<name>/` (`cp -r staging/<name> .claude/skills/`). Run the same scenarios through the skill instead of manually. Same quality? Good. Something off? Fix it and update the skill.
6. **Let it fail.** Use it for a week. When it breaks, don't just fix the output — fix the skill so that failure can't happen again.
7. **Stop touching it.** When it just works, move on.

```
Do it manually (2-3x) → Codify into SKILL.md → Test → Fail/fix/update → Stable → Move on
```

**The trap:** Writing a detailed SKILL.md on day one from your head. It will be wrong in ways you can't predict until you've run the workflow for real.

### Scope: when one task is really several

Step 1 says "pick one task," but "one task" is about a single coherent flow, not a single rubric item. Split into separate skills when **either** holds:

- **Distinct trigger audiences.** The phrasings that should fire it fall into unrelated buckets a single description can't sharpen around without going generic (and tripping Check 7b). One audience → one skill.
- **Independent sub-procedures with separate outputs.** The steps branch into parts that don't share state and each emit their own deliverable. If you could run half the skill and hand off a complete artifact, that half is its own skill.

A skill covering many rubric *areas* over one input is still one task. `reviewing-pull-requests` checks correctness, tests, security, and style — four areas — but it's one task: one diff in, one prioritized comment list out, one trigger ("review this PR"). Don't split that. Do split "review my PR and also deploy it" — two audiences, two outputs.

### Anti-patterns

- **Autoresearching a brand-new skill.** `improving-skills` polishes an existing SKILL.md against test-prompts.md and tests.md. It is not a substitute for doing the work manually 2–3 times first.
- **Writing SKILL.md as documentation.** Skills are instructions to Claude ("Read X. Apply Y. Emit Z."), not exposition about what the skill is. If a section starts with "This skill helps...", rewrite it imperatively.
- **Broadening the description "just in case."** Trades trigger precision for recall and silently degrades discrimination. Sharpen the trigger language instead of widening it.
- **Inlining executable logic as prose.** If the instructions describe a deterministic procedure ("loop through files, count matches, sort by..."), it belongs in `scripts/` as code the skill executes, not as instructions Claude re-derives each invocation. Reserve SKILL.md prose for decisions scripts can't make.

### Mechanical checks

Every new or modified skill must clear the harness:

```bash
eval/check-skill.sh staging/<name>          # or examples/<name>, .claude/skills/<name>
```

Run it after every SKILL.md edit, on both source and destination after `cp -r`, and before opening a PR. Zero FAILs required; WARNs are prompts to look. See `eval/README.md` for the full check list.

CI (`.github/workflows/harness.yml`) enforces this on every push/PR: `examples/` must be 0 FAIL **and** 0 WARN, live and staging skills 0 FAIL.

**When modifying the harness itself** (`eval/check-skill.sh` or the Python eval scripts), the self-tests must pass before committing — and a behavior change should come with a fixture or test that pins it:

```bash
bash eval/tests/run-self-tests.sh
python -m pytest eval/tests -q
```

### Value-add check (generative skills)

The harness proves a skill is *well-formed*; it cannot prove the skill *beats just asking the model*. A skill can be perfectly structured and still lose a blind head-to-head to the cold model — its scaffolding burying its rigor. Before promoting a **generative/judgment** skill (idea generation, review, research, summarization), run the value-add test (`eval/value-add-test.md`) and record the verdict (`PASS / CONCERN / FAIL`, date, lift, seeds) in the skill's PROMOTION-CHECKLIST. A CONCERN/FAIL must be acknowledged — with a fix or a written justification — never silently ignored.

It is advisory, not a hard gate (only `check-skill.sh` hard-blocks), and it is a pre-promotion milestone check, not a per-edit one. It does **not** apply to deterministic, safety/refusal, or format-compliance skills, where "beats the cold model" is the wrong frame — record `N/A` for those. See `eval/value-add-test.md` for applicability and method.

## Using improving-skills

The `improving-skills` skill autoresearches improvements to an existing SKILL.md.

- Invoke explicitly: `/improving-skills <path-to-SKILL.md>`. It has `disable-model-invocation: true` and will not auto-fire.
- Optionally set a measurable target first: `/goal-improve-skill <skill-dir>` interviews you on axis + numeric target + preserved behavior, writes `<skill-dir>/goal.md`, and prints a `/goal` line to paste. The loop seeds `program.md` from `goal.md` when present.
- Prerequisites: the target skill must ship both `tests.md` (≥3 scenarios) and `test-prompts.md` (positive + negative trigger fixtures). The loop refuses to start without them.
- Output: a scratch git branch and `runs/<run-id>/results.tsv` audit log. `runs/` is gitignored; do not commit run artifacts.
- When the loop stops, it runs the value-add test once on the final candidate (for generative skills) and records the verdict — a maxed composite proves the skill is tight, not that it beats just asking the model. See `eval/value-add-test.md`.
- See `.claude/skills/improving-skills/references/loop.md` and `references/scoring.md` for stopping conditions and the composite score.

## Supporting files in a skill

Keep SKILL.md focused on overview + navigation. Move detail into sibling files in the skill directory — Claude loads them only when needed (progressive disclosure).

```
my-skill/
├── SKILL.md              # Overview and instructions (required, keep ≤500 lines)
├── tests.md              # at least 3 verification scenarios (quality scoring) — required
├── test-prompts.md       # Positive/negative trigger fixtures (trigger-accuracy scoring) — only for autoresearch
├── PROMOTION-CHECKLIST.md  # Promotion gates for skills in .claude/skills/ — see Skill location rules
├── scripts/              # Utility scripts — executed, not loaded
├── templates/            # Skill-local templates (e.g. program.md scaffolds)
└── references/           # Detailed docs loaded on demand — recommended for multi-topic skills
    ├── reference.md
    ├── examples.md
    ├── learnings.md
    └── ...
```

**Reference docs go in `references/`; scripts go in `scripts/`.** As soon as a skill carries reference content besides `SKILL.md`, put it in a `references/` subdir — don't leave `reference.md`/`examples.md`/etc. loose at the skill root (matches the official [skill structure docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)). `check-skill.sh` Check 18 WARNs on any non-governance `.md` beside `SKILL.md`, and Check 19 WARNs on any loose script (`*.py`/`*.sh`/… belongs in `scripts/`). Only `SKILL.md` and the required sidecars (`tests.md`, `test-prompts.md`, `PROMOTION-CHECKLIST.md`, `goal.md`) stay at the root. Each reference file should also stay under 200 lines (Check 16 prompts a split), and any over 100 lines should open with a `## Contents` table of contents so Claude can navigate it from a partial read (Check 17).

Reference supporting files from SKILL.md so Claude knows they exist and what they contain. Keep references one level deep (no `a → b → c` chains). A bundled file that SKILL.md never links is an orphan — Claude won't load it; Check 20 flags it so you either link it or move it out of the promoted skill (working/meta docs like a campaign tracker belong in `staging/`, not the live skill).

**Which sidecars to keep vs strip.** `tests.md` is always required (outside `staging/`). The rest earn their place or get deleted:

- `references/` files — keep only when the detail would push the SKILL.md body toward the ~500-line budget, or is genuinely needed on demand rather than every run. Otherwise inline it. If a single file grows past 200 lines, split into multiple topic-focused files within `references/`.
- `test-prompts.md` — only if you plan to autoresearch-improve the skill. `scripts/` — only if the skill executes deterministic logic (see the inlining anti-pattern).

Worked contrast: `reviewing-pull-requests` ships a single `references/reference.md` (a multi-section rubric that would bloat the body) and no `examples.md`; a simple self-explanatory skill ships neither — just `SKILL.md` + `tests.md`. `generating-ai-ideas` and `improving-skills` carry a fuller `references/` set. When in doubt, strip — an unused sidecar is just maintenance surface.

### tests.md sidecar (required outside staging/)

At least 3 verification scenarios (golden / edge / error; add more as you verify them — optional integration too). Include a `Last verified: YYYY-MM-DD` line so staleness is visible. Add tests opportunistically — when you touch a skill for other reasons, capture the scenarios you just verified. **Don't fabricate scenarios for skills you haven't recently exercised.**

Harness FAILs on missing tests.md outside `staging/`, WARNs inside.

## Skill location rules

- **New work** lives in `staging/<gerund-name>/` until it passes the harness AND the tests.md scenarios verify.
- **Promote** with `cp -r staging/<name> .claude/skills/`, then **strip staging-only working files** from the promoted copy — `goal.md`, `goals/`, and any raw "do-the-work-first" evidence logs that aren't referenced from `SKILL.md` (the live skill ships only the curated sidecars Claude loads at runtime; the evidence trail stays in `staging/`). Record the exact strip list in the skill's `PROMOTION-CHECKLIST.md`. Fill in `PROMOTION-CHECKLIST.md` first (shipped in `templates/skill-skeleton/`); it records the pre-promotion gates, including the value-add verdict for generative skills. `check-skill.sh` (Check 15) warns if a promoted skill lacks it.
- **Demote rather than fix in place.** If a skill in `.claude/skills/` regresses or FAILs the harness, move it back to `staging/` and fix there. Concretely:
  1. `mv .claude/skills/<name> staging/<name>` (if a `staging/<name>` already exists as the working copy, reconcile into it rather than clobbering).
  2. Fix and re-verify in `staging/`: `eval/check-skill.sh staging/<name>` to zero FAILs, re-run the `tests.md` scenarios, and (for a generative skill) re-run the value-add test, recording the new verdict in `PROMOTION-CHECKLIST.md`.
  3. Re-promote with the normal `cp -r staging/<name> .claude/skills/` flow.
  - Why move, not edit live: promoted skills are copies, so `.claude/skills/<name>` carries no edit history — the `staging/` copy is the working record where fixes and their rationale live.
- **Examples** in `examples/` are reference material — they should be exemplary at all times. Never break them; if you need to demonstrate failure, use a temp file outside the repo.

## Naming and frontmatter

Rules (lowercase-hyphen names, gerund preferred, third-person descriptions, ≤1024 chars, etc.) are enforced by `eval/check-skill.sh` and documented inline in `templates/skill-skeleton/SKILL.md.template`. Push back if a check feels wrong rather than working around it.

The `description:` is the primary trigger mechanism — getting it right (imperative, intent-focused, ≥2 concrete conditions, pushy but sharp) is its own skill. See [WRITING-DESCRIPTIONS.md](WRITING-DESCRIPTIONS.md).
