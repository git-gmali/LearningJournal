# 10 — Credentials & Learning Resources

> At Director+ certificates are a weak signal — with three exceptions, noted below. Artifacts and
> writing beat credentials by a wide margin. Budget accordingly: ~80% of your hours to building
> and writing, ~20% to structured learning, and only one or two certifications total.

---

## 1. Certifications — what's worth it

### ✅ Worth doing

| Credential | Why | When | Rough cost |
|---|---|---|---|
| **ISO/IEC 42001 Lead Implementer** | The strongest differentiator available to you. Certifiable, governance-level (i.e. Director-shaped), pairs naturally with your ISO 21434 experience, and directly qualifies you for the under-supplied AI governance/assurance roles. | Months 10–14 | $1,500–2,500 |
| **AWS Certified Machine Learning Engineer – Associate** *(or the ML Specialty)* | You are AWS-heavy and it shows current-stack engagement. Moderate signal, low cost, quick. Skip if time is tight. | Month 8 | ~$150–300 |
| **NVIDIA DLI courses** (inference optimization, LLM deployment) | Not really a credential — genuinely useful hands-on content for [04](04_AI_Platform_And_Inference_Economics.md). Take for the learning, not the certificate. | Months 5–7 | Free–$500 |

### 🟡 Consider, conditionally

| Credential | Condition |
|---|---|
| **IAPP AIGP (AI Governance Professional)** | Only if you're deliberately targeting Archetype E (Head of AI Governance). Recognition is growing but still uneven. Redundant if you do ISO 42001. |
| **Cloud architecture professional-level cert** (if you don't already hold one) | Useful if your target is an org that screens on it. Low signal at Director level otherwise. |

### ❌ Skip

- Generic "AI for Executives" / "AI Strategy" certificates from business schools. These read as a
  substitute for depth, which is the exact impression you're trying to avoid.
- Prompt engineering certifications. No credibility.
- Vendor certifications for tools you don't use.
- Any bootcamp. You have a Masters and a PG in ML/AI already — your credential gap is not academic,
  it is *recency and evidence*.

### On your existing education

Your LJMU Masters and IIITB PG in ML/AI are real assets — they clear the "does this person have
formal grounding" filter instantly, which matters for non-technical hiring managers and for visa
processes. **Keep them prominent.** The problem is not the credentials; it is that the work sitting
on top of them is dated. Fix the top layer, not the foundation.

---

## 2. Courses — a short, high-signal list

**Don't over-enrol.** Two or three, finished, with the exercises done, beats twelve started.

| Priority | What | Why | Hours |
|---|---|---|---|
| 1 | A from-scratch LLM implementation course or book (build a small GPT end to end) | Removes bluffing permanently. The single best foundational investment. | 25–35 |
| 2 | Anthropic's and other providers' official docs + cookbooks, worked through | Current-stack, practical, free. Far more current than any course. | 15 |
| 3 | An evals-focused course or workshop | The biggest gap; formal structure helps here more than elsewhere. | 15 |
| 4 | An LLM inference/serving deep dive (vLLM docs + NVIDIA DLI material) | Directly feeds [04](04_AI_Platform_And_Inference_Economics.md). | 15 |
| 5 | ISO/IEC 42001 Lead Implementer training | Credential + real content. | 40 |

**On AI courses generally:** the field moves faster than curricula. Prefer primary sources —
provider documentation, model technical reports, framework docs, and papers — over packaged
courses for anything post-2024. Courses are best for *foundations* (transformers, training) where
the material is stable.

---

## 3. Reading

### Books (pick 4–5, not all)
- A build-an-LLM-from-scratch book — work the code, don't read it
- A practical LLM/AI engineering handbook covering RAG, agents, and evaluation end to end
- *Designing Data-Intensive Applications* — you likely know most of it, but the chapters on
  consistency and stream processing are worth re-reading through an AI-systems lens
- *Team Topologies* — directly applicable to the AI platform vs. embedded question in
  [06](06_AI_Leadership_And_Org_Design.md)
- *An Elegant Puzzle* or *The Manager's Path* — if you haven't read one, for the Director-scope
  leadership vocabulary
- A serious AI-policy/governance text once you're into [07](07_Governance_Safety_Regulation.md)

### Papers — read ~2/month, not 2/week
Read the *abstract, method, and limitations*. Skip the related work. A reading list to work
through, roughly in order:
- "Attention Is All You Need" — the origin
- Scaling laws: Kaplan et al., then Chinchilla
- LoRA — parameter-efficient fine-tuning
- InstructGPT/RLHF — how models became useful
- DPO — why preference optimization simplified post-training
- Chain-of-thought prompting, then a modern reasoning-model technical report
- PagedAttention / vLLM — the serving paper that matters most for your interests
- A recent open-weights model technical report (read one every ~6 months to track the frontier)
- A retrieval/RAG survey, and a GraphRAG paper
- An agent-architecture survey, plus at least one honest agent-failure-mode analysis
- Model distillation, and a recent small-model technical report — for [05](05_Edge_And_Physical_AI.md)
- A robot/vision-language-action foundation model paper — for physical AI context

### Primary sources to keep open
- Model provider documentation and cookbooks (Anthropic, and one other for comparison)
- vLLM / SGLang documentation
- MCP specification
- OWASP Top 10 for LLM Applications
- NIST AI RMF, NIST PQC publications (FIPS 203/204/205, IR 8547)
- The official EU AI Act text and the Commission's implementation pages
- ISO standard abstracts (full texts are paywalled; buy the two that matter most to you —
  ISO/IEC 42001 and ISO/PAS 8800 — and expense them if you can)

### Staying current — 20 minutes a day, no more
- Two or three high-signal AI engineering newsletters (prefer practitioner-written over
  news-aggregator)
- A small curated list on X/LinkedIn — researchers and infra practitioners, not commentators
- One SDV/automotive-software source to keep your domain edge sharp
- **Deliberately skip:** AI news cycles, model-release hype, and anything framed as "X changes
  everything." The signal-to-noise is terrible and it consumes the time you need for building.

---

## 4. Tools to actually use

| Category | Get hands-on with | Note |
|---|---|---|
| Coding agent | Claude Code or an IDE agent | Use daily on real work. This *is* practice for [06](06_AI_Leadership_And_Org_Design.md) §3. |
| Local models | Ollama or LM Studio | For edge experiments and quantization work |
| Serving | vLLM (primary), SGLang, llama.cpp (edge) | Stand at least one up yourself |
| Orchestration | Direct SDK calls first; a framework only when you feel the pain | Frameworks hide the mechanics you're trying to learn |
| Evals | Build your own first, then look at existing frameworks | Building it yourself is the learning |
| Tracing | An LLM observability tool + OpenTelemetry GenAI conventions | Cost/latency/quality on one view |
| Vector store | pgvector first | Reach for a dedicated vector DB only when you can articulate why |
| GPU access | Modal, RunPod, Lambda, or Colab Pro | Rent; don't buy hardware for this |
| MCP | Build a server yourself | Half a day, high current-stack signal |

**Budget:** ₹5,000–9,000/month (~$60–100) on API credits and GPU rental. Treat it as tuition. From
your conversation with Stephan, corporate credit allocation is already stalling your work for days
at a time — do not let your own learning sit in that queue.

---

## 5. Community and visibility

- **Speaking targets:** SDV/automotive software conferences, AI infrastructure meetups, MLOps and
  AI engineering community events, plus any Stellantis forum with external visibility.
  Submit to 4 CFPs in Months 10–16; expect 1–2 acceptances.
- **Open source:** a small, genuinely useful contribution in the inference/eval space beats a large
  abandoned personal repo. Even good documentation contributions to vLLM or an eval framework carry
  real signal.
- **Communities worth being in:** practitioner-heavy AI engineering communities and any
  SDV/automotive-software professional group. Lurk first, contribute specifics second.
- **Mentorship, both directions:** find one person two levels ahead in AI leadership and stay in
  touch quarterly; mentor two engineers on AI adoption. The second one is also how you build the
  internal case at Stellantis.
