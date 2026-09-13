# 05 — Edge & Physical AI: Your Moat

> This is the file that makes you *rare* rather than merely *qualified*. Everything else in this
> roadmap gets you to parity with other candidates. This gets you past them.

---

## 1. Why this is your moat

Most AI leaders in 2026 came up through web, data or research. They understand models and cloud.
Almost none of them have ever shipped inference onto a device with a fixed memory budget, a
thermal envelope, an intermittent network, a 15-year service life, and a regulator who can pull
the product.

**You have done all of that, repeatedly, for 22 years:**

| Your experience | What it maps to in physical AI |
|---|---|
| RLE compression at IndiaGames (60–70% reduction, BREW qualification) | Model compression, quantization, on-device footprint budgets |
| K-MAP rendering for 100k+ fleet pins on a phone | Efficient computation under hard device constraints |
| **Harman Hybrid Inference Engine** — on-device inference + model adaptation | *Literally the architecture pattern the industry is now rediscovering for edge LLMs* |
| Aha Connect SDK — device- and protocol-agnostic | Hardware-abstraction for heterogeneous accelerators (NPU/DSP/GPU) |
| V2C platform at 15M vehicles | Fleet-scale model deployment, OTA model updates, staged rollout |
| Config-driven vehicle simulator, 1K+ vehicles | **Synthetic data generation and sim-to-real** — same thing, new vocabulary |
| ISO 21434 / UNECE R155-R156 delivery | Safety and security assurance for AI-carrying systems |
| Kafka/streaming telemetry at scale | The data flywheel that feeds fleet learning |

The Hybrid Inference Engine is the most underexploited line on your résumé. You architected an
on-device inference and model-adaptation platform. That is the *exact* problem shape the entire
edge-AI industry is working on right now. Currently it's one bullet. It should be a story you
tell in every interview, updated to today's stack.

---

## 2. The market thesis (why this is where value is going)

- Cloud-only inference is hitting three walls simultaneously: **cost** (tokens at fleet scale),
  **latency** (interactive and safety-relevant use cases), and **sovereignty/privacy** (where the
  data may go).
- The answer everyone is converging on is **hybrid**: small capable models on-device, escalation
  to cloud for hard cases, continuous distillation from cloud to edge.
- Simultaneously, AI is moving from screens into things that move — vehicles, robots, drones,
  industrial equipment. "Physical AI" / "embodied AI" is where the next decade of platform
  investment goes.
- The talent flow between SDV and robotics is real and bidirectional. The skills are the same:
  real-time distributed systems, sensor fusion, safety cases, OTA, simulation, fleet learning.

**The roles this opens:** Director of Edge AI, Head of AI Platform at an OEM/Tier-1, Director of
AI Infrastructure at a robotics company, VP Engineering at an industrial-autonomy scale-up,
Head of SDV Software. All of these value your 22 years. None of them require a PhD.

---

## 3. What to learn

### 3.1 Model compression and edge deployment

- **Quantization for edge**: post-training quantization vs quantization-aware training;
  INT8/INT4; AWQ, GPTQ, GGUF; per-channel vs per-tensor; where quality actually breaks
- **Distillation**: the core technique. Teacher (frontier) → student (edge-sized). Response-based,
  feature-based, and task-specific distillation. Data generation for distillation.
- **Pruning**: structured vs unstructured; why structured pruning is what actually helps hardware
- **Small language models (SLMs)**: the 1B–8B class. What they can and cannot do. The rapid
  capability improvement at small sizes is the enabling trend for everything here.
- **Speculative decoding on device**; early exit; adaptive computation
- **Hardware awareness**: NPUs/accelerators in automotive SoC classes, DSPs, memory bandwidth as
  the binding constraint, thermal and power envelopes. *You already think this way.*
- **Runtimes**: llama.cpp, ONNX Runtime, TensorRT, ExecuTorch, vendor SDKs, and the
  hardware-abstraction problem across an OEM's heterogeneous fleet

### 3.2 Hybrid edge-cloud architecture — the core pattern

```
   ┌─────────────── VEHICLE / DEVICE ────────────────┐
   │  Sensors ─► Small model (on NPU)                │
   │                │                                │
   │                ├─ confident? ──► respond locally│   ← low latency, zero marginal cost
   │                │                                │
   │                └─ uncertain? ──┐                │
   └────────────────────────────────┼────────────────┘
                                    ▼
                          ┌──────────────────┐
                          │  CLOUD (large)   │ ──► response + logged hard case
                          └──────────────────┘
                                    │
                     hard cases ────┴──► distillation ──► next edge model ──► OTA
                                              (the fleet learning flywheel)
```

The design questions — which are the interview questions:
- **What is the escalation signal?** Confidence, entropy, a verifier model, a rules layer?
- **What happens when the network is gone?** Graceful degradation is a product decision.
- **Privacy**: what data may leave the vehicle at all? What can be learned without it?
- **OTA for models**: staged rollout, canary cohorts, rollback when the model is the regression,
  and the regulatory implication that a model update may be a *software update* under R156.
- **Version skew**: a fleet of 15M vehicles is never on one model version. Design for the spread.
- **Fleet learning**: federated learning vs centralized retraining on logged hard cases.

> **You have already solved the non-AI version of all of these.** OTA at 15M vehicles, staged
> rollout, version skew, config-driven behaviour. Say this explicitly when you talk about it —
> the transfer is the whole argument.

### 3.3 Simulation, synthetic data, and world models

- **Your vehicle simulator is a synthetic data generator.** Reframe it that way permanently.
- Sim-to-real transfer and the reality gap; domain randomization
- Scenario generation for coverage — including rare/edge cases you can't collect in the field
- **World models**: learned simulators; predicting future states from action sequences. The
  research frontier for robotics and AV, increasingly for planning generally.
- Digital twins for fleet-scale testing — the SDV term for what you've already built
- Evaluation in simulation vs field validation, and the argument for why sim results count

### 3.4 Physical AI / robotics adjacency

Enough to hold a strategic conversation, not to build:
- Robot foundation models / vision-language-action models — the "one model, many embodiments" thesis
- Sensor fusion and perception stacks
- The autonomy levels framing, and why the same staged-autonomy argument now appears in
  enterprise agents
- Safety cases for learned components — where [07](07_Governance_Safety_Regulation.md) and this
  file meet. **ISO/PAS 8800 is the bridge document.**

---

## 4. The signature project

**Portfolio Project 3 — Hybrid Edge-Cloud Inference Router** (detailed in [09](09_Portfolio_Projects.md)).

Build it. Publish it. It should include:
- A small model running locally (quantized, measured footprint and tokens/sec on real hardware)
- A confidence-based escalation policy to a frontier cloud model
- Measured results on **three axes**: latency, cost, and quality — as a tradeoff surface, not a
  single number
- The failure analysis: what the escalation policy gets wrong, and what that costs
- A written architecture note connecting it to fleet-scale OTA model deployment

**Title it something like:** *"Hybrid Inference, Ten Years On: what I'd rebuild from the Harman
HIE with 2026 models."* That headline does an enormous amount of work — it establishes that you
were doing this before it was a trend, and that you've kept up.

---

## 5. Reframing your résumé lines

Change these in Phase 4 ([11](11_Positioning_And_Job_Search.md)):

| Current | Reframed |
|---|---|
| "Architected the Hybrid Inference Engine (HIE): end-to-end ML inference and model-adaptation platform for on-device personalized recommendations." | "Architected an on-device inference and model-adaptation platform (Hybrid Inference Engine) — edge-first inference with cloud-assisted adaptation, shipped into OEM vehicle programs. The hybrid edge/cloud pattern the industry standardized on a decade later." |
| "Architected a config-driven simulator for large-scale remote-operation testing" | "Built a synthetic-environment platform generating fleet-scale scenario data for validation without physical assets — 1K+ simulated vehicles, 50% less test time. Sim-first validation for systems that can't be tested in production." |
| "Predictive ML autoscaling; AI anomaly detection; self-healing architecture" | "Applied ML to platform operations: demand forecasting for capacity, anomaly detection on fleet telemetry, and automated remediation — with the measured cost and reliability outcomes." |

Same facts. The second column is written in the vocabulary the market searches on.

---

## 6. Hands-on checklist

- [ ] Run a quantized small model on the most constrained hardware you own. Measure footprint,
      tokens/sec, and power/thermal behaviour if you can.
- [ ] Quantize the same model at 3+ precisions; run a fixed eval set at each; plot quality vs size.
- [ ] Distill a small model for one narrow task using a frontier model as teacher. Measure the
      gap on your eval set.
- [ ] Build the confidence-based escalation router. Sweep the threshold; plot the cost/quality
      frontier.
- [ ] Design (on paper, rigorously) OTA model deployment for a 15M-unit fleet: cohorts, canary,
      rollback, version skew, regulatory classification.
- [ ] Write the ISO/PAS 8800 ↔ AI-assurance mapping note (shared with [07](07_Governance_Safety_Regulation.md)).
