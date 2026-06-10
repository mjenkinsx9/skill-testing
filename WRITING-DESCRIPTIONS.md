# Writing skill descriptions

The `description:` in a skill's frontmatter is the **primary trigger mechanism**. It is the only
thing — alongside the name — that Claude sees when deciding whether to consult a skill. The body
is loaded *after* that decision. So a perfectly-written body behind a weak description never runs.
This guide is the constructive companion to the rules `eval/check-skill.sh` enforces (Checks 7,
7b, 8) and the "Broadening the description" anti-pattern in `CLAUDE.md`.

## What a description must carry

- **What it does AND when to use it.** Both, in one place — all the "when to use" information
  lives here, never only in the body.
- **Imperative, third person.** "Reviews a diff and… Use when the user asks to…" — not "I can
  review" or "You can use this to". (Check 6 FAILs first/second person.)
- **User intent, not implementation.** Describe what the user is trying to achieve, not the
  mechanics of how the skill works. Users phrase requests by goal, not by your internal steps.

## Enumerate concrete conditions — then generalize, don't list

- **Name ≥2 concrete trigger conditions** (Check 7b). "Use when the user asks to review a PR,
  audit a diff, or check changes before merging" beats "Use when you have code to look at".
- **Generalize to intent categories; do not enumerate an ever-expanding list of exact queries.**
  The description is injected into *every* prompt and competes with every other skill — a
  query-by-query list bloats it and overfits. When triggering misses, widen the *category* of
  intent, not the count of examples. (This is the same anti-overfit principle behind the
  "Broadening… just in case" anti-pattern in `CLAUDE.md`.)

## Pushy ≠ broad (the one tension to get right)

Two pieces of guidance look like they conflict; they don't, because they act on different axes:

- **Be a little pushy** (tone / confidence) — combat *under*-triggering. Claude tends to skip
  skills on tasks it thinks it can handle alone. State plainly when the skill *should* fire,
  including phrasings where the user never names the skill or file type.
- **Stay sharp** (boundary / precision) — combat *over*-triggering. Keep the scope's edges
  concrete so the skill declines near-misses.

A description can — and should — be both. Pushy is about how confidently you assert the trigger;
sharp is about how precisely you draw its boundary. The failure modes are timid-and-narrow ("may
be useful if you happen to have a PDF") and pushy-but-vague ("use this whenever you have anything
to work with"). Aim for **pushy and sharp**.

## Distinctiveness

Your description competes with dozens of others for Claude's attention. Make it immediately
recognizable — lead with the distinctive intent it owns. (In this repo a live probe showed a
generic "review this PR" prompt firing a *different* review skill; an indistinct description loses
these contests silently.)

## Constraints

- ≤1024 chars (hard limit, Check 5; over-limit is truncated). Aim for ~100–200 words.
- ≥40 chars of substance (Check 8) — a one-liner is too vague to drive discovery.
- No angle brackets / XML (Check 5).

## Examples

| | |
|---|---|
| ✗ timid + narrow | `Builds a dashboard for internal data.` |
| ✗ pushy + vague | `Use this whenever the user has any data or numbers to deal with.` |
| ✓ pushy + sharp | `Builds a fast dashboard from internal metrics. Use when the user asks to visualize company data, build a metrics dashboard, or chart internal numbers — even if they don't say "dashboard".` |

For the **negatives** that prove a description is sharp, the valuable cases are *near-misses* —
queries that share keywords but need something else. For a PDF skill, "extract the fields from
this API JSON response" is a good negative (shares "extract", not the PDF context); "write a
fibonacci function" is a useless negative because nothing about it tempts the skill.

## How to check a description

- **Static:** `eval/check-skill.sh <skill-dir>` — Checks 7 (trigger language), 7b (≥2 concrete
  conditions), 8 (substantive length).
- **Empirical:** the trigger-accuracy harness (`eval/trigger-accuracy.md`) measures whether the
  description *actually* fires on positive prompts and declines on negatives — ground truth, not a
  static guess. Pair it with a `test-prompts.md` of positives + near-miss negatives.
