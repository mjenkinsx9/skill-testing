# Promotion Record — automating-browsers

**Skill**: automating-browsers
**Promoted**: 2026-05-31 (recorded retroactively — the skill was already live;
this file captures the gates as actually verified)

> Mechanical gates (harness 0 FAIL, tests.md scenarios + freshness, naming,
> sidecar linking) are machine-enforced by `eval/check-skill.sh` and CI and
> are not re-attested here.

## Real-work evidence

All six tests.md scenarios are drawn from live runs: Anthropic docs heading
extraction, the-internet.herokuapp.com login/logout flows, an Acumatica ERP
session (iframes, unlabeled icons, custom tab strips), and a Pure Storage SAN
dashboard (virtualized tables). The patterns the skill teaches — ref drift,
virtualized-table gaps, link-click non-redirects — come directly from those
runs. Environment identifiers in the enterprise scenarios were anonymized for
publication (2026-06-10); the runs were real. The golden path and the-internet
login flow were re-verified live 2026-05-31.

## Security review

No harness security smells, no secrets. Credential handling is constrained by
the SKILL.md "Credentials" section: never hardcode/echo/commit credentials,
treat user-supplied credentials as session-only, stop at unexpected login
walls. Dual-use browser-driving tool, scoped to interactive, user-authorized
use.

## Value-add verdict

- [x] **Value-add verdict**: **N/A (non-generative)** — a procedural/tool-driving
      skill (snapshot→act→verify against a live browser), not a generative/judgment
      skill, so the "beats the cold model" frame doesn't apply (per CLAUDE.md). Its
      value is the Playwright-MCP tool-selection table and hard-won pitfalls —
      mechanical aids, not judgment to A/B test.

## Strip list

None — all sidecars (`references/reference.md`, `tests.md`, `test-prompts.md`)
are production.

## Live test

2026-05-31 — heading extraction and the-internet login flow exercised via
Playwright MCP in a live session; skill auto-loads and appears in the
available-skills list with the repo as cwd.
