# 06 — AI Leadership & Org Design

> Directors are not hired for what they can build. They are hired for the decisions they make
> when the answer isn't obvious and the cost of being wrong is high. This is the judgment layer.
> You have 22 years of it in platform engineering — this file translates it into AI.

---

## 1. The decisions you will be hired to make

### 1.1 Build vs Buy vs Fine-tune vs API

The question every AI Director is asked. Have a framework, not an opinion.

| Dimension | Favours API / buy | Favours self-host / build |
|---|---|---|
| Differentiation | It's not your product's edge | It *is* the product |
| Volume | Low or spiky | High and sustained (utilization matters — see [04](04_AI_Platform_And_Inference_Economics.md) §5) |
| Latency | Tolerant | Hard real-time or edge |
| Data sensitivity | Acceptable to send out | Regulated, sovereign, or contractually restricted |
| Capability needed | Frontier | Narrow task a small model can do |
| Team | No ML platform capability | Have or can hire one |
| Time horizon | Need it this quarter | Multi-year platform bet |

**The trap to name explicitly in interviews:** self-hosting business cases usually fail on
utilization, not on unit price. A reserved GPU at 20% utilization loses to per-token API.
Saying this out loud signals that you've actually done the analysis.

**The second trap:** "we'll fine-tune" is very often the wrong first move. The order is almost
always: better prompting → retrieval → routing → fine-tune → train. Skipping to fine-tune is a
common expensive mistake and a good thing to have a story about.

### 1.2 Where does AI capability live in the org?

| Model | Structure | Works when | Fails when |
|---|---|---|---|
| **Centralized CoE** | One AI team builds everything | Early, scarce talent, need standards | Becomes a bottleneck; distant from product |
| **Embedded** | AI engineers inside product teams | Mature tooling, many use cases | Duplication, inconsistent quality/safety, no cost control |
| **Platform + embedded** (usually right) | Central team owns gateway/evals/tooling/governance; product teams build features on it | Most orgs past the first few use cases | Platform team over-builds and under-serves |
| **Federated with a guild** | Embedded + a cross-cutting practice community | Scaling adoption | Needs real sponsorship or it decays into a meeting |

**The platform team's actual charter** (write this down, you'll be asked to define it):
- The AI gateway: routing, caching, budgets, guardrails, telemetry
- The eval infrastructure and quality bar
- Model lifecycle: selection, versioning, evaluation, deprecation
- Golden paths and reference architectures
- Governance, risk classification, and the approval process — kept *fast* enough to be used
- **Not**: building every feature. If they are, the platform has failed.

### 1.3 Model portfolio strategy

You will own a portfolio, not a model:
- **Tiering**: frontier for hard/rare, mid for general, small/edge for high-volume and latency-critical
- **Provider diversity**: leverage and resilience vs operational cost. Have a view and a trigger
  for when you'd change it.
- **Abstraction discipline**: the gateway is your insulation layer; don't let provider SDKs leak
  into application code
- **Deprecation**: providers retire models. Your evals are what make a migration a one-week job
  instead of a quarter. This is a concrete argument for eval investment that CFOs understand.

---

## 2. Measuring AI honestly — your credibility differentiator

The industry is full of inflated AI productivity claims. A Director who measures honestly is
immediately more credible than one with better numbers. You already framed this well in the
AI-era résumé ("measured on real time-to-completion and defect-escape, not perceived speed") —
now build the method behind it.

### 2.1 For AI-assisted engineering productivity

**Don't measure:** lines of code, AI suggestion acceptance rate, "developers report feeling faster."
Perceived speedup is known to diverge from measured speedup — sometimes in the opposite direction.

**Do measure:**
- **Time-to-done** on comparable units of work (cycle time from start to merged/deployed)
- **Defect escape rate** — the cost AI most often hides
- **Change failure rate and MTTR** (DORA metrics still apply, and now matter more)
- **Review burden** — AI generates more code; is review becoming the bottleneck?
- **Rework rate** — how much AI-produced work gets substantially rewritten
- Cost: tokens per merged change

**Method:** differences-in-differences across comparable teams, or staged rollout with a holdout.
Not a before/after on one team — too many confounders. *This is where the causal inference reading
in [03](03_Technical_Syllabus.md) T2.1 pays off directly.*

### 2.2 For AI product features

- Task success rate (the core metric — requires evals)
- Cost per successful task
- Containment / deflection rate where relevant
- Human escalation rate and its trend
- Quality regression detection latency — how fast do you notice the model got worse?

### 2.3 For AI cost programs

- Absolute spend, and spend per unit of business value
- **The counterfactual** — what spend would have been without intervention. State your method.
- Quality-held-constant proof: cost went down *and here is the eval showing quality didn't*

---

## 3. Driving adoption — the part that's actually hard

You are already doing this at Stellantis (the AI career path you built for the SDP team, and the
enablement article you shared with Stephan). Formalize it.

**The adoption curve in an engineering org:**

1. **Enthusiasts (10%)** — already using it, often unsafely. *Channel them; make them the guild.*
2. **Pragmatists (60%)** — will adopt if it demonstrably saves them time on their actual work.
   *Need golden paths, not evangelism.*
3. **Skeptics (25%)** — often your best engineers, and often right about the failure modes.
   *Recruit them to define the quality bar rather than arguing with them.*
4. **Refusers (5%)** — leave them. Don't build policy around them.

**What actually works:**
- Remove friction first — credits/licences available without a request queue. *You are living the
  counter-example: from the transcript, you burn 2000 credits in a day or two and then halt. If
  that's true for you, it's crippling your whole team's adoption.* Make the budget case with the
  throughput math, and get the allocation raised.
- Pair AI tooling with a **quality gate**, not a mandate. Adoption without evals creates a mess
  that discredits the whole program.
- Publish real internal case studies with real numbers, including the failures.
- Make the career path explicit — engineers adopt what they believe they're rewarded for.
  (You've already built this. It's a genuinely strong Director-level artifact — write it up.)

**What doesn't work:** adoption mandates, seat-count targets as a KPI, executive demos.

---

## 4. Hiring and evaluating AI talent when you aren't an ML researcher

You will have to hire people who know things you don't. This is a normal senior-leadership problem
and you've done it before — but AI has specific pitfalls.

**Roles to distinguish:**
- **AI/ML Engineer** — builds AI features; systems skills + model intuition. The most common hire.
- **ML Platform / Inference Engineer** — serving, GPUs, performance. Rare and expensive. *Your kind of person.*
- **Research Engineer / Scientist** — training and post-training. Only hire if you genuinely train models.
- **AI Quality / Eval Engineer** — an emerging, under-hired role and a strong signal of maturity.
- **AI Product Manager** — owns the use case, the quality bar, and the cost envelope.

**Screening signals that work when you can't out-depth the candidate:**
- Ask about a *failure*. "Tell me about an AI system you shipped that didn't work. What was the
  root cause?" Strong candidates have specific, unflattering, technical answers.
- Ask how they knew it was working. Weak candidates describe a demo; strong ones describe an eval.
- Ask about cost. Most candidates have never thought about it. The ones who have are senior.
- Ask what they'd *not* use AI for. Judgment, not enthusiasm.

**The anti-pattern to avoid:** hiring a researcher to solve an engineering problem. Most enterprise
AI failure is systems failure, not modelling failure.

---

## 5. AI program governance — fast enough to be used

The failure mode is a review board so slow that teams route around it.

**A workable stage-gate:**

| Gate | Question | Artifact | Who |
|---|---|---|---|
| **G0 — Intake** | Is this even an AI problem? What's the risk tier? | One-pager, risk classification | Product + AI platform |
| **G1 — Feasibility** | Does a prototype clear the quality bar? | Eval set + baseline results | Team |
| **G2 — Production readiness** | Guardrails, cost envelope, rollback, monitoring? | Eval suite in CI, runbook, budget | Platform + security |
| **G3 — Scale** | Is it working in the field? Cost per success? | Live dashboards, incident record | Director level |

Risk-tier the gates: a low-risk internal tool should clear G0–G2 in days. A high-risk customer- or
vehicle-facing system gets the full treatment. **One process for everything is how governance dies.**

---

## 6. The executive conversation

Directors translate. Practise these until they're fluent:

| They ask | They mean | Answer with |
|---|---|---|
| "Are we behind on AI?" | Am I exposed? | A capability map + the two highest-value gaps, not a tool inventory |
| "What's the ROI?" | Can I defend this in the budget meeting? | Cost per successful task, the counterfactual, and the method |
| "Can't we just use ChatGPT?" | Why do we need a platform? | The three things a platform gives you: cost control, quality proof, and not being on the front page |
| "Is it safe?" | What's my liability? | Risk tiering, guardrail architecture, the human-accountability boundary, regulatory posture |
| "How many people do you need?" | What's the headcount ask? | Capability-based, not headcount-based: what you'll own and what it takes |

**Your specific advantage:** you already got an AI proposal through SVP and CTO review. That is
the skill. Analyse what worked in that process and be able to describe it — it's one of the
strongest Director-level stories you have.

---

## 7. Your Stellantis-specific plays (next 12 months)

Directly from the constraints in your manager conversation:

- [ ] **Convert the AI enablement article into a formal SDP-wide operating model.** Stephan
      explicitly asked for input on it and wants to "adjust the direction for the team." Take that
      opening — an org-wide AI operating model authored by you is exactly the "impact beyond
      headcount" evidence he said the Director case needs.
- [ ] **Make the AI budget ask concrete.** Stephan said AI budget is "not on the count" — i.e. it
      may be fundable outside the flat scenario. Put a number on it with a throughput model:
      *X credits → Y engineer-hours released → Z delivery capacity under a frozen headcount.*
      That's the argument that wins in a flat-budget year.
- [ ] **Own an AI capability, not a migration program.** Note Stephan's warning about the GSDP
      ownership trap — that if you share the dossier you get made the owner of a program-management
      job you don't want. The inverse is also true: deliberately *claim* ownership of the AI
      platform capability. It's the perimeter growth he told you the Director case requires, and
      it's the one that compounds into your external market value too.
- [ ] **Instrument and publish the results internally.** Cost-per-success, defect escape, cycle
      time. Real numbers, honestly measured, quarterly.
