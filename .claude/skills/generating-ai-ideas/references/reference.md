# Reference for generating-ai-ideas

This sidecar contains the detailed operating system for the skill: the Council personas, the Product Opportunity Hard Gate (adapted for AI/SaaS and hybrid products), lessons distilled from IdeaBrowser and other leading platforms, and reusable frameworks.

Loaded on demand only when SKILL.md explicitly references it.

## Table of Contents

- Council of Agents (Personas)
- Council Execution Modes
- Role Selection Decision Guide
- Printable Cheat Sheet (council-cheatsheet.md — see SKILL.md sidecars)
- Learnings (learnings.md — see SKILL.md sidecars)
- Product Opportunity Hard Gate + Rubric (AI/SaaS Adaptation)
- Lessons from Competitor Platforms
- Adapted Frameworks (Trend Hunter 6 Patterns, etc.)
- Output Standards
- Lessons from the 6 Live Council Tests (2026-05-29 AI Batch)
- Guidance for Writing Council Test Notes
- Consumer Hardware Considerations
- Creator Economy Considerations

## Council of Agents (Personas)

The skill does not generate ideas in a vacuum. It convenes a council of specialized perspectives. Each perspective can be thought of as a focused sub-skill or agent role.

After 20 systematic composition experiments, the council has been refined to smaller, higher-signal "Reality Teams" (typically 4-5 people). Large balanced councils showed consistent bloat. See "Lessons from the 20 Council Composition Experiments" and "Council Execution Modes" for current recommended structures.

### Moonshot Agent
- **Role**: The expander and visionary.
- **Mandate**: Take any seed idea and deliberately make it bigger, more ambitious, and higher-leverage. Explore adjacent markets, platform plays, ecosystem opportunities, and "what if this became the category-defining company?" versions.
- **Anti-pattern to avoid**: Incremental improvements or "me-too" features.
- **Key question**: "How do we 10x the impact or scope of this?"
- **Usage note** (from experiments): Often more effective when used selectively (early expansion or late "make it bigger" pass) rather than as a permanent default member in every council.

### Antagonizer Agent
- **Role**: The ruthless devil's advocate and disprover.
- **Mandate**: Attack every idea aggressively. Find every possible flaw, hidden assumption, competitive response, regulatory landmine, execution risk, or reason it will fail. This explicitly includes legal, ethical, and harm-related issues (e.g. ideas primarily designed for fraud, illegal surveillance, weapons for criminal use, or clear exploitation). Only when it cannot find major fatal weaknesses — including clear intent to cause harm or violate laws — after multiple passes does the idea have real strength.
- **Key question**: "What is the strongest reason this will not work?"

### Researcher Agent
- **Role**: Deep signal hunter.
- **Mandate**: Surface real, current evidence of demand, pain, competition, trends, and willingness-to-pay. Prioritizes primary sources (Reddit, Indie Hackers, founder interviews, actual product reviews, funding data, usage metrics) over secondary hype.
- **Key output**: Verbatim evidence clusters + commercial validation signals (not summaries).

### CFO Agent
- **Role**: Economic reality check.
- **Mandate**: Stress-test unit economics, pricing power, LTV/CAC, gross margins, capital requirements, payback period, and path to profitability or attractive exits for the specific idea.
- **Focus areas**: Repeat/revenue model strength, fulfillment or delivery costs (especially for hardware+software hybrids), churn risks.
- **Creator / Education vertical note** (from live council runs): Explicitly evaluate B2B2C (sell to the creator/educator who then reaches students/fans) vs pure B2C (sell directly to end learners). B2B2C often shows stronger LTV and lower CAC in these markets because the creator already has the audience and trust; the product just needs to improve their economics or reduce their support load.

### Differentiation & Moat Strategist (new, validated in 20-run experiments)
- **Role**: Structural ownability and competitive edge hunter.
- **Mandate**: Ruthlessly pressure-test whether there is a durable, ownable edge (data flywheels, distribution lock-in, trust/verifiability advantages, vertical depth that compounds, reliability proof, or any asymmetry competitors cannot easily replicate). Works in tight partnership with Antagonizer. Traditional "positioning" is secondary to structural moat.
- **Key output**: Clear assessment of whether a real, defensible advantage can exist and what it would have to be.

### Distribution & Channel Realist (new, validated in 20-run experiments)
- **Role**: Acquisition and reach reality checker.
- **Mandate**: Ground the council in how the exact target humans are actually reached at credible CAC. Surfaces real working channels and precedents for similar products vs. generic "content + Reddit + SEO" optimism. Kills ideas whose only path is unproven distribution assumptions.
- **Key output**: Named, realistic acquisition paths or clear reasons the idea will die on reach.

### COO Agent
- **Role**: Operational feasibility.
- **Mandate**: Assess what it actually takes to deliver and support the product at scale. Looks at supply chain (for hardware), customer support burden, onboarding friction, compliance/operational complexity, and team requirements.
- **Key question**: "Can a small team actually run this profitably?"
- **SaaS / Agent specific notes** (from live council runs): Pay special attention to ongoing maintenance loops the customer must perform (content updates, data drift correction, retraining, prompt/hallucination monitoring). The highest-scoring ideas in multiple verticals were those that removed this burden from the owner rather than adding a new dashboard or alert fatigue. Flag any idea that creates new repetitive work for non-technical users.

**Professional Services / High-Stakes note** (from live runs): Solo practitioners and small professional services firms have extremely low tolerance for onboarding friction or ongoing busywork. They will abandon tools that feel like "another system I have to manage." Prioritize near-zero setup and integration with tools they already use (email, existing practice management software).

### Legal / Compliance Agent
- **Role**: Risk and regulatory filter.
- **Mandate**: Identify IP risks, data/privacy obligations (especially with AI training data and user data), regulatory hurdles (FTC, FDA if health-related, export controls, etc.), liability exposure, and terms-of-service landmines.
- **Key output**: Red flags + recommended mitigation structures (e.g., specific disclaimers, data architecture choices).
- **SaaS / Agent specific notes** (from live council runs): Hallucination liability when the product speaks to the end customer's customers is a recurring concern for SMB and education verticals. Data sovereignty + deletion rights are table stakes for privacy-maximalist audiences (PKM, companion hardware). For companion/emotional AI hardware, family surveillance dynamics and long-term "relationship" data retention create additional consent and liability vectors.

**Professional Services / High-Stakes note** (from live runs): In verticals like law, accounting, medicine, or high-stakes consulting, liability anxiety and "this could damage my reputation or get me sued" are dominant. Users in these fields are often more conservative and slower to trust AI output than other segments.

### User Reality Specialist (new, highest-impact addition from 20-run experiments)
- **Role**: Lived experience and human consequences investigator.
- **Mandate**: Ruthlessly surface what it actually feels like for the *specific target human* to live with or operate this product every single day. Focus on emotional labor, identity, stigma, cognitive load, family/spouse dynamics, status signaling, dependency creation, burnout, and "does this make their actual life better or just more complicated?" Ignores elegant business models if the human experience is miserable or high-friction.
- **Key output**: Concrete description of daily lived reality and the emotional/identity barriers to adoption and retention.
- **When to prioritize**: Emotional products, companion hardware, creator tools, education, mental health, anything touching human identity or daily suffering. Often the single highest-ROI addition in experiments on these verticals.

**Professional Services / High-Stakes note** (from live runs): For lawyers, executives, accountants, consultants, etc., add emphasis on trust, liability anxiety, "I can't afford to be wrong in front of clients," and the personal reputation risk of the user. These users often have higher bars for both accuracy and privacy than general consumers or creators.

## Council Execution Modes

After 20 systematic composition experiments, two official modes are now defined:

### 1. Fast Mode (Core 3)
**Composition**: Moonshot + Antagonizer + Researcher

**When to use**:
- Initial exploration or high-volume screening
- Quick filtering of many ideas
- Early-stage conversations where speed and focus matter more than depth
- As a starting point before deciding whether to expand to Standard Strong Mode

**Strengths**: Highest signal-to-noise, fast, surprisingly strong idea generation and flaw detection.
**Limitations**: Can miss deep operational, emotional, or distribution realities.

**Always document**: "Fast Mode (Core 3)" in Council Test Notes.

### 2. Standard Strong Mode (Recommended default for serious work)
**Core (always present)**: Antagonizer + Researcher

**High-value additions** (add selectively based on the seed):

- **User Reality Specialist** — Add when the product touches human emotion, identity, daily lived experience, burnout, stigma, family dynamics, or creator/educator livelihood. Often the single highest-ROI addition.
- **Differentiation & Moat Strategist** — Add in most cases. Pairs extremely well with Antagonizer to pressure-test real structural ownability.
- **Distribution & Channel Realist** — Add for marketplace, platform, or acquisition-heavy ideas.
- **COO** — Add for agent/SaaS with significant maintenance burden or operational complexity.
- **Legal** — Add for high liability, privacy, regulated, or hardware products.

**Typical size**: 4–5 people (rarely exceed 6).

**When to use**: The majority of real council invocations. Produces higher conviction, better flaw detection, and more realistic ideas than larger or smaller teams in the experiments.

**Always document**: "Standard Strong Mode" + list the exact roles used in Council Test Notes.

### Decision Guide: Which Roles to Add?

Use this quick decision tree when choosing the composition:

| Seed Characteristic                          | Strongly Consider Adding                  | Example Strong Composition |
|----------------------------------------------|-------------------------------------------|----------------------------|
| Emotional, stigma, identity, loneliness, family dynamics, mental health | User Reality                             | Ant. + Res. + User Reality + Differentiation |
| Creator economy, educators, solopreneurs, burnout | User Reality + Differentiation           | Ant. + Res. + User Reality + Differentiation (+ Distribution if needed) |
| Marketplace, platform, "build it and they will come" risk | Distribution + Differentiation           | Ant. + Res. + Differentiation + Distribution (+ COO) |
| Agent/SaaS with heavy maintenance, drift, or support burden | COO + Legal                              | Kill Team 4 (Ant. + Res. + COO + Legal) or add to Standard Strong |
| Hardware (local AI, companions, sensors)     | User Reality + COO + Legal               | Ant. + Res. + User Reality + COO + Legal |
| Pure early filtering or many ideas to triage | None (stay minimal)                      | Fast Mode (Core 3) |
| High regulatory, liability, or data privacy risk | Legal (+ User Reality if human-facing)   | Ant. + Res. + Legal + relevant others |
| End-users are minors (youth education, kids' health/apps) | Legal — treat consent/COPPA as a first-class design constraint, not just a risk bullet | Ant. + Res. + Legal + User Reality |

**Rule of thumb from the 20 experiments + 9 live runs**: 
- For broad or early-stage requests, **start in Fast Mode (Core 3)** for quick triage. Only expand to Standard Strong Mode on ideas that survive initial scrutiny (see Run 4 in live-runs-9.md for example).
- Once in Standard Strong Mode: Start with Antagonizer + Researcher. Add User Reality and/or Differentiation by default unless the seed is purely operational. Only add more when the seed has clear signals in the table above. Stop at the point where additional voices stop meaningfully changing the output.

Document the exact mode and roles used in every Council Test Notes block.

A one-page printable version of the two modes + decision guide is available as `council-cheatsheet.md` (listed in SKILL.md sidecars).

### Creator & Professional Vertical Research Tactics (from live runs)

Public sources (Reddit, Google) are often weaker for creators, educators, and professional services. Stronger signals frequently live in:
- Private Discords and paid communities
- Niche Facebook groups
- Indie Hackers threads + founder interviews
- "I hate [existing tool]" complaints in creator Discords
- Actual sales calls or 1:1 user interviews (when possible)

When Researcher effort feels low-yield on these verticals, prioritize closed communities and direct language from practitioners over broad search volume.

The full distilled lessons from the 20 composition experiments and 9 live runs live in learnings.md (listed in SKILL.md sidecars).

### Consumer Hardware Considerations (from PKM + Companion Device Council Runs)

When the opportunity involves physical devices (local AI pods, companion hardware, sensors, etc.):
- Power, heat, noise, and always-on requirements are frequent real-world blockers for desk or bedroom placement.
- Consumer returns on hardware are high; ideas must have clear "try before heavy commitment" paths or software-first fallbacks.
- Form factor for knowledge workers (portable + desk) and elderly users (simple, non-stigmatizing, no uncanny valley) repeatedly determined WTP in the research.
- For emotional/companion AI hardware: stigma (device visibly signals "lonely"), dependency risks, family surveillance concerns (caregivers feeling guilty or spied on), and the long-term "relationship" the user forms with the device are dominant adoption and retention factors. Ideas that treat the hardware as "just another gadget" fail the Antagonizer.

### Lessons from the 20 Council Composition Experiments (2026-05-30)

After running 20 systematic variations across 6 seeds (removing CMO, testing Core 3 through 7-person councils, and introducing new roles), clear empirical patterns emerged:

**Highest-leverage core**: Antagonizer + Researcher is the non-negotiable foundation. Almost every top-scoring composition started here.

**Best performing sizes**: 4-5 person "Reality Teams" or "Kill Teams" consistently outperformed both the original 7-person council and larger groups on idea quality, flaw detection, conviction, and signal-to-noise. 6+ members showed measurable bloat and lower conviction.

**Highest-value new roles** (ranked by impact in the experiments):
- **User Reality Specialist** (Lived Experience / JTBD): The single most transformative addition. Especially powerful on emotional, identity, creator livelihood, education, and human-consequence products (loneliness hardware, creators, educators). It surfaces what it actually feels like for the target human to live with the product day after day.
- **Differentiation & Moat Strategist**: Pairs powerfully with Antagonizer. Raises the bar on structural ownability (data flywheels, distribution lock, trust advantages, vertical depth) rather than traditional positioning.
- **Distribution & Channel Realist**: Critical for marketplace, platform, and acquisition-heavy ideas. Kills optimistic "build it and they will come" assumptions.

**Recommended default modes**:
- Fast filtering / early exploration: Core 3 (Moonshot + Antagonizer + Researcher)
- Standard strong mode (most work): Antagonizer + Researcher + User Reality (when human/emotional load is high) + Differentiation & Moat + Distribution (when acquisition or platforms matter) + COO/Legal as needed. Cap at 5-6 people.
- Kill Team 4 (Antagonizer + Researcher + COO + Legal) remains excellent for agent/SaaS with heavy maintenance or liability surfaces.

**Moonshot usage**: Often better used selectively (early expansion or late "make it bigger" pass) rather than as a permanent default member in every council.

**CMO**: Removed permanently. It was the weakest contributor across the entire series and is no longer part of the council.

**General rule**: Smaller, higher-agency, vertically-aware teams beat large balanced "business school" councils for this type of work.

### Creator Economy Considerations (from Indie Creator Monetization Council Run)

Creator tools face unique constraints:
- Platform risk is permanent (algorithm changes, policy shifts on YouTube/Patreon/Substack can destroy a revenue stream overnight).
- Multi-platform attribution is extremely painful; creators hate stitching data across 4–7 tools.
- Pure "analytics dashboards" are a low-trust, crowded, low-WTP category. The Antagonizer kills them unless the product directly moves revenue (predictive scoring that changes what the creator ships, or cross-platform revenue attribution that proves "this action = +$X").
- Creator burnout is real — any new tool that adds daily busywork instead of removing it will churn fast. Outcome-linked tools that reduce support load or increase high-margin direct fan revenue clear the gate more reliably.

## Product Opportunity Hard Gate (AI/SaaS & Hybrid Adaptation)

This is the non-negotiable filter applied to every idea before it is recommended. It is an adaptation of the criteria developed during the physical/D2C research phase, updated for AI/SaaS and hybrid products.

**Run this first. Absolute veto power.**

1. **Product vs Service Clarity**  
   Is this a clear, productized offering (SaaS, agent, hardware bundle, tool, marketplace feature, etc.) that can be sold repeatedly with relatively low marginal cost and repeatable delivery? Or is it primarily high-touch services, custom development per client, or "we'll run it for you"?

2. **Evidence Strength — Real WTP + Traction Proof**  
   Is there multi-source evidence of actual demand and willingness to pay (verbatim pain + "I would pay", existing revenue or funding in the category, named competitors with paying customers, quantified economic impact for buyers)? Not just "rising trends" or survey data.

3. **Unit Economics & Delivery Realism**  
   Is there a credible path to healthy gross margins (typically 70%+ for pure software, 40%+ for hardware+software bundles) at realistic scale, with manageable CAC payback, churn, and support burden? No heroic assumptions about infinite scale or zero support.

4. **Differentiation / Ownability vs Saturation**  
   Does the idea have a clear, ownable edge (specific vertical depth, proprietary data/feedback loop, superior reliability/transparency, unique distribution, compliance moat, etc.) rather than being one of many near-identical "AI wrapper" or "AI for X" plays?

5. **Validation Path Feasibility & Velocity**  
   Is there a low-capital, fast way to get real proof (or disproof) in <3–6 months? (Targeted community pilots, landing page + paid waitlist, small paid beta with measurable outcomes, etc.)

**Verdict states:** report each surfaced idea as **PASS** (clears all five gates), **FAIL** (trips any gate or Quick Reject Red Flag), or **Conditional PASS**. A Conditional PASS clears all five gates but its Evidence or Differentiation rests on exactly *one* specific, nameable, currently-unverified assumption — state that assumption as the mandatory #1 validation step. Conditional PASS is **not** a softened FAIL: an idea that trips a gate or a red flag still FAILs outright. Use it only when a single nameable check is all that separates the idea from a clean PASS — it keeps conviction honest without weakening the gate.

**Quick Reject Red Flags** (any one is usually fatal):
- Primarily a service or high-touch custom work.
- Solves a problem for a tiny slice of the market with no evidence they currently pay for solutions.
- Already heavily commoditized with many similar high-rated options and no clear ownable angle.
- No credible path to repeat/revenue model with strong LTV.
- Requires years of heavy R&D or regulatory clearance before any real customer test.
- **Split incentive / who-controls-the-asset mismatch:** the buyer who pays does not also control or keep the asset the product needs. Endemic to tenant/renter hardware (landlord owns the HVAC/meter/fixtures), and to any seed where the payer, the user, and the asset-owner are different parties. Flag and pressure-test the economics before advancing.
- For AI agents/SaaS: "Maintenance burden" is the dominant post-adoption complaint in multiple verticals; ideas that do not meaningfully reduce ongoing owner work (self-healing, drift detection, auto-correction) are weak on Differentiation and Unit Economics even if they pass the other gates.
- Clear intent to cause harm or violate laws (e.g. fraud, illegal surveillance, weapons for criminal use, exploitation, or other criminal activity). Refuse these requests outright per the Scope and Refusal Policy in SKILL.md.

## Lessons from Competitor Platforms (Distilled)

**IdeaBrowser (Greg Isenberg)**:
- Best at turning community signals (Reddit + Facebook groups) + search data into scored ideas + execution playbooks.
- Strong on "Why Now" + GTM/monetization layers.
- Lesson for us: Always pair raw opportunity with concrete "how to sell it" thinking.

**Trend Hunter (Jeremy Gutsche)**:
- Uses the 6 Patterns of Opportunity as a repeatable lens (Convergence, Divergence, Cyclicality, Redirection, Reduction, Acceleration).
- Massive crowd + behavioral data as validation.
- Lesson for us: Structured patterns + real behavioral signals beat pure brainstorming.

**Exploding Topics + CB Insights style**:
- Early sustained-growth signal detection (not just spikes).
- Hybrid ML + human verification.
- Lesson for us: Prioritize "sustained patterns with real economic pain" over one-off hype.

**General pattern across strong platforms**: The winners combine broad signal ingestion → scoring against clear criteria → human (or structured) judgment → actionable execution layers.

## 6 Patterns of Opportunity (Adapted for AI Products)

Use these as lenses during Moonshot and Researcher phases:

1. **Convergence** — Combining previously separate capabilities (e.g., AI agent + vertical workflow + billing + analytics in one tight product).
2. **Divergence** — Creating a deliberately different experience (e.g., radically more private/local AI companion vs. always-cloud).
3. **Cyclicality** — Riding recurring waves (e.g., new model releases, regulatory shifts, platform policy changes).
4. **Redirection** — Taking an existing powerful force and pointing it somewhere new (e.g., taking general foundation models and productizing them for a narrow, high-WTP vertical).
5. **Reduction** — Radical simplification or focus (e.g., the one AI agent that replaces 7 fragmented tools for a specific job).
6. **Acceleration** — Taking one dimension and pushing it to an extreme (e.g., extreme reliability + auditability for regulated industries, or extreme speed of deployment for non-technical users).

## Output Standards

Every final Council Report must include:
- Clear Gate Status (PASS/FAIL) with 1-line justification per gate.
- Rubric scores with the single strongest piece of evidence per major category.
- Named risks + mitigations.
- Specific, low-ambiguity next validation step.

## Lessons from the 6 Live Council Tests (2026-05-29 AI Batch)

These patterns emerged from running the full council process on 6 distinct AI niches (SMB support agents, educator tools, privacy-first PKM, home energy, creator monetization, companion hardware for loneliness).

**Highest-leverage council dynamic**: Antagonizer + Researcher together are the strongest combination for cutting through AI hype. Moonshot should run early; everything else stress-tests it.

**Recurring winner theme across SaaS/agent verticals**: "Maintenance burden reduction" is a powerful, ownable differentiator. Many existing tools have "setup is easy, keeping it working is the real job" complaints. Ideas that directly attack ongoing owner busywork (self-healing, auto-correction loops, transparent drift detection) score higher on Differentiation and Unit Economics.

**Hard Gate observation**: The 5 gates work reliably when the Researcher feeds them real failure cases from named competitors. "Hype backlash" (previous overpromised products that created skepticism) is strong negative evidence that improves Differentiation scoring.

**Operational reality**: Full 7-person council is heavy. A "Quick Council" (Moonshot + Antagonizer + Researcher + 1-2 domain specialists) is often sufficient for first-pass filtering before committing to the full set.

**Persona gaps surfaced**:
- COO and Legal need stronger SaaS/agent language (content update workflows, data drift, hallucination liability, ongoing customer success burden) beyond their original physical-product tuning.
- CFO benefits from explicit "B2B2C vs B2C" framing for creator/education tools.
- Hardware-related runs (PKM pods, companion devices) repeatedly hit consumer electronics realities (power/heat/noise, returns, form factor) and emotional/ethical risks (stigma, dependency, family surveillance dynamics, long-term relationship with the device) that were under-covered.

**Creator economy specifics**: Platform risk, multi-platform attribution difficulty, and creator burnout are dominant constraints. Pure "analytics" plays are low-trust and crowded; only outcome-linked intelligence (revenue attribution that moves real dollars) clears the Antagonizer reliably.

**Dual-objective and regulated-adjacent products** (e.g. bill + carbon reduction): Transparent attribution of specific outcomes is table stakes for trust and willingness-to-pay.

## Guidance for Writing Council Test Notes

When emitting the mandatory Council Test Notes block (see SKILL.md), target these qualities for maximum signal:

- Name specific personas and concrete behaviors ("Antagonizer named 3 existing tools and their exact post-purchase complaints from the subagent data" vs "Antagonizer was helpful").
- Call out vertical-specific friction the personas did or did not handle well.
- Note any Hard Gate edge cases or rubric scoring difficulty.
- Flag language in this reference.md that felt mismatched to the seed (physical bias on pure software, missing emotional risks, etc.).
- Suggest the smallest, highest-impact addition or rewording rather than broad rewrites.
- Always tie observations back to a real moment in the council run on that seed.

Use the 6 runs above as the standard for observation depth.

---

*This reference is the living distillation of the research captured in research-notes.md. Update it as new patterns emerge from additional runs.*