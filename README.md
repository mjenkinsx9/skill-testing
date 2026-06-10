<div align="center">

# 🛠️ skill-testing

### A test harness and workshop for Claude agent skills

**Author, evaluate, security-review, and autonomously improve skills — with evidence, not vibes.**

[![CI](https://github.com/mjenkinsx9/skill-testing/actions/workflows/harness.yml/badge.svg)](https://github.com/mjenkinsx9/skill-testing/actions/workflows/harness.yml)
![Harness checks](https://img.shields.io/badge/harness_checks-21-blue)
![Bash](https://img.shields.io/badge/bash-3.2%2B-4EAA25?logo=gnubash&logoColor=white)
![Python](https://img.shields.io/badge/python-3.x-3776AB?logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/platform-linux%20%7C%20macos%20%7C%20windows-lightgrey)
![Made for Claude Code](https://img.shields.io/badge/made_for-Claude_Code-d97757)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

</div>

---

Every skill in this repo aims to follow Anthropic's published
[agent-skills best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
([overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) ·
[Claude Code skills](https://code.claude.com/docs/en/skills)) — and the tooling here
exists to *prove* it rather than assume it.

The core idea: **a skill is only as good as the evidence behind it.** Skills are
built from real runs (never from imagination), linted by a 21-check static harness,
exercised live against their own test scenarios, and — for generative skills —
blind-judged against a no-skill baseline to confirm they actually beat just asking
the model.

## ✨ What's inside

| Layer | Tool | What it proves | Cost / cadence |
|---|---|---|---|
| 🧹 **Static** | [`eval/check-skill.sh`](eval/check-skill.sh) | The skill is *well-formed* — 21 checks: frontmatter, naming, trigger language, tree-wide secret & security-smell scans, sidecar hygiene, staging-drift | Free · every edit |
| ⚡ **Behavioral** | [`eval/behavioral-check.py`](eval/behavioral-check.md) | The documented commands *actually run* against the real system today | ~0 tokens · on touch |
| 🎯 **Empirical trigger** | [`eval/trigger-accuracy.py`](eval/trigger-accuracy.md) | The skill *really fires* on its positive prompts and declines its negatives (observed, not self-predicted) | Sub-agents · milestone |
| ⚖️ **Value-add** | [`eval/value-add-test.sh`](eval/value-add-test.md) | The skill *beats the cold model* in a blind head-to-head | LLM-judged · pre-promotion |
| 🔄 **Autoresearch** | [`improving-skills`](.claude/skills/improving-skills/SKILL.md) | Iterative modify → score → keep-or-revert loop that tightens an existing SKILL.md | Heavy · on demand |

The harness is self-tested (10 pathological fixtures + 26 pytest tests) and CI
enforces the repo invariants on every push and PR.

## 🚀 Quick start

```bash
git clone https://github.com/mjenkinsx9/skill-testing.git
cd skill-testing
pip install pyyaml            # the only Python dependency

# Lint any skill directory (or a SKILL.md directly):
eval/check-skill.sh examples/reviewing-pull-requests
```

```
checking: examples/reviewing-pull-requests/SKILL.md
  PASS  1. frontmatter parses as YAML
  PASS  2. name 'reviewing-pull-requests' is valid
  ...
summary: 0 FAIL, 0 WARN
```

Exit code is `0` only with zero FAILs — WARNs are prompts to look, not blockers.

> **Note:** open a Claude Code session with this repo as the working directory and
> everything in `.claude/skills/` auto-loads — edits are picked up live, no restart
> needed.

## 📁 Repository layout

```
.claude/skills/            live skills, auto-loaded by Claude Code in this repo
  improving-skills/        purpose-built autoresearch loop for SKILL.md improvement
staging/                   WIP / under-review skills (NOT auto-loaded)
examples/                  exemplary reference skills (always pass the harness cleanly)
templates/skill-skeleton/  scaffold: SKILL.md + tests.md + PROMOTION-CHECKLIST + sidecars
eval/                      the evaluation stack (see table above)
  tests/                   self-tests for the harness itself (fixtures + pytest)
runs/                      autoresearch working dirs (gitignored)
```

## 🔁 Workflows

### 1 · Author a new skill

**Never write a skill from imagination.** Do the task manually 2–3 times, then
codify what actually worked — see [CLAUDE.md](CLAUDE.md) for the full process and
its anti-patterns.

```bash
# 1. After 2-3 real manual runs, scaffold a draft in staging/:
mkdir -p staging/<gerund-form-name>
cp -r templates/skill-skeleton/. staging/<gerund-form-name>/

# 2. Lint while drafting (WARNs are OK in staging/):
eval/check-skill.sh staging/<gerund-form-name>

# 3. Promote once it passes cleanly, tests.md captures real runs, and the
#    PROMOTION-CHECKLIST.md is filled in:
cp -r staging/<gerund-form-name> .claude/skills/
eval/check-skill.sh .claude/skills/<gerund-form-name>   # must be 0 FAIL
```

Optionally anchor the work first with `/goal-new-skill <name>` — it interviews you
into a verifiable end state and writes `staging/<name>/goal.md`.

For a **generative/judgment** skill, also run the value-add baseline before
promoting — a maxed linter score proves structure, not value. See
[eval/value-add-test.md](eval/value-add-test.md).

### 2 · Import an existing skill

Drop the folder under `staging/<name>/`, read it carefully, run the harness, and
promote with `cp -r staging/<name> .claude/skills/` when satisfied.

### 3 · Security-review a skill

Same as import — but keep it in `staging/` while reviewing so it can't auto-load
and trigger. The harness scans the **whole skill tree** (including `scripts/` and
`references/`) for security smells (`curl … | sh`, `rm -rf`, base64 decodes,
outbound POSTs) and leaked credentials. Smells are WARNs — prompts to look, not
verdicts. The human reviewer decides.

### 4 · Autoresearch-improve a skill

```
/goal-improve-skill examples/reviewing-pull-requests   # optional: set a measurable target
/improving-skills examples/reviewing-pull-requests/SKILL.md
```

Runs a modify → score → keep-or-revert loop against a fixed composite:

- **Mechanical floor** — `eval/check-skill.sh` must pass every kept iteration
- **Trigger accuracy** — positive + negative fixture prompts (simulated per
  iteration, measured empirically once at loop end)
- **Instruction quality** — LLM-as-judge against the skill's own `tests.md` scenarios
- **Token efficiency** — rewards shrinking, never penalizes a smaller body

The composite proves a skill is *well-formed and tight* — **not** that it beats
just asking the model. So when the loop stops, it runs the value-add baseline once
on the final candidate and records the verdict. Output: a scratch branch with one
commit per kept iteration plus a `runs/<run-id>/results.tsv` audit log. See
[scoring.md](.claude/skills/improving-skills/references/scoring.md) and
[loop.md](.claude/skills/improving-skills/references/loop.md).

## 🔍 The eval harness in depth

The full 21-check table — levels, pragmas (`allowlist secret`,
`allowlist windows-path`), and interpretation guidance — lives in
[eval/README.md](eval/README.md). Highlights:

- **Tree-wide security scanning** — secrets and security smells are checked in
  every `.md` and script file in the skill, not just `SKILL.md`
- **Trigger-language lint** — descriptions must carry genuine "use when…" trigger
  phrasing with ≥2 concrete conditions; vague prose doesn't pass
- **Sidecar hygiene** — `tests.md` with a `Last verified:` date is required outside
  `staging/`; reference docs belong in `references/`, scripts in `scripts/`, and
  orphan files that `SKILL.md` never links get flagged
- **Staging-drift detection** — a promoted skill is diffed against its `staging/`
  twin so the two can't silently diverge

## 🧪 Testing the tests

The harness itself is under test — because a linter you can't trust is worse than
no linter:

```bash
bash eval/tests/run-self-tests.sh    # 10 fixture skills with known verdicts
python -m pytest eval/tests -q      # 26 tests for the Python eval scripts
```

CI ([`harness.yml`](.github/workflows/harness.yml)) runs on every push and PR:
`examples/` must score **0 FAIL, 0 WARN**, live and staging skills must score
**0 FAIL**, and both self-test suites must pass.

## 💻 Requirements

Scripts are `bash` + a handful of POSIX tools + Python, and run identically on
Linux, macOS, and Windows.

| Platform | Shell | Setup |
|---|---|---|
| **Linux** | system `bash` | `python3` + `pip install pyyaml` |
| **macOS** | system `bash`/`zsh` (scripts use `#!/usr/bin/env bash`) | `python3` + `pip install pyyaml` |
| **Windows** | [Git Bash](https://git-scm.com/download/win) or WSL | `python` from [python.org](https://www.python.org/downloads/) + `pip install pyyaml` — the harness tries `python3` / `python` / `py -3` and skips the silent MS-Store shim |

All scripts use forward-slash paths and LF line endings (enforced via
`.gitattributes`). Documented commands like `cp -r` are POSIX — run them in bash.

## 📚 Documentation map

| Doc | What it covers |
|---|---|
| [CLAUDE.md](CLAUDE.md) | The build process, anti-patterns, promotion/demotion rules, sidecar policy |
| [eval/README.md](eval/README.md) | The full 21-check table + all four eval layers |
| [WRITING-DESCRIPTIONS.md](WRITING-DESCRIPTIONS.md) | How to write the `description:` — the primary trigger mechanism |
| [templates/skill-skeleton/](templates/skill-skeleton) | The scaffold every new skill starts from |

## 🤝 Contributing

PRs welcome. The bar is mechanical and enforced by CI:

1. `eval/check-skill.sh` must report **zero FAILs** on any skill you touch
   (zero WARNs if it's in `examples/`).
2. `tests.md` scenarios must come from runs you actually performed — fabricated
   scenarios are the one unforgivable sin here.
3. If you change the harness, the self-tests must pass (and probably grow).

## 📄 License

[MIT](LICENSE) © 2026 Mike Jenkins
