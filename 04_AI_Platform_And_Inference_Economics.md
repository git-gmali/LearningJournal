# 04 — AI Platform & Inference Economics

> **Make this your best skill.** It is the single most-demanded and least-supplied capability at
> AI Director level in 2026. It is also the closest thing in the AI stack to what you have done
> for 22 years: making expensive systems cheap under hard constraints. You already have an AI
> cost-optimization program backed at SVP and CTO level — this file turns that from an initiative
> into an expertise.

---

## 1. The mental model

Traditional infra cost thinking (CPU hours, instance right-sizing, reserved capacity) does not
transfer cleanly. AI cost has a different shape:

| Traditional infra | AI inference |
|---|---|
| Cost ∝ instance-hours | Cost ∝ tokens, and tokens are user-controlled |
| Load is roughly predictable | A single user prompt can 100× its own cost via long context or reasoning tokens |
| Right-sizing is the main lever | Model choice, caching and routing dominate; right-sizing is third |
| Utilization is the KPI | Tokens/sec/GPU $ and cost-per-successful-task are the KPIs |
| Cheaper = same output | Cheaper usually = *different quality*. Cost and quality are coupled. |

**The core insight to internalize:** in AI, cost optimization is a *quality* decision, not just
an infra decision. That coupling is why it needs an engineering leader who understands both —
which is exactly the gap you can fill.

---

## 2. The unit economics you must be able to compute on a whiteboard

### 2.1 GPU memory math

```
Total GPU memory needed ≈ Model weights + KV cache + activations + framework overhead

Model weights   = parameters × bytes_per_param
                  (FP16 = 2 B, FP8 = 1 B, INT4 = 0.5 B)
                  e.g. 70B params @ FP16 = 140 GB  → needs 2× 80GB GPUs minimum

KV cache        = 2 × layers × kv_heads × head_dim × seq_len × batch × bytes_per_param
                  ("2" = one K and one V)
                  This is the term that explodes. At long context it can exceed the weights.

Activations     = relatively small during decode; significant during prefill
```

**Drill until automatic:** for a given model, how many concurrent requests fit at 8k context?
At 128k? What does GQA do to that number? What does FP8 KV cache do to it?

This is the calculation that decides your serving cost, and remarkably few directors can do it.

### 2.2 The latency decomposition

```
TTFT (time to first token)   ← prefill; compute-bound; scales with input length
TPOT (time per output token) ← decode; memory-bandwidth-bound; scales with model size
Total latency = TTFT + (TPOT × output_tokens)
```

Consequences you should be able to state instantly:
- Long prompts hurt TTFT, not TPOT → **prefix caching is the fix**
- Large models hurt TPOT → **quantization, speculative decoding, or a smaller model is the fix**
- Reasoning models generate many hidden output tokens → **they hurt total latency *and* cost
  superlinearly**. This is the #1 silent budget killer in 2026.

### 2.3 Throughput vs latency — the tension

Continuous batching raises throughput (tokens/sec/GPU, i.e. lowers $/token) but raises per-request
latency. **There is no single right operating point — there is a point per workload class.**

| Workload | Optimize for | Batching | Typical target |
|---|---|---|---|
| Interactive chat / voice | TTFT | Low, aggressive scheduling | TTFT < 500ms |
| In-vehicle voice assistant | TTFT hard ceiling | Edge-first | TTFT < 300ms |
| Background agent / batch analysis | $/token | Max batch, offline queue | Cost floor |
| Code/ops assistant | Balanced | Medium | TTFT < 2s |

> **Director-level artifact:** publish the throughput-latency-cost curve for one real model on
> one real GPU, measured by you. It's a weekend of work and it buys you enormous credibility.

---

## 3. The cost-reduction lever stack (in order of ROI)

Work top-down. Most teams start at #6 and wonder why nothing improved.

| # | Lever | Typical saving | Effort | Notes |
|---|---|---|---|---|
| 1 | **Don't use AI for it** | 100% | Low | Deterministic code, a regex, a lookup table. The most under-used lever in the industry. |
| 2 | **Prompt/prefix caching** | 50–90% on repeated prefixes | Low | Nearly free. Requires stable prompt prefixes — a design discipline. |
| 3 | **Model routing / cascading** | 40–80% | Medium | Cheap model first; escalate on low confidence. Needs a confidence signal and an eval to prove no quality loss. |
| 4 | **Prompt & context compression** | 20–50% | Medium | Stop stuffing the context window "just in case." Retrieval precision is a cost lever. |
| 5 | **Output-length control** | 20–40% | Low | Cap max tokens; discourage preamble; watch reasoning-token budgets. |
| 6 | **Quantization** | 30–60% on serving | Medium | Requires eval to prove quality is retained. |
| 7 | **Batching + scheduling** | 2–5× throughput | Medium | vLLM/SGLang continuous batching; separate interactive and batch tiers. |
| 8 | **Distillation to a small model** | 70–95% | High | Highest ceiling. Requires data, eval, and time. **Your edge play.** |
| 9 | **Edge / on-device inference** | ~100% of marginal cloud cost | High | Capex/device constraints instead. **Your moat.** See [05](05_Edge_And_Physical_AI.md). |
| 10 | **Commercial** — committed spend, batch APIs, provider mix | 20–50% | Low-Medium | Real, unglamorous, and Directors are expected to own it. |

### The measurement that matters

Not `$/token`. Not `$/request`. **`$ per successfully completed task`.**

A cheaper model that fails 30% of the time and needs a retry or a human is more expensive.
Building the instrumentation to measure cost-per-*success* is what makes the AI FinOps
conversation real — and it requires evals ([03](03_Technical_Syllabus.md) T1.6). The two skills
are joined.

---

## 4. The AI Gateway — the most Director-shaped piece of infrastructure

If you build or specify one thing as an AI platform leader, build this. A single ingress for all
model traffic in the organization:

```
        ┌──────────────────────────────────────────────────────────┐
        │                      AI GATEWAY                          │
App ──► │ authn/z → budget check → routing → cache → provider call │ ──► Models
        │            ↓ guardrails ↓        ↓ trace ↓ meter         │      (cloud / self-hosted / edge)
        └──────────────────────────────────────────────────────────┘
                                    │
                         cost · latency · quality telemetry
```

Responsibilities:
- **Auth & tenancy** — who is calling, on whose budget
- **Budget enforcement** — hard caps per team/feature; the thing that prevents a 3am cost incident
- **Routing** — model selection by policy; failover across providers; canarying a new model
- **Caching** — exact-match, semantic, and prefix
- **Guardrails** — input/output policy enforcement at a choke point
- **Observability** — one trace per request with cost, latency, tokens, model version, quality signal
- **Abstraction** — so a provider change is a config change, not a migration

**Why it's the right artifact for you:** it is a config-driven, multi-tenant, policy-enforcing
gateway. You built exactly this shape at Stellantis for vehicle commands (config-driven command
framework, single Kafka gateway). The pattern transfers, and you can say so credibly.

---

## 5. Capacity and procurement (Senior Director territory)

- **Buy vs rent vs serverless**: API per-token → serverless GPU → reserved cloud GPU → owned
  hardware. The crossover is usually around sustained high utilization; know how to compute it.
- **Utilization is everything for owned/reserved GPUs.** A reserved GPU at 20% utilization is
  more expensive than per-token API. Most enterprise self-hosting business cases quietly fail here.
- **Commitment risk**: hardware generations and model efficiency both move fast. Long commitments
  on specific hardware are a bet against improvement.
- **Multi-provider strategy**: real cost leverage, real operational tax. Have a view.
- **Data residency and sovereignty** as a cost driver — EU/China/India constraints force
  architecture choices. *You have lived this with Tencent Cloud for China; it's a strong story.*

---

## 6. Your Stellantis program — how to turn it into a portable asset

You have something most candidates don't: a real AI cost initiative with SVP and CTO backing.
Right now it is trapped inside the company. Extract the transferable layer:

**Write these five things (sanitized, no confidential figures):**

1. **The problem framing** — what was expensive, and how you established that it was expensive
   (measurement before optimization; most teams skip this)
2. **The decision framework** — how you chose which levers to pull, and in what order
3. **The quality guardrail** — how you ensured cost reduction didn't degrade outcomes. *This is
   the part that proves seniority.*
4. **The counterfactual** — what would have happened without the intervention, and how you know.
   (See causal inference in [03](03_Technical_Syllabus.md) T2.1.)
5. **The organizational move** — how you got SVP/CTO backing. Directors are hired as much for
   this as for the technical content.

Publish it as a framework with your own measured examples substituted for the confidential ones.
That becomes **Portfolio Project 5** ([09](09_Portfolio_Projects.md)) and probably your most
interview-useful artifact.

---

## 7. Hands-on checklist

- [ ] Stand up vLLM or SGLang locally or on a rented GPU. Serve a 7–8B model.
- [ ] Benchmark: throughput vs batch size vs context length. Plot it. Keep the data.
- [ ] Measure quality at FP16 / FP8 / INT4 on a fixed eval set. Find the knee. Write it up.
- [ ] Implement prefix caching; measure the saving on a realistic repeated-prompt workload.
- [ ] Build a two-tier router (small→large on low confidence); measure the cost/quality frontier.
- [ ] Compute the crossover point: at what sustained QPS does self-hosting beat per-token API
      for a given model? Show the working.
- [ ] Instrument cost-per-successful-task on one real workload, end to end.
- [ ] Write the AI gateway specification for a hypothetical 200-engineer org.

**Exit standard:** you can be handed an unfamiliar AI workload and a bill, and produce a
prioritized, quantified reduction plan with quality guarantees, in under an hour.
