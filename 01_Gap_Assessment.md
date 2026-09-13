# 01 — Gap Assessment: Where You Actually Stand

> Read this before anything else. The point is not to be discouraging — it's to stop you
> spending 18 months studying the things you already know.

---

## 1. What the market is actually hiring for at "AI Director+"

There are six distinct roles hiding behind that title. They are **not** interchangeable and
they have different bars:

| Archetype | Owns | Bar | Your fit today |
|---|---|---|---|
| **A. Director, AI/ML Engineering** | ML teams building models + pipelines | Deep DS/MLE background, published or shipped models | ⚠️ Weak — your DL credentials are pre-transformer |
| **B. Director, AI Platform / Inference Infra** | GPU fleet, serving, model gateway, cost | Distributed systems + inference economics | ✅ **Strong fit** — closest to your substrate skills |
| **C. Head of Applied AI / AI Products** | AI features inside a product | Product judgment + AI system design | 🟡 Medium — you have platform, less consumer product |
| **D. Director, AI Enablement / AI-native Delivery** | Internal productivity, agentic workflows, adoption | Org change + measurable throughput | ✅ **Strong fit** — you're already doing this at SDP |
| **E. Director/Head of AI Governance & Assurance** | Risk, compliance, model assurance | Regulation + engineering literacy | ✅ **Strong latent fit** — ISO 21434/R155 experience transfers |
| **F. VP Engineering (AI-native org)** | A whole engineering org that uses AI well | Classic eng leadership + AI credibility | ✅ **Strongest fit** — closest to what you do now |

**Strategic call: target B, D and F. Use E as the differentiator that makes you memorable.
Do not chase A.** You will lose an A-track interview to a 34-year-old with a PhD and three
papers, and that is fine — those roles are not where your 22 years compound.

---

## 2. Honest scorecard

Scale: 1 = no exposure · 2 = aware · 3 = working knowledge · 4 = can lead the work · 5 = can set direction others follow

### Where you're already strong (leverage, don't re-learn)

| Capability | Score | Evidence in your record |
|---|---|---|
| Large-scale distributed systems | 5 | V2C platform, 15M vehicles, Kafka gateway unification |
| Real-time / streaming / event-driven | 5 | Kafka message layer, remote ops, telematics |
| Edge & resource-constrained engineering | 5 | RLE compression at IndiaGames, BREW qualification, K-MAP fleet rendering, Harman HIE |
| Platform consolidation & config-driven design | 5 | VMN/RCZ unification, config-driven commands (weeks→hours) |
| Simulation-first / digital twin validation | 4 | Vehicle simulator, 1K+ simulated vehicles, 50% test-time cut |
| Org building & scaling | 5 | 0→40+ in a year; 3→12 at Harman; onboarding framework |
| Observability & production ops | 4 | ELK/Prometheus/Grafana, enterprise observability stack |
| Automotive safety & cybersecurity regulation | 4 | ISO 21434, UNECE R155/R156 delivery constraints |
| Cost optimization & infra right-sizing | 4 | Kafka/Redis/Mongo/JVM tuning, 10% headcount optimization |
| Executive stakeholder management | 4 | AI cost proposal backed at SVP + CTO |

### Where the gaps are (this is the roadmap's job)

| Capability | Score | Why it's a gap | Target | Fixed in |
|---|---|---|---|---|
| **Transformer / LLM internals** | 2 | Résumé says "CNN, RNN, LSTM, GRU" — that is 2018 vocabulary. An AI hiring manager reads it as *hasn't kept up*. | 4 | [03](03_Technical_Syllabus.md) Tier 1 |
| **Post-training (SFT/DPO/RLVR), reasoning models** | 1 | Zero exposure. You need to speak it, not do it. | 3 | [03](03_Technical_Syllabus.md) Tier 1 |
| **Inference serving & GPU economics** | 2 | You tune JVM/Kafka; you have not tuned a KV cache or reasoned about TTFT vs TPOT. | 5 ← *make this your best skill* | [04](04_AI_Platform_And_Inference_Economics.md) |
| **RAG / context engineering** | 2 | Not evidenced anywhere. Table stakes. | 4 | [03](03_Technical_Syllabus.md) Tier 1 |
| **Agentic systems & tool use (incl. MCP)** | 2 | Claimed on the AI-era résumé ("AI/agentic orchestration") but not evidenced. That's a liability in interview. | 4 | [03](03_Technical_Syllabus.md), [09](09_Portfolio_Projects.md) |
| **Evals & AI quality engineering** | 1 | **Biggest single gap.** This is the #1 thing that separates "uses AI" from "runs AI in production." | 5 | [03](03_Technical_Syllabus.md), [09](09_Portfolio_Projects.md) P4 |
| **AI security (prompt injection, OWASP LLM)** | 2 | You know product security, not model security. | 4 | [07](07_Governance_Safety_Regulation.md) |
| **AI regulation (EU AI Act, ISO 42001)** | 2 | Adjacent knowledge, not the actual frameworks. Cheap to close, high differentiation. | 5 | [07](07_Governance_Safety_Regulation.md) |
| **AI unit economics / FinOps for AI** | 2 | You run a cost program but on infra logic, not token/GPU logic. | 5 | [04](04_AI_Platform_And_Inference_Economics.md) |
| **Public technical presence** | 1 | **Second biggest gap.** No talks, no OSS, no writing, no visible artifacts. At Director+ this is how the market finds you. | 4 | [09](09_Portfolio_Projects.md), [11](11_Positioning_And_Job_Search.md) |
| **Hands-on modern AI coding** | 2 | You must be able to build a working agent yourself. Directors who can't lose the room. | 4 | [02](02_Master_Roadmap.md) Phase 1 |
| **Post-quantum cryptography** | 2 | You explored blockchain for PKI — right instinct, wrong technology. PQC is the actual answer and it's urgent in automotive. | 4 | [08](08_Quantum_And_PQC_Track.md) |
| **Quantum computing** | 1 | Not on the critical path. Deliberately low priority. | 2 | [08](08_Quantum_And_PQC_Track.md) |

---

## 3. The five specific liabilities to fix first

These are things that will actively cost you in a screening call:

1. **"TensorFlow, scikit-learn | CNN, RNN, LSTM, GRU" on the résumé.**
   In 2026 this signals a leader who stopped learning around 2019. It must be replaced with
   current stack vocabulary backed by real work. Do not simply swap the words — that's worse,
   because the follow-up question will expose it. Build first, rewrite second.

2. **"AI/agentic orchestration" claimed without artifacts.**
   The AI-era résumé makes claims you cannot currently defend for 20 minutes under questioning.
   Either build the evidence (Phase 1–2) or soften the claim until you have. An unsupported
   AI claim from a 22-year veteran reads as desperation.

3. **The AI cost-optimization program is your best asset and it's invisible.**
   SVP + CTO backing is genuinely strong currency. But right now it exists only as an internal
   Stellantis initiative with no portable, sanitized articulation. If you leave tomorrow you
   can describe it but not *show* it. Fix that in Phase 2.

4. **No AI on the P&L side.** Directors are asked: *"What did AI cost you and what did it
   return?"* You need a defensible number with a methodology you can explain, including the
   counterfactual. Most candidates cannot do this. It is a very cheap way to stand out.

5. **Constraint that is actually also an asset: you are credit-starved.**
   From the transcript — 2000 AI credits burned in a day or two, initiatives halted. That is a
   real blocker for learning. **Budget your own money for this.** A personal spend of
   ~$50–100/month on API credits + a modest GPU rental budget is the single highest-ROI
   investment in this entire plan. Do not wait for corporate budget approval to learn.

---

## 4. What you get to skip

Do not spend time on these. You either already have them or they don't pay at your level:

- ❌ Re-learning Python fundamentals, pandas/numpy basics — you have them, they're not the gate.
- ❌ Classical DL from scratch (backprop by hand, CNN architectures). Pre-transformer DL is
  ~10% of the modern job and you have the credential already.
- ❌ Kaggle competitions. Zero signal at Director level.
- ❌ Generic "AI for Executives" certificates. Negative signal — they read as substitute for depth.
- ❌ Training a foundation model. You will never do this and nobody hiring you expects it.
- ❌ Deep math re-derivation (measure theory, optimization proofs). Know what the objects *do*.
- ❌ Becoming a quantum programmer. See [08](08_Quantum_And_PQC_Track.md) for why.

---

## 5. Time-to-credible estimate

| Milestone | When | What "done" looks like |
|---|---|---|
| **Can hold a technical AI conversation with a staff ML engineer without bluffing** | Month 3 | You can whiteboard attention, explain KV-cache cost, and argue RAG-vs-fine-tune with reasons |
| **Can defend an AI Director résumé in a screening call** | Month 6 | Two shipped artifacts, one public write-up, real eval and cost numbers |
| **Externally competitive for Director, AI Platform / AI-native VP Eng** | Month 12 | Signature project public, 1 conference talk, governance credential in flight, network warm |
| **Competitive for Senior Director / Head of AI** | Month 18 | Recognised POV in edge/physical AI, speaking invitations inbound, 2+ referral paths per target company |

**And the parallel track:** every artifact above is also the evidence Stephan needs to make the
internal Director case. The migration-completion evidence he described + a demonstrable AI
platform capability you personally created is a far stronger promotion dossier than delivery
metrics alone. Build once, use twice.
