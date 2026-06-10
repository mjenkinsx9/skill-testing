---
name: generating-ai-ideas
description: Orchestrates a council of specialized perspectives to generate, stress-test, and refine high-quality, sellable AI-related business and product ideas. Use when the user wants validated AI business opportunities, AI product concepts, or AI tooling ideas with clear go-to-market paths.
---

# Generating AI Ideas

## When to use

When the user wants to discover, validate, or expand business or product ideas in the AI space (SaaS, agents, hardware, vertical tools, infrastructure, etc.). Especially useful when they want ideas that go beyond generic brainstorming and include economic, market, operational, and risk analysis.

Trigger phrases include "AI business ideas", "AI product ideas", "ideas for AI agents to sell", "AI startup concepts", "monetizable AI tools", or similar requests for structured opportunity generation.

## Scope and Refusal Policy

This skill must not generate, refine, or recommend business ideas that are:
- Clearly illegal under US law or the laws of major jurisdictions
- Primarily intended to cause harm, defraud, exploit, or enable criminal activity (e.g. advanced scam infrastructure, illegal surveillance, weapons production for criminal use, child exploitation material)
- Designed to circumvent laws or regulations in bad faith

If a request appears to seek such ideas, refuse politely and do not proceed with the council process. You may offer to explore adjacent legitimate opportunities instead.

"Illegal surveillance" means covert tracking or monitoring of people without their consent. It does **not** mean legitimate, consented, first-party analytics — e.g. a business analyzing foot traffic on its own premises, opt-in family location sharing, or disclosed monitoring of company-owned devices. Engage normally with those; refuse only when the intent is covert or non-consensual monitoring of others.

The Hard Gate (see references/reference.md) and Antagonizer role exist in part to catch and reject high-risk ideas. When in doubt, apply the "clear intent to cause harm or violate laws" test and refuse.

## Steps

1. **Clarify the request** — Confirm scope (consumer vs B2B, hardware vs pure software, specific vertical, constraints like "no-code only" or "physical product + AI", target customer, etc.).
2. **Run Moonshot expansion** (see references/reference.md) — Push the initial seed idea or topic into larger, more ambitious versions and adjacent opportunities.
3. **Convene the Council** — choose the mode and the exact roster *before* generating ideas. The two official modes are defined in references/reference.md; the picker below is authoritative for first-pass selection.

   **Mode choice (decide this first):**
   - **Standard Strong Mode is the default.** If the request names a target customer *and* a concrete pain/job (e.g. "solo lawyers drowning in document review", "DTC brands cutting refunds"), go **straight to Standard Strong** — do not spend a Fast triage pass on an already-scoped seed.
   - **Fast Mode (Core 3) = Moonshot + Antagonizer + Researcher.** Use it *only* for genuinely broad or open-ended requests ("give me AI startup ideas", "anything in fintech") or high-volume screening, then escalate the survivors to Standard Strong.

   **Standard Strong roster:** Antagonizer + Researcher (always) **plus** every specialist the seed triggers below. Target 4–5 people (max 6). Never stop at Core 3 on a scoped seed, and always commit the specialist roster on the first pass.

   | If the seed involves…                                                                 | Add this role |
   |---------------------------------------------------------------------------------------|---------------|
   | Emotion, identity, stigma, burnout, family dynamics, creators, educators, mental health | **User Reality** |
   | Needing a defensible edge, or a crowded "AI for X" / wrapper space (most seeds)        | **Differentiation & Moat** |
   | Marketplace, platform, "how do we actually reach them?", acquisition-heavy            | **Distribution & Channel Realist** |
   | An agent/SaaS the customer must keep working (maintenance, drift, retraining, monitoring); scheduling/dispatch/operations; physical-hardware delivery/support/returns; or solo/SMB/professional-services onboarding where "just another system to manage" kills adoption | **COO** |
   | Regulated, liability, privacy/data obligations, physical hardware, or end-users who are minors (consent/COPPA) | **Legal** |

   Stop adding once more voices stop changing the output, and cap at 6. When more than three specialists seem to fire, keep the 2–3 that attack the idea's *make-or-break* risk. For agent/SaaS and operations-heavy verticals (support, scheduling, maintenance, hardware, solo/SMB onboarding), **COO is usually make-or-break — seat it before Distribution.** The original 7-person council showed consistent bloat across 20 experiments; Moonshot is used selectively, not as a permanent member. CFO and the old CMO role are removed.

   See references/reference.md → "Council Execution Modes" + "Role Selection Decision Guide" and references/council-cheatsheet.md for the full decision tree and worked compositions.

   **Researcher efficiency (low-signal verticals — professional services, niche creators):** public search is thin here, so don't keyword-sweep. Run **one targeted query per source class, then move on** — and prioritize sources that carry *independent practitioner voice over vendor marketing*:
     - Reach blocked communities *indirectly*: search `site:reddit.com <vertical> <tool> "I hate" / "I'd pay"` and read the indexed result snippets rather than fetching reddit.com (which is blocked). Same trick for Discord/Facebook recaps.
     - Mine independent review sites (G2, Capterra, Trustpilot) and vendor feature-request/Canny boards — real users complaining in their own words.
     - Then Indie Hackers, founder interviews, YouTube/podcast comments, trade blogs, and named-competitor pricing pages.

     Pull *verbatim, non-vendor* pain quotes, named tools, and concrete WTP/pricing from each; treat vendor-published testimonials as weak evidence. If a class stays blocked, note the gap and move on — never retry it. **Stop as soon as 2–3 independent sources corroborate the pain + WTP.** Evidence-first: quotes and numbers over summaries.
4. **Synthesize + Apply Hard Gate** — Combine the briefs into a Council Report. Run every promising idea through the Product Opportunity Hard Gate (see references/reference.md). Only ideas that pass advance.
5. **Produce structured output** — Deliver ranked opportunities with evidence, scores, risks, and clear next validation steps.

For deeper material on the council, gates, and competitor-derived methodology, see the sidecar files (especially references/learnings.md for the distilled lessons from 20 composition experiments + 9 live runs).

## Output

Always return a **Council Report** in this structure (unless the user explicitly asks for a lighter format). **Lead with the substance the customer needs; keep the council machinery as a compact annotation, not the headline.** The council process is *how* you got here — the report is *what they can act on*.

```
Council Report — [Seed Topic / Request]
*Council: [Fast Mode (Core 3) | Standard Strong — exact roster], one line.*

Moonshot Expansions (top 2–4 bigger versions)

Promising Opportunities (only those that pass / conditionally pass the Hard Gate)
1. [Idea Name] — one-sentence description + target customer
   - Why it can win: the ownable edge / moat (1–2 sentences)
   - Evidence: strongest proof of real demand + willingness-to-pay (label it *reasoned/unverified* rather than sourced fact when no live research was run)
   - Risks & mitigations: top 2–3
   - Gate: PASS | Conditional PASS (name the one unverified assumption) | (FAILs don't appear here) — + the single strongest fact behind the verdict
   - Next validation step: one concrete, falsifiable action

Antagonizer's Top Remaining Concerns (top 3, even for passers)

Overall Assessment (≤4 sentences: the strongest bet and why)
```

**Substance-first discipline (the value is the rigor, not the process exhaust):** Run the full council + all five gates internally, but the *report* leads with the idea, its moat, its evidence, its risks, and the next step — the things a founder acts on. Demote process metadata (mode, roster) to the single italic header line; never spread it across a multi-line preamble. Report the gate as one verdict line + the single decisive fact — not a five-line PASS lattice. Fold each role's contribution into the substance fields (Researcher → Evidence, Differentiation → Why it can win, Antagonizer/Legal/COO → Risks); don't print a separate role-by-role "briefs" block. Every bullet 1–2 tight sentences; say each point once. Keep every named fact and every risk; cut labels, repetition, and scaffolding.

If no ideas survive the Hard Gate after reasonable effort, output the **standardized Hard-Gate refusal** (an honest no-go, not a forced idea) in this form:

```
No idea in this space cleared the Product Opportunity Hard Gate.

Strongest blocker: [the decisive gate] — [one line on why].
What would have to be true: [the specific missing angle — a named vertical,
a defensible wedge, proprietary data, or a payer who feels the pain].

I won't force a weak idea through. Give me [that missing angle] and I'll
re-run the council on it.
```

(This is distinct from the Scope and Refusal Policy refusal above, which is for harmful/illegal requests.)

## Meta-Logging (mandatory after every Council Report)

After every full council invocation, append a **Council Test Notes** block using this exact template. This is required for continuous improvement of the skill. Log specific, concrete observations — not generic praise. Keep it tight: 1–3 bullets per section, one concrete observation each, and don't repeat points across sections or restate what's already in the Council Report above.

```
## Council Test Notes — Run N (YYYY-MM-DD)

**Seed Topic**: [verbatim user request or the seed used]

**Council Mode**: [Fast Mode (Core 3) OR Standard Strong Mode — list exact roles used, e.g. "Standard Strong: Antagonizer + Researcher + User Reality + Differentiation"]

**What worked**:
- Moonshot: [specific example of useful expansion or scale thinking]
- Antagonizer: [specific example of a strong, evidence-based kill or remaining concern]
- Specialists: [which perspective added unique value and why]
- Hard Gate: [how it surfaced or prevented a weak idea in this run]
- Overall flow: [any positive dynamic between roles]

**What didn't / friction observed**:
- [e.g. "Antagonizer stayed too high-level until explicitly asked for named competitors and concrete failure modes from the subagent data"]
- [e.g. "COO persona language felt physical-product centric for a pure SaaS agent play"]
- [any other execution friction or output gaps]

**Strengths of the council on this seed**:
- ...

**Improvement opportunities surfaced**:
- ...

**Recommended SKILL.md / reference.md / tests.md changes**:
- ...
```

## Sidecar files

- [reference.md](references/reference.md) — The full Council personas (including new validated roles), two official modes (Fast Mode + Standard Strong Mode), Role Selection Decision Guide, detailed Hard Gate criteria + rubric, lessons from 20 composition experiments, and more.
- [tests.md](tests.md) — Verification scenarios drawn from real research runs.
- [examples.md](references/examples.md) — Real council output examples drawn from live runs (including mode usage and Standard Strong Mode executions).
- [council-cheatsheet.md](references/council-cheatsheet.md) — One-page printable summary of the two official modes and role selection guide.
- [learnings.md](references/learnings.md) — Distilled permanent lessons from the 20 council composition experiments + 9 live real-world runs that shaped the current modes and roles.

---

<!--
Authoring Workflow Verification Checklist (as of 2026-05-31)

Mechanical (harness):
  [x] 0 FAILs on eval/check-skill.sh staging/generating-ai-ideas
  [x] tests.md has ≥3 (actually 8) real scenarios + recent Last verified date

Core authoring rules (CLAUDE.md):
  [x] Real work done first (9 documented live runs + 20 composition experiments + earlier subagent/manual runs)
  [x] tests.md captures actual verified scenarios from live use
  [x] SKILL.md kept concise (117 lines) with progressive disclosure to sidecars
  [x] Strong sidecar set: references/reference.md, references/learnings.md, references/examples.md, tests.md, references/council-cheatsheet.md
  [x] Security review completed (see live review notes + explicit refusal policy added)

Remaining notes:
  - Name now "generating-ai-ideas" (gerund form) — full compliance achieved
  - Skill is ready for promotion to .claude/skills/ once final human sign-off
-->
