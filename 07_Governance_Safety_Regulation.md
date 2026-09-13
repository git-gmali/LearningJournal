# 07 — Governance, Safety & Regulation

> **This is your cheapest differentiator.** You already operate under automotive cybersecurity and
> functional-safety regimes. Almost no AI Director does. As AI regulation bites, the combination of
> *"understands transformers"* + *"has shipped under a regulator"* becomes a hiring criterion rather
> than a bonus. Six months of focused study here buys more differentiation than two years of model work.
>
> ⚠️ **Verify current status before quoting any timeline.** AI regulation has been amended actively,
> including proposals to adjust the EU AI Act's application dates. Treat dates below as *what to
> check*, not as settled fact.

---

## 1. The landscape in one table

| Framework | Type | Scope | Why it matters to you |
|---|---|---|---|
| **EU AI Act** | Binding law (EU) | All AI placed on the EU market | Extraterritorial. Automotive is squarely in scope. |
| **ISO/IEC 42001** | Certifiable standard | AI management system (org-level) | Becoming a procurement requirement. Certifiable — so it's a credential. |
| **ISO/IEC 23894** | Guidance | AI risk management | The risk-process companion to 42001 |
| **NIST AI RMF 1.0** | Voluntary framework (US) | AI risk management | The US lingua franca; expected knowledge in US roles |
| **ISO/PAS 8800** | Standard | Safety and AI in road vehicles | **The bridge between your world and AI assurance.** Very few people know it. |
| **ISO 26262** | Standard | Automotive functional safety | You work adjacent to it |
| **ISO 21448 (SOTIF)** | Standard | Safety of the intended functionality | Handles hazards from performance limitations — i.e. the *right* frame for ML |
| **ISO/SAE 21434** | Standard | Automotive cybersecurity engineering | ✅ You already have this |
| **UNECE R155 / R156** | Regulation | CSMS / Software Update Management System | ✅ You already have this. R156 is how a *model update* gets classified. |
| **GDPR / India DPDP Act** | Binding law | Personal data | Training data, prompts, logs, telemetry |
| **OWASP LLM Top 10** | Community standard | LLM application security | The practical security checklist |

---

## 2. EU AI Act — what a Director actually needs

### The risk tiers
- **Unacceptable risk** — prohibited outright
- **High risk** — the heavy regime. Two routes in: (a) Annex III use cases, and (b) AI as a safety
  component of a product already covered by EU product-safety legislation — **which is the route
  that catches automotive.**
- **Limited risk** — transparency obligations (disclose AI interaction, label synthetic content)
- **Minimal risk** — no specific obligation

### GPAI (general-purpose AI) obligations
A separate track for foundation-model providers: technical documentation, training-data summaries,
copyright policy, and additional obligations for models with systemic risk. Mostly relevant to you
as a **deployer** — but know it, because it determines what documentation you can demand from your
model vendors. *That's a procurement lever most engineering leaders don't realise they have.*

### High-risk obligations (what changes about how you build)
- Risk management system across the lifecycle
- Data governance — training/validation/test data quality, bias examination
- Technical documentation and automatic logging/traceability
- Transparency to deployers; instructions for use
- **Human oversight designed in** — not bolted on
- Accuracy, robustness, cybersecurity throughout the lifecycle
- Conformity assessment before market placement; post-market monitoring after

> **Notice how much of that is already your day job.** Risk management across a lifecycle, technical
> documentation, traceability, post-market monitoring, cybersecurity — that is ISO 21434 and R155
> with different nouns. **Write that mapping down.** It's your highest-value publishable artifact
> in this whole domain.

### Roles matter
Provider vs deployer vs importer vs distributor carry different obligations. An OEM integrating a
third-party model may become a *provider* if it substantially modifies the system or puts it on
the market under its own name. This is a real trap and a good question to raise in interviews.

### What to actually do
- [ ] Read the risk-tier structure and the high-risk obligations directly (the official text, not
      a vendor blog)
- [ ] **Check the current application timeline** — dates have been subject to amendment proposals
- [ ] Write the ISO 21434/R155 ↔ EU AI Act obligation mapping
- [ ] Build a one-page AI risk-classification decision tree an engineering team can actually use

---

## 3. ISO/IEC 42001 — the credential worth having

An AI Management System standard, structured like ISO 27001 (Plan-Do-Check-Act, Annex controls).
It is **certifiable**, which is why it's appearing in enterprise procurement.

Covers: AI policy, roles and responsibilities, AI impact assessment, lifecycle management, data
management, third-party/supplier management, and continual improvement.

**Why it's the right credential for you** ([10](10_Credentials_And_Resources.md)):
- It's about *governing* AI, which is a Director's job — not implementing models, which isn't
- It's certifiable, so it's legible on a résumé in a way a course completion isn't
- It pairs naturally with your existing 21434 CSMS experience — you already know how to run a
  management system under audit
- It is a direct qualification for Archetype E roles (Head of AI Governance), which are growing
  fast and are chronically under-supplied

**Target: Lead Implementer certification during Months 10–14.**

---

## 4. ISO/PAS 8800 — your bridge document

*Road vehicles — Safety and artificial intelligence.* Addresses the safety of AI within road
vehicles: safety requirements for AI components, data quality, model development, verification,
and the residual-risk argument for systems whose behaviour is learned rather than specified.

**Why this specific document matters for you:** it is the exact intersection of the two things you
know — automotive safety engineering and AI. It is new enough that expertise in it is scarce. It
connects ISO 26262 (systematic failure) and ISO 21448/SOTIF (performance limitations) to the
machine-learning lifecycle.

Read it alongside SOTIF. **SOTIF is the correct conceptual frame for AI safety generally** — the
system does exactly what it was built to do, and that turns out to be unsafe in an unforeseen
situation. That's a hallucinating agent, described in automotive language, fifteen years early.

> **Publishable artifact:** *"What AI safety could learn from SOTIF."* Take a framework the
> automotive industry has used for a decade for handling hazards that arise from performance
> limitations rather than faults, and apply it to LLM systems. Very few people can write this.
> You can. It would travel.

---

## 5. AI security — the practitioner layer

### OWASP Top 10 for LLM Applications
Work through each hands-on against your own system in Phase 2 ([02](02_Master_Roadmap.md) Month 5):

1. **Prompt injection** — direct and indirect. *Unsolved.* Mitigate by architecture (least
   privilege, isolation, human gates), not by filtering.
2. **Sensitive information disclosure** — in outputs, logs, and traces
3. **Supply chain** — model provenance, weights as untrusted artifacts, dependency risk
4. **Data and model poisoning**
5. **Improper output handling** — treating model output as trusted input to another system.
   *The classic pattern behind the worst incidents.*
6. **Excessive agency** — too many permissions, too little oversight. **The category that matters
   most for anything that touches a vehicle.**
7. **System prompt leakage**
8. **Vector and embedding weaknesses** — poisoning the retrieval corpus
9. **Misinformation** and over-reliance
10. **Unbounded consumption** — cost/DoS via expensive queries

### The architectural principle to internalize
**You cannot make the model trustworthy. You make the *system* safe despite an untrustworthy model.**
Least privilege on tools, validated and typed outputs, sandboxed execution, human approval for
irreversible actions, rate and budget limits, full traceability.

This is *exactly* how safety-critical automotive systems are built: assume the component fails,
engineer the system to contain it. **Say this in interviews.** It's the sentence that will make an
interviewer sit up, because it reframes AI safety as an engineering discipline you already practise.

---

## 6. Data protection

- **GDPR**: lawful basis for training data, purpose limitation, data minimization in prompts and
  logs, automated decision-making rules (Art. 22), DPIAs for high-risk processing, cross-border
  transfer
- **India DPDP Act 2023** — consent, data-fiduciary duties, and its implementing rules. Relevant to
  you as an India-based leader running platforms for a European parent.
- **Practical engineering consequences**: PII redaction before the model call, prompt/response log
  retention policy, right-to-erasure when the data is in a fine-tuned model (genuinely hard —
  have a view), data residency driving deployment topology
- **Your China experience is an asset here.** Delivering on Tencent Cloud for a China perimeter is
  data-sovereignty architecture. Frame it that way.

---

## 7. Study plan (Months 10–12, ~40h) and outputs

| Week | Focus | Output |
|---|---|---|
| 1–2 | EU AI Act structure, tiers, high-risk obligations; verify current timeline | Risk-classification decision tree (1 page) |
| 3–4 | ISO/IEC 42001 structure and controls | Gap-assessment template for an engineering org |
| 5 | NIST AI RMF; ISO/IEC 23894 | Comparison note: AI RMF vs 42001 vs AI Act — who needs which |
| 6–7 | ISO/PAS 8800 + ISO 21448 SOTIF | **Published article: "What AI safety could learn from SOTIF"** |
| 8 | The 21434/R155 ↔ AI Act mapping | **Published mapping document** |
| 9–10 | OWASP LLM Top 10, hands-on red team | Red-team report on your own agent |
| 11–12 | Data protection: GDPR + DPDP for AI systems | AI data-handling policy template |

**Two published artifacts from this section (the SOTIF piece and the 21434↔AI Act mapping) will
do more for your external visibility than anything else in this roadmap.** They sit in a gap
almost nobody is qualified to fill.
