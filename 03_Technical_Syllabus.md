# 03 — Technical Syllabus

> Tiered by how deeply you need it. **Tier 1** you must be able to argue about with a staff ML
> engineer and not lose. **Tier 2** you need working knowledge to make decisions. **Tier 3** you
> need to recognise and know who to ask.
>
> For each item the test is: *can you explain it to a smart engineer who doesn't know it, and
> answer the obvious follow-up question?*

---

## TIER 1 — Non-negotiable (Months 1–6)

### T1.1 Transformer architecture, properly

- **Self-attention**: Q/K/V, scaled dot-product, why the √d scaling, multi-head intuition
- **Attention variants and why they exist**: MHA → MQA → GQA → MLA. Every one of these is a
  *memory-bandwidth* optimization, which is exactly the kind of engineering tradeoff you've made
  your whole career. Understand them through that lens.
- **Positional encoding**: absolute → learned → RoPE; context-length extension (interpolation,
  YaRN-style scaling) and why long context degrades
- **The residual stream, layer norm / RMSNorm placement, feed-forward blocks, SwiGLU**
- **Mixture of Experts (MoE)**: sparse activation, routing, why it decouples parameter count from
  inference FLOPs, load balancing, and the memory cost you still pay
- **Decoder-only vs encoder-decoder**; why decoder-only won
- **Tokenization**: BPE, vocabulary design, and the failure modes it causes (numbers, code,
  non-English, and why token counts ≠ word counts for billing)

> **Build task:** implement single-head attention and a tiny character-level transformer once,
> from scratch, ~200 lines. You will never bluff about this again.

### T1.2 The training pipeline (know it, don't do it)

- **Pretraining**: next-token prediction, data mixture, curriculum, scaling laws
  (Kaplan → Chinchilla), compute-optimal vs inference-optimal training
- **Mid-training / continued pretraining**: domain adaptation — relevant if you ever adapt a model
  to automotive/telematics corpora
- **Post-training**:
  - SFT (supervised fine-tuning) — instruction following
  - RLHF — reward model + PPO; what actually makes models helpful/harmless
  - DPO and preference-optimization variants — why they displaced PPO for many teams
  - RLVR / verifiable-reward RL — the mechanism behind modern reasoning models
- **Distillation**: teacher→student; the single most important technique for *your* domain,
  because it's how frontier capability gets to the edge
- **Reasoning models and test-time compute**: chain-of-thought, extended thinking, verifier
  models, the cost implications of "thinking" tokens. *Director-relevant because reasoning
  tokens can 10× your bill without anyone noticing.*
- **Fine-tuning in practice**: full FT vs LoRA vs QLoRA; parameter counts, memory, when each is
  right; catastrophic forgetting

**The decision framework you must own** — asked in every AI Director interview:

| Situation | Right answer |
|---|---|
| Model lacks *knowledge* | RAG |
| Model lacks *format/style/behaviour* | Fine-tune (usually LoRA) |
| Model lacks *reasoning on your task* | Better prompting, then reasoning model, then distillation |
| Model needs to *do things* | Agent + tools |
| Cost is the problem | Routing, caching, smaller model, distillation |
| Latency is the problem | Smaller model, edge, speculative decoding, prefix caching |

### T1.3 Inference — the mechanics

*(economics covered separately in [04](04_AI_Platform_And_Inference_Economics.md))*

- **Prefill vs decode**: compute-bound vs memory-bandwidth-bound; why this split explains
  almost all serving behaviour
- **KV cache**: what it stores, how to size it (layers × heads × head_dim × 2 × seq_len ×
  batch × dtype), why it dominates memory at long context, PagedAttention
- **Continuous batching** vs static batching — the single biggest throughput lever
- **Prefix / prompt caching**: massive cost lever for repeated system prompts. Know it cold.
- **Speculative decoding**, draft models, medusa-style approaches
- **Quantization**: FP16/BF16 → FP8 → INT8 → INT4; weight-only vs activation quantization;
  AWQ, GPTQ, GGUF; quality degradation curves and where they knee
- **Parallelism**: tensor, pipeline, expert, data — what each costs in interconnect
- **Serving frameworks**: vLLM, SGLang, TensorRT-LLM, llama.cpp (edge). Know the tradeoffs.

### T1.4 Retrieval and context engineering

- **Embeddings**: what they encode, dimensionality, model choice, domain adaptation
- **Vector search**: HNSW, IVF, product quantization; recall/latency tradeoffs; when a vector DB
  is overkill (often — pgvector is frequently enough)
- **Hybrid retrieval**: BM25 + dense + fusion (RRF); why pure vector search underperforms
- **Rerankers**: cross-encoders, the two-stage retrieve-then-rerank pattern
- **Chunking**: fixed, semantic, hierarchical, late chunking; metadata design
- **Query transformation**: rewriting, decomposition, HyDE, multi-query
- **GraphRAG** and structured retrieval — relevant when relationships matter (vehicle
  configurations, part hierarchies, fault trees — very much your world)
- **Context engineering as a discipline**: context windows are a *budget*; what to put in,
  what to compress, what to leave out, context rot at long lengths
- **Evaluating retrieval separately from generation** — most RAG failures are retrieval failures

### T1.5 Agents

- **The loop**: perception → planning → tool call → observation → repeat; termination conditions
- **Tool/function calling**: schema design, parameter validation, error handling, idempotency
  (you know this from API design — it transfers directly)
- **MCP (Model Context Protocol)**: the emerging standard for tool/context integration.
  Build a server. It's a half-day and it's current-stack credibility.
- **Memory**: short-term (context), episodic, semantic; compaction and summarization strategies
- **Multi-agent patterns**: orchestrator-worker, hand-off, debate, parallel fan-out; and the
  honest counter-argument that a single well-scoped agent usually beats a multi-agent system
- **Agent failure modes**: infinite loops, context exhaustion, tool misuse, cascading
  hallucination, cost runaway, silent partial failure
- **Sandboxing and least privilege**: what an agent can touch, and the blast radius when it's wrong
- **Human-in-the-loop design**: approval gates, dry-run modes, reversibility — *your safety-critical
  instincts are an advantage here, use them*

### T1.6 Evals and AI quality engineering — **make this your strongest skill**

This is the biggest differentiator per hour invested. Very few engineering leaders can do it well.

- **Why traditional testing fails**: non-determinism, no single correct output, semantic equivalence
- **Eval types**: reference-based (exact/fuzzy/semantic), reference-free, rubric-based,
  LLM-as-judge, human eval, preference/pairwise
- **LLM-as-judge done right**: position bias, verbosity bias, self-preference bias, calibration
  against human labels, judge-model drift. *Knowing the biases is the expertise.*
- **Building a golden dataset**: sourcing, stratification, edge cases, adversarial cases,
  maintenance as the product changes
- **Task-specific metrics**: retrieval (precision@k, recall@k, MRR, NDCG), generation
  (faithfulness, answer relevance, groundedness), agents (task completion, step efficiency,
  tool-call accuracy)
- **Online eval**: A/B, interleaving, shadow deployment, canary, guarded rollout
- **Regression suites in CI**: quality gates that actually block a deploy
- **Observability**: tracing spans for LLM calls, OpenTelemetry GenAI semantic conventions,
  cost/latency/quality on one dashboard
- **Drift**: input drift, model drift (provider silently updates the model), prompt drift,
  retrieval-corpus drift

> **This is the skill that makes the rest of the roadmap credible.** A Director who can answer
> *"how do you know it's working?"* with a real methodology beats one with a better demo.

### T1.7 AI security

- **OWASP Top 10 for LLM Applications** — work through every item hands-on
- **Prompt injection** (direct and indirect) — the unsolved problem; mitigation is defence in
  depth, not a filter
- **Data exfiltration** via tool use, markdown image rendering, and retrieval side-channels
- **Excessive agency** — the most dangerous category for a company that controls vehicles
- **Supply chain**: model provenance, model weights as untrusted code, dependency risk
- **Training-data poisoning, membership inference, model extraction**
- **Guardrail architecture**: pre-filters, post-filters, classifier models, structured output
  constraints, policy engines, and why none of these are sufficient alone

---

## TIER 2 — Working knowledge (Months 6–12)

### T2.1 Classical ML that still pays (you have most of this — refresh, don't relearn)

- **Time-series forecasting** — directly powers your predictive autoscaling work. Know
  seasonality decomposition, and why gradient-boosted trees still beat deep models on most
  tabular/time-series problems.
- **Anomaly detection**: statistical, isolation forest, autoencoder, and the precision/recall
  reality of alerting at scale (you already live this)
- **Causal inference basics** — difference-in-differences, synthetic control. *This is how you
  defend the claim "AI saved us X" against a skeptical CFO.* Highly underrated at Director level.
- **Forecasting for capacity/cost** — bridges your infra work to the AI cost program

### T2.2 Data platform for AI

- Feature stores, and when you don't need one
- Data flywheel design: capture → label → retrain → deploy → capture
- Labeling operations, active learning, weak supervision
- Synthetic data generation — and its collapse risks. *Your vehicle simulator is a synthetic data
  generator; make that connection explicitly, it's a strong interview story.*
- Data lineage, rights, licensing, PII handling in prompts and logs
- Vehicle/telemetry data specifics: sampling, edge filtering, cost of moving data off-vehicle

### T2.3 MLOps / LLMOps

- Model registry, versioning, and treating prompts as versioned artifacts
- Deployment patterns: shadow, canary, blue-green, feature-flagged models
- Rollback strategy when the model is the regression
- Cost attribution per team/feature/user
- The AI gateway pattern: single ingress for auth, routing, rate-limiting, caching, logging,
  budget enforcement. **Build/specify one — it's the most Director-shaped piece of AI infra.**

### T2.4 Multimodal

- Vision-language models: architecture, image tokenization, resolution/cost tradeoffs
- Audio: ASR, TTS, speech-to-speech, latency budgets for in-vehicle voice
- Video understanding, and why it's expensive
- Document AI / OCR-free understanding — the highest-ROI enterprise multimodal use case

---

## TIER 3 — Informed observer (opportunistic)

- **World models and simulation-trained policies** — read enough to have a view; the SDV/robotics
  convergence makes this strategically relevant to you (see [05](05_Edge_And_Physical_AI.md))
- **Neuromorphic and analog compute** — watch, don't invest
- **Alternative architectures** (state-space/Mamba-class, hybrid attention) — know they exist
  and why long-context efficiency is the driver
- **Federated learning** — genuinely relevant to fleets; know the tradeoffs, low urgency
- **Quantum machine learning** — see [08](08_Quantum_And_PQC_Track.md); deliberately deprioritized

---

## Self-test checkpoints

Answer these out loud, to another person, without notes.

**After Month 3:**
1. Why does the KV cache dominate memory at long context, and what do you do about it?
2. When would you fine-tune rather than use RAG, and how would you decide?
3. What breaks first when you put an agent in production, and how would you detect it?

**After Month 6:**
4. Your inference bill tripled last month with flat traffic. Walk me through your diagnosis.
5. How do you know your AI feature is better this week than last week?
6. Prompt injection against a system that can send commands to a vehicle — how do you architect
   against it?

**After Month 12:**
7. Build vs buy vs fine-tune for a new AI capability — give me your framework and apply it.
8. How would you structure an AI platform team for a 200-engineer org, and what do they own?
9. Your model is in a high-risk EU AI Act category. What changes about how you build and ship?
10. Edge or cloud inference for this workload — defend your answer with numbers.
