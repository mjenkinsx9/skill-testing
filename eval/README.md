# eval/

Skill evaluation. Checks at increasing cost:

- **`check-skill.sh`** — a fast, deterministic **static** linter (frontmatter,
  naming, line counts, security, secrets, `tests.md` presence). Run it on every
  edit. Documented below.
- **`behavioral-check.py`** (+ `behavioral-check.md`) — a deterministic **live**
  check that *runs* the skill's `tests.md` commands against the real system and
  reports each one's exit / output / sanity flags. The only check that exercises
  the tool path, and cheap (~0 tokens). See [Behavioral test](#behavioral-test-live).
- **`value-add-test.md`** (+ `value-add-test.sh`) — a **dynamic** check that asks
  what the linter and the autoresearch composite can't: *does running the skill
  actually beat just asking the model?* LLM-judged, expensive, run at promotion
  milestones. See [Value-add test](#value-add-test-dynamic).
- **`trigger-accuracy.md`** (+ `trigger-accuracy.py`) — an **empirical** check that
  asks whether a skill *actually fires* on its positive prompts and declines on its
  negatives, by spawning real sub-agents and observing the `Skill` call — versus the
  improving-skills loop's *simulated* "would you fire?" prediction. Milestone check.
  See [Trigger-accuracy test](#trigger-accuracy-test-empirical).

## Usage

```bash
eval/check-skill.sh .claude/skills/generating-ai-ideas
eval/check-skill.sh examples/reviewing-pull-requests/SKILL.md
eval/check-skill.sh .claude/skills/improving-skills
```

Accepts either a skill directory or a direct path to `SKILL.md`.

## What it checks

Encodes the published Anthropic rules from:

- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- https://code.claude.com/docs/en/skills

Each finding is labeled:

- **PASS** — check satisfied
- **WARN** — strong recommendation violated; doesn't block
- **FAIL** — hard rule violated; non-zero exit code

| # | Check | Level |
|---|---|---|
| 1 | Frontmatter parses as YAML between `---` markers | FAIL |
| 2 | `name` present, ≤64 chars, `^[a-z0-9-]+$`, no "anthropic"/"claude"/XML | FAIL |
| 3 | `name` matches folder name (when invoked on a directory) | FAIL |
| 4 | `name` uses gerund form (`processing-pdfs`) | WARN |
| 5 | `description` present, ≤1024 chars, no XML | FAIL |
| 6 | `description` in third person (no "I can…", "You can…") | FAIL |
| 7 | `description` includes genuine trigger phrasing — one of "use when"/"use whenever"/"use this skill when"/"use for"/"when the user"/"when working"/"when asked"/"trigger(s) when" (case-insensitive). A bare mid-sentence "when" or a "for \<task noun\>" clause is deliberately NOT enough — those made the check unfailable. | WARN |
| 7b | `description` trigger clause (the text after whichever phrase Check 7 matched) enumerates ≥2 concrete conditions (not a single generic one). Always emits exactly one line: WARNs when no clause is extractable after the trigger phrase, and WARNs when Check 7 itself warned (no trigger language means no enumerable conditions). | WARN |
| 8 | `description` substantive length (≥40 chars) | WARN |
| 9 | Body ≤500 lines (split into supporting files past that) | WARN |
| 10 | No Windows-style backslash paths (drive-letter `C:\…` or UNC `\\host\share`) in `SKILL.md` or reference `.md` files (same walk/exclusions as Check 16 — `scripts/`, `templates/`, `assets/`, `goals/` and governance files other than `SKILL.md` are skipped; scripts may legitimately carry Windows paths). A line tagged `allowlist windows-path` is exempt (e.g. a skill documenting a Windows CLI). Findings reported as `path:line`. | FAIL |
| 11 | References one level deep from SKILL.md | WARN |
| 12 | Security smells (`curl … \| sh`, `rm -rf`, base64 decode, outbound POSTs) across the whole skill tree — every `.md` plus every script file (`*.py *.sh *.bash *.js *.ts *.rb *.pl *.ps1`), including `scripts/`, `references/`, and `templates/`. Findings reported as `path:line`. `PROMOTION-CHECKLIST.md` alone is excluded — it records the post-promotion strip commands (`rm -rf goal.md goals/`) by design. | WARN |
| 13 | No leaked secrets (`sk-…`, `ghp_…`, `AKIA…`) anywhere in the skill tree — same `.md`+script scan set as Check 12, with no exclusions (a leaked key is never legitimate). A line tagged `allowlist secret` is exempt, in every file. Findings reported as `path:line`. | FAIL |
| 14 | `tests.md` sidecar with `Last verified:` date and ≥3 scenarios (FAIL if missing outside `staging/`, WARN inside; WARN if Last verified > 90 days old). The staleness date is read from the `Last verified:` line itself (not the first date anywhere in the file), and structural `##` headings (Contents, Notes, Setup, Prerequisites, Preamble, Background) don't count as scenarios. | FAIL/WARN |
| 15 | `PROMOTION-CHECKLIST.md` present and records a `Value-add verdict:` line — only for promoted skills under `.claude/skills/` (`staging/`, `examples/`, and bare-`SKILL.md` paths are exempt) | WARN |
| 16 | All reference `.md` files (sibling to `SKILL.md` or in `references/`) are ≤200 lines. Walks the skill dir tree; excludes `SKILL.md`, `tests.md`, `test-prompts.md`, `PROMOTION-CHECKLIST.md`, `goal.md`, and the `scripts/`/`templates/`/`assets/`/`goals/` subdirs. Suggestion on WARN: split into smaller topic-focused files or move into a `references/` subdir (per the official skill-structure docs). | WARN |
| 17 | Reference `.md` files over 100 lines open with a table of contents (an H1–H3 heading whose text contains "contents"). Same walk/exclusions as Check 16. Encodes Anthropic's "Structure longer reference files with table of contents" so Claude sees the full scope even when it previews a long file with a partial read. | WARN |
| 18 | No reference docs loose beside `SKILL.md` — any top-level `.md` that isn't a governance file (`SKILL.md`, `tests.md`, `test-prompts.md`, `PROMOTION-CHECKLIST.md`, `goal.md`, `README.md`) should move into a `references/` subdir. Fires as soon as a skill carries reference content besides `SKILL.md` (per the official skill-file-structure docs). | WARN |
| 19 | No scripts loose beside `SKILL.md` — top-level executable files (`*.py`, `*.sh`, `*.bash`, `*.js`, `*.ts`, `*.rb`, `*.pl`, `*.ps1`) belong in a `scripts/` subdir (executed, not loaded). | WARN |
| 20 | Every reference file is linked from `SKILL.md` (its basename appears there) — the reverse of Check 11, catching orphans Claude would never load. Same file set/exclusions as Check 16. Remedy: link it from `SKILL.md`, or remove it (if it's working/meta content, move it out of the promoted skill). | WARN |
| 21 | Staging/live drift — only for skills under `.claude/skills/`. If a `staging/<name>` twin exists in the same repo (repo root found by walking up to a dir containing both `.claude` and `staging`, falling back to `git rev-parse --show-toplevel`), every file present in BOTH copies is compared with `diff -q`; diverged files are listed. Extra files on either side are fine (staging keeps `goal.md` and evidence logs). PASSes with "no staging twin" or "in sync with staging/\<name\>". | WARN |

Exit code is 0 only when zero FAILs are recorded.

## Interpreting WARN

A WARN is a prompt to look — not a verdict. Security smells in particular are
patterns that *might* be malicious or *might* be a legitimate example. The
human reviewer decides.

For Check 13, a skill that must show realistic key shapes (a security-review or
secrets-handling example) can suppress the FAIL per-line by tagging it with the
`allowlist secret` marker, e.g. `AKIAIOSFODNN7EXAMPLE <!-- pragma: allowlist
secret -->`. The marker works in every scanned file (`references/`, `scripts/`,
…), not just `SKILL.md`. Only use it on deliberate examples; a real accidental
leak won't carry the marker.

Check 10 has the same per-line escape hatch for skills that legitimately
document Windows paths (e.g. a skill about a Windows CLI): tag the line with
`allowlist windows-path`, e.g. `C:\Users\... <!-- allowlist windows-path -->`.
A genuine portability bug won't carry the marker.

## Behavioral test (live)

`check-skill.sh` proves a skill is *well-formed*; it never runs the skill. For a
script-/tool-backed skill, the question that matters most — does the documented
command actually reach the system and return sane output today? — is only answered
by executing its `tests.md` scenarios live.

```bash
python eval/behavioral-check.py <skill-dir> --cwd <checkout>   # run the tests.md commands live
python eval/behavioral-check.py <skill-dir> --dry-run          # list commands, run nothing
```

It extracts the runnable commands from `tests.md` (fenced bash blocks and inline
`python …` spans), runs each from `--cwd`, and reports each command's exit code, an
output preview, and sanity flags (`TIMEOUT` / `TRACEBACK` / `EMPTY-STDOUT`). Point
`--cwd` at the checkout where the skill's `scripts/` and `.env` live — a git
worktree won't carry the gitignored `.env`. It does the deterministic part only:
**you compare each output to the scenario's `Expected` prose** to decide PASS/FAIL.

**Cheap and real** (~0 tokens, seconds) — the counterpart to the heavy `tune-skill`
composite, which never touches live infrastructure. Full procedure, applicability,
and safety in [behavioral-check.md](behavioral-check.md).

## Value-add test (dynamic)

`check-skill.sh` proves a skill is *well-formed*; it cannot prove the skill is
*worth invoking*. A skill can be perfectly structured (and score ~0.97 on the
improving-skills composite) yet **lose a blind head-to-head to the cold model** —
its scaffolding burying the rigor that was supposed to be its value. The
value-add test catches that.

```bash
eval/value-add-test.sh <skill-dir>            # preflight: seeds + A/B order + protocol
eval/value-add-test.sh --tally <results.tsv>  # lift + PASS/CONCERN/FAIL
```

It runs the skill's output vs a no-skill baseline on representative seeds (the
positive prompts from `test-prompts.md`), blind-judged on rigor / evidence /
actionability / risk-awareness / signal-to-noise. The script does only the
deterministic parts (seed selection, A/B randomization, lift arithmetic) — the
generation and judging are agent-driven. The full protocol, applicability limits
(it's for generative/judgment skills, not deterministic/safety/format ones), and
interpretation live in [value-add-test.md](value-add-test.md).

**This is a milestone / pre-promotion check, not a per-edit one** — it spawns
`~4N` sub-agents per run. Required to run-and-record before promoting a
generative skill; advisory (not a hard gate), since only the deterministic
`check-skill.sh` hard-blocks.

## Trigger-accuracy test (empirical)

`check-skill.sh` lints a description's *shape* (Checks 7/7b/8); the improving-skills
composite scores triggering by **simulation** (a sub-agent predicts whether it would
fire). Neither observes the *real* decision. `trigger-accuracy` does: it spawns a real
Agent sub-agent per fixture prompt and detects whether the target `Skill` actually
fired, in this session's full skill environment — your repo skills **plus** every
global plugin — so collisions are real signal, named in `other_skill_fired`.

```bash
python eval/trigger-accuracy.py parse-prompts <skill-dir>/test-prompts.md         # parse fixtures
python eval/trigger-accuracy.py score runs.json --skill <name> --skill-md <path>  # detect + aggregate
```

The script does the deterministic parts (parse, transcript detection, aggregation);
the orchestrator spawns the sub-agents per the full procedure in
[trigger-accuracy.md](trigger-accuracy.md). `aggregate.trigger` = accuracy, a drop-in
for the improving-skills composite. **Milestone, not per-edit** — it spawns
`N_prompts × runs` sub-agents. No skill isolation is possible (sub-agents inherit the
live registry), so it measures real-environment triggering, which is the useful
question in a crowded skill setup.

## Why a shell script

The harness is intentionally trivial to read and modify. It is a playground
tool, not a CI gate. If you want a CI gate, port it to your language of choice
and lock down the WARNs you care about as FAILs.

## Notes

- The harness expects `<dir>/SKILL.md` at the given path. Files matching
  `*.template` (e.g. `templates/skill-skeleton/SKILL.md.template`) are
  intentionally skipped — pointing the harness at a template-only directory
  errors with `SKILL.md not found`, which is the intended behavior (templates
  are scaffolds, not skills).

## Dependencies

- `bash`, `awk`, `grep`, `sed`, `wc`, `find`, `diff`, `mktemp` (Check 21 also
  shells out to `git rev-parse` as a repo-root fallback, but degrades gracefully
  without it)
- `python3` with `pyyaml` (used only for the YAML parse check)
- `trigger-accuracy` additionally needs a running Claude Code session (it spawns Agent
  sub-agents); `trigger-accuracy.py` itself is pure-stdlib `python3`, no `pyyaml`.
- `behavioral-check.py` is pure-stdlib `python3`; it additionally needs whatever the
  target skill's commands need at run time (its interpreter, `scripts/`, and `.env`
  credentials) — point `--cwd` at the checkout that has them.
