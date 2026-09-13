# 09 — Portfolio Projects

> **Without these, this roadmap is just reading.** At Director+ the market does not verify
> certificates; it looks for evidence that you have done the thinking. Six artifacts, sequenced.
> Each one should be publishable (sanitized), defensible under 30 minutes of questioning, and
> tied to your domain rather than generic.

**The rule for every project:** publish the *failure analysis*, not just the working demo. Anyone
can show a demo. A written account of what broke, why, what it cost, and what you'd do differently
is what a senior artifact looks like — and it's what convinces an interviewer you actually ran it.

---

## Project 1 — Agentic Incident Triage over Telemetry
**Phase 1, Month 3 · ~25h · Difficulty: moderate**

**What:** An agent that ingests logs, traces and metrics for an incident and produces a ranked
root-cause hypothesis with supporting evidence and a suggested remediation.

**Why this one first:** it uses data you understand deeply (ELK/Prometheus-shaped observability),
solves a problem you actually have, and forces you through the full agent stack — tool calling,
retrieval over unstructured data, multi-step reasoning, and cost control.

**Build:**
- Tools: log search, metric query, trace fetch, deployment-history lookup, runbook retrieval
- The agent loop with a bounded step budget and a hard cost cap
- Structured output: hypothesis, confidence, evidence chain, suggested action
- **Never auto-remediate.** Propose only. Make the human-approval boundary explicit and document why.

**Measure:** hypothesis accuracy against historical incidents with known causes, time-to-hypothesis
vs. human baseline, cost per incident, false-confidence rate.

**Publish:** *"I built an incident triage agent. Here's what it got right, and the three ways it
confidently got things wrong."*

**Ties to:** [03](03_Technical_Syllabus.md) T1.5, and directly to the AI-driven RCA line already on
your Stellantis roadmap — this makes that bullet real.

---

## Project 2 — Eval Harness for a Production-Shaped AI System
**Phase 2, Month 4 · ~25h · Difficulty: moderate · Highest ROI per hour in the list**

**What:** A complete evaluation framework for Project 1 — golden dataset, metrics, LLM-as-judge
with calibration, regression suite in CI, and a quality dashboard.

**Why:** evals are the single clearest signal that separates people who have run AI in production
from people who have prototyped. Very few engineering leaders can talk about this credibly. It is
also the foundation that makes Projects 3 and 5 measurable.

**Build:**
- A golden dataset of 100–200 real cases, stratified by difficulty, including adversarial cases
- Multiple metric types: exact-match where possible, rubric-based judging where not
- **Judge calibration** — measure agreement between your LLM judge and your own human labels.
  Report the agreement number. This is the detail that proves rigour.
- CI integration with a hard quality gate that can block a merge
- Drift detection: re-run the suite on a schedule and alert on regression

**Publish:** *"Evals are the unit tests of AI — and most teams don't have any."* Include your judge
calibration data and the biases you found.

**This is the artifact to lead with in interviews.** It answers "how do you know it's working?" —
the question most candidates fail.

---

## Project 3 — Hybrid Edge-Cloud Inference Router ⭐ **SIGNATURE PROJECT**
**Phase 3, Months 7–9 · ~50h · Difficulty: high**

**What:** A small quantized model running locally, with a confidence-based escalation policy to a
frontier cloud model, and a full three-axis characterization of the tradeoff surface.

**Why this is *the* one:** it is the direct descendant of the Hybrid Inference Engine you
architected at Harman. Nobody else applying for these roles has both built the 2015 version and
rebuilt it with 2026 models. It makes your 22 years an asset rather than a length of time.

**Build:**
- Local: a small model (1–8B class), quantized at multiple precisions, measured on real constrained
  hardware — footprint, tokens/sec, memory, and thermal/power behaviour if you can capture it
- Cloud: a frontier model as the escalation target
- **The escalation policy** — this is the intellectual core. Options to compare: token-level
  confidence/entropy, a self-assessment prompt, a small trained verifier, a rules layer. Compare
  at least two and explain the tradeoff.
- Graceful degradation when the network is unavailable
- A sweep across the escalation threshold, producing the **latency × cost × quality surface**

**Measure:** for each threshold — escalation rate, end-to-end p50/p95 latency, cost per 1000
requests, and quality against the Project 2 eval set. Plot the frontier. Identify the knee.

**Then write the architecture note:** how this deploys to a 15M-unit fleet. Model OTA, cohorting,
canary, rollback, version skew, and the regulatory classification of a model update under R156.
**That note is the Director-level half of the project** — the code proves you can build, the note
proves you can lead.

**Publish as:** *"Hybrid Inference, Ten Years On: rebuilding the Harman HIE with 2026 models."*

**Ties to:** [05](05_Edge_And_Physical_AI.md) entirely, [04](04_AI_Platform_And_Inference_Economics.md) §2–3.

---

## Project 4 — Safety-Constrained Agent for Vehicle Operations
**Phase 2, Month 5 · ~30h · Difficulty: high · Rarest artifact in the list**

**What:** A reference architecture (plus a working prototype against a simulator, not a vehicle)
for an agent that can reason about and propose vehicle remote operations, but is *architecturally
incapable* of autonomously executing a safety-relevant command.

**Why:** this is the artifact nobody else can produce. It requires simultaneous fluency in agent
design, AI security, and automotive functional safety. It is the physical embodiment of your
differentiator, and it converts the "AI governance" claim from words into a thing that exists.

**Build:**
- Command classification by risk tier — informational / reversible / safety-relevant / prohibited
- A capability-scoped tool layer: the agent literally does not hold credentials for the top tier
- Mandatory human approval gates with full context presented for the decision
- Full traceability: every proposal, its reasoning, its evidence, its approval or rejection
- **Red team it.** Prompt injection via retrieved content, indirect injection through telemetry
  data, social-engineering the approval step, tool-chaining to escalate privilege. Document every
  attack you tried, including the ones that failed.

**Write:** the control architecture, the residual risk argument, and the mapping to ISO 21434 /
UNECE R155 concepts and EU AI Act human-oversight obligations.

**Publish as:** *"Agents that can touch physical things: a safety architecture."*

**Ties to:** [07](07_Governance_Safety_Regulation.md) §5, [03](03_Technical_Syllabus.md) T1.7.
This one probably also becomes a **conference talk** — it's a distinctive topic with a clear thesis.

---

## Project 5 — The AI Cost Optimization Playbook
**Phase 2, Month 6 · ~20h · Difficulty: low-moderate · Highest interview utility**

**What:** The portable, sanitized articulation of the AI cost-optimization program you are already
leading at Stellantis — as a reusable framework with your own measured examples substituted for
anything confidential.

**Why:** you have SVP and CTO backing on a real AI cost program. That is genuinely strong currency
and right now it is completely invisible outside the company. Every AI Director interview asks
about cost. Almost nobody has a framework; you'd have one with real numbers behind it.

**Build/write:**
1. The measurement layer — how to establish what AI actually costs you, before optimizing.
   Cost per successful task, not cost per token.
2. The lever stack in priority order, with expected savings and effort
   (the table in [04](04_AI_Platform_And_Inference_Economics.md) §3 is your starting skeleton —
   replace the typical ranges with numbers you measured)
3. The quality guardrail — how you prove cost reduction didn't degrade outcomes. Uses Project 2.
4. The counterfactual methodology — how you defend the savings claim to a skeptical CFO
5. The organizational playbook — how you get executive backing for an AI investment in a
   flat-budget year. *You have actually done this; write down how.*

**Back it with your own data:** run the levers on Projects 1 and 3 and report actual before/after
numbers. A framework with measurements beats a framework with assertions.

**Publish as:** *"What AI actually costs: a practitioner's cost model."*

---

## Project 6 — PQC Crypto-Agility Framework for Connected Vehicles
**Phase 3–4, Months 12–14 · ~25h · Difficulty: moderate · Rarest positioning**

**What:** A post-quantum migration assessment and crypto-agility architecture framework for
vehicle PKI, OTA signing and V2C communications.

**Why:** see [08](08_Quantum_And_PQC_Track.md). Deadline-driven, unowned, board-legible, and the
population of people who understand *both* PQC migration and automotive PKI constraints is
extremely small. It also converts your earlier "blockchain for PKI compliance" exploration into a
story about judgment rather than a dated bullet.

**Write:**
1. Threat model: harvest-now-decrypt-later against a 15–20 year asset lifetime
2. Cryptographic inventory methodology for a V2C platform
3. Prioritization framework — what migrates first and why
4. Crypto-agility patterns for constrained devices and immutable verifiers
5. Hybrid transition sequencing
6. Interaction with UNECE R155/R156 — a crypto migration as a software-update-management problem
7. Program shape: phases, dependencies, honest cost drivers

**Publish as:** *"Your 2026 vehicle will still be on the road in 2045. Its cryptography won't be."*

---

## Sequencing and dependencies

```
Month:  1   2   3   4   5   6   7   8   9   10  11  12  13  14
        │       │   │   │   │   │           │           │   │
P1 ─────────────┤   │   │   │   │           │           │   │   Agentic triage
P2 ─────────────────┤   │   │   │           │           │   │   Eval harness  ◄── everything depends on this
P4 ─────────────────────┤   │   │           │           │   │   Safety-constrained agent
P5 ─────────────────────────┤   │           │           │   │   Cost playbook
P3 ─────────────────────────────────────────┤           │   │   ⭐ Hybrid inference router
P6 ─────────────────────────────────────────────────────────┤   PQC framework
```

**Critical path: Project 2.** Evals gate the credibility of Projects 3 and 5 — you cannot claim a
cost or quality result without a measurement method. Do not skip it because it's less fun.

---

## Publishing discipline

- **Where:** a personal blog or GitHub Pages you own (don't rent your archive), cross-posted to
  LinkedIn. LinkedIn is where your target audience — OEM and Tier-1 engineering leaders, recruiters
  for Director roles — actually reads.
- **Cadence:** one substantial piece every 3–4 weeks. Ten to twelve pieces over 18 months.
- **Length:** 1,200–2,500 words. Long enough to show depth, short enough to be read.
- **Tone:** what you did, what the numbers were, what went wrong. No thought-leadership voice.
  The dry, specific, slightly self-critical register is what reads as senior.
- **Confidentiality:** never company data, never internal figures, never architecture that isn't
  public. Frameworks and your own measured experiments only. When in doubt, leave it out —
  the framework is the valuable part anyway.
- **Every piece ends with:** what you'd do differently. That single habit will distinguish your
  writing from 95% of what's published on these topics.
