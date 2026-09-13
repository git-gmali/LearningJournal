# 14 — Glossary & Abbreviations

> Every acronym and piece of jargon used across files 01–13, in one place, so you don't have to
> break flow to search for it. Organized by topic, not alphabetically — read the section that
> matches what you're currently studying. Two terms that look alike but mean very different
> things are flagged explicitly (search "⚠️ not to be confused with").

---

## Core LLM & model concepts

| Term | Meaning |
|---|---|
| **LLM** | Large Language Model — a neural network trained on huge text corpora to predict/generate text |
| **VLM** | Vision-Language Model — an LLM that also takes images as input |
| **SLM** | Small Language Model — the 1–8B-parameter class, small enough to run on-device |
| **Transformer** | The neural network architecture behind virtually all modern LLMs, built on the attention mechanism |
| **Attention / self-attention** | The mechanism letting a model weigh how relevant every other token is to the one it's currently processing |
| **Q/K/V** | Query, Key, Value — the three vectors attention computes and compares to decide what to focus on |
| **Multi-head attention (MHA)** | Running several attention computations in parallel, each learning a different kind of relationship |
| **MQA** | Multi-Query Attention — all heads share one Key/Value set, to save memory bandwidth |
| **GQA** | Grouped-Query Attention — a middle ground between MHA and MQA (groups of heads share K/V) |
| **MLA** | Multi-head Latent Attention — a further memory-compression scheme for the KV cache (used in some recent models, e.g. DeepSeek's) |
| **RoPE** | Rotary Position Embedding — the dominant way modern transformers encode token position |
| **Residual stream** | The running "main channel" of information that each transformer layer reads from and adds back into |
| **LayerNorm / RMSNorm** | Normalization techniques that keep activations numerically stable through many layers |
| **FFN / MLP block** | Feed-Forward Network — the per-token, position-independent computation inside each transformer layer |
| **SwiGLU** | A specific activation-function variant commonly used in modern FFN blocks |
| **MoE** | Mixture of Experts — a model design where only a subset of "expert" sub-networks activate per token, decoupling parameter count from compute cost |
| **Decoder-only** | The now-dominant transformer variant (GPT-style) that only predicts the next token, vs. older encoder-decoder designs |
| **Tokenization / BPE** | Breaking text into sub-word units (Byte-Pair Encoding is the common algorithm) before feeding it to a model; token count ≠ word count |
| **Parameters** | The learned numeric weights of a model; "70B" means 70 billion parameters |
| **Context window** | The maximum number of tokens (input + output) a model can attend to at once |
| **Temperature / top-p / top-k** | Sampling controls for how random vs. deterministic generated text is |
| **Hallucination** | A model generating fluent but false or unsupported content |
| **Context rot** | Quality degradation that occurs as a context window fills up, even below its hard limit |

## Training & adaptation

| Term | Meaning |
|---|---|
| **Pretraining** | The initial, massive-scale training phase — next-token prediction over large text corpora |
| **Scaling laws** | Empirical relationships between model size, data size, compute, and resulting performance (Kaplan, then Chinchilla) |
| **Post-training** | Everything done to a pretrained model to make it useful/aligned: SFT, RLHF, DPO, RLVR |
| **SFT** | Supervised Fine-Tuning — training on labeled (instruction → ideal response) examples |
| **RLHF** | Reinforcement Learning from Human Feedback — using a reward model trained on human preferences to further tune a model (classically via PPO) |
| **PPO** | Proximal Policy Optimization — a reinforcement-learning algorithm historically used in RLHF |
| **DPO** | Direct Preference Optimization — a simpler alternative to RLHF/PPO that skips the separate reward model |
| **RLVR** | Reinforcement Learning from Verifiable Rewards — training with rewards from checkable outcomes (e.g. "did the code pass tests"); the mechanism behind modern reasoning models |
| **Reasoning model / test-time compute / "thinking" tokens** | Models that generate extended internal reasoning steps before answering — higher quality, and higher token cost (a hidden cost driver — see [04](04_AI_Platform_And_Inference_Economics.md)) |
| **Fine-tuning** | Further training a pretrained model on a narrower dataset to change its behaviour, format, or domain knowledge |
| **LoRA** | Low-Rank Adaptation — a parameter-efficient fine-tuning method that trains small added matrices instead of the whole model |
| **QLoRA** | LoRA combined with quantizing the base model, to fine-tune large models on modest hardware |
| **Catastrophic forgetting** | A model losing previously learned capability while being fine-tuned on new data |
| **Distillation** | Training a smaller "student" model to mimic a larger "teacher" model's outputs — the core technique for edge deployment |
| **Knowledge cutoff** | The date after which a model has no training data / awareness of events |

## Inference & serving

| Term | Meaning |
|---|---|
| **Inference** | Running a trained model to produce an output (as opposed to training it) |
| **Prefill** | The compute-heavy phase where the model processes the entire input prompt at once |
| **Decode** | The memory-bandwidth-bound phase where the model generates output tokens one at a time |
| **TTFT** | Time To First Token — latency from request to the first generated token (dominated by prefill) |
| **TPOT** | Time Per Output Token — latency per subsequent token (dominated by decode) |
| **KV cache** | Stored Key/Value tensors from earlier tokens, reused so the model doesn't recompute attention from scratch each step; the dominant memory cost at long context |
| **PagedAttention** | A memory-management technique (from the vLLM project) that lets the KV cache be allocated in non-contiguous chunks, like OS virtual memory |
| **Continuous batching** | Dynamically adding/removing requests from a GPU batch as they arrive/finish, instead of waiting for a fixed batch to fill — the single biggest throughput lever in serving |
| **Prefix caching / prompt caching** | Reusing computed attention state for a repeated prompt prefix (e.g. a shared system prompt) instead of recomputing it — a major cost lever |
| **Speculative decoding** | Using a small "draft" model to propose several tokens which the large model verifies in one pass, speeding up decode |
| **Quantization** | Reducing the numeric precision used to store/compute model weights (and sometimes activations), trading some quality for much lower memory/compute cost |
| **FP16 / BF16 / FP8 / INT8 / INT4** | Numeric precision formats, from highest (FP16, 16-bit float) to lowest (INT4, 4-bit integer) — lower precision = smaller/faster but lower fidelity |
| **AWQ, GPTQ** | Popular post-training quantization algorithms for LLMs |
| **GGUF** | A model file format optimized for quantized inference on CPUs/consumer hardware (used by llama.cpp, Ollama) |
| **Tensor / pipeline / expert / data parallelism** | Different ways of splitting a model or workload across multiple GPUs |
| **vLLM, SGLang, TensorRT-LLM** | Popular open-source/vendor inference-serving engines that implement the above optimizations |
| **llama.cpp** | A lightweight inference engine designed for running quantized models on CPUs and edge devices |
| **QPS** | Queries Per Second — a throughput measure |
| **NPU / DSP** | Neural Processing Unit / Digital Signal Processor — specialized low-power chips used for on-device (edge) AI inference |
| **SoC** | System on Chip — an integrated chip (e.g. in a vehicle's compute unit) combining CPU, GPU/NPU, memory controller, etc. |

## Retrieval & context (RAG)

| Term | Meaning |
|---|---|
| **RAG** | Retrieval-Augmented Generation — fetching relevant external documents and inserting them into the prompt so the model can use information it wasn't trained on |
| **Embedding** | A numeric vector representation of text (or other data) capturing its meaning, used for similarity search |
| **Vector database / vector search** | A database optimized for finding the nearest embeddings to a query embedding |
| **HNSW** | Hierarchical Navigable Small World — a common fast approximate-nearest-neighbor algorithm used in vector search |
| **IVF** | Inverted File Index — another approximate-nearest-neighbor indexing method |
| **PQ (Product Quantization)** | A vector-compression technique used inside some vector search indexes. ⚠️ **Not to be confused with PQC (Post-Quantum Cryptography, see below) — same initials, unrelated fields.** |
| **pgvector** | A PostgreSQL extension adding vector-similarity search to a regular Postgres database — often "good enough" instead of a dedicated vector DB |
| **BM25** | A classic keyword/term-frequency-based search ranking algorithm, often combined with vector search ("hybrid retrieval") |
| **Reranker / cross-encoder** | A second-stage model that re-scores an initial set of retrieved candidates for better precision |
| **RRF** | Reciprocal Rank Fusion — a simple method for combining rankings from multiple retrieval methods (e.g. BM25 + vector) |
| **Chunking** | Splitting documents into smaller pieces before embedding/indexing them |
| **HyDE** | Hypothetical Document Embeddings — a query-transformation technique: generate a hypothetical answer, then search using *its* embedding |
| **GraphRAG** | Retrieval that uses a knowledge graph of entities/relationships instead of (or alongside) plain vector search — useful when relationships between things matter (e.g. vehicle part hierarchies) |
| **Context engineering** | The discipline of deciding what information to put into a model's limited context window, and how to compress/prioritize it |

## Agents

| Term | Meaning |
|---|---|
| **Agent** | An LLM-driven system that can plan, call tools, observe results, and take further steps toward a goal (vs. a single one-shot prompt) |
| **Tool calling / function calling** | A model producing a structured request to invoke an external function/API, which the surrounding code executes |
| **ReAct** | A common agent pattern: alternating "Reason" (think) and "Act" (call a tool) steps |
| **MCP** | Model Context Protocol — an open standard for connecting AI models to external tools/data sources in a consistent way; increasingly the default integration layer |
| **Orchestrator-worker** | A multi-agent pattern where one agent plans/delegates and others execute sub-tasks |
| **Excessive agency** | An AI-security term (OWASP) for an agent having more permissions or autonomy than it needs — the top risk category for anything that can touch a physical system |
| **Human-in-the-loop** | A design where a human must approve certain agent actions before they execute |

## Evals & quality

| Term | Meaning |
|---|---|
| **Eval / evaluation harness** | A systematic framework for measuring whether an AI system's outputs are good, run repeatably (like a test suite, but for non-deterministic outputs) |
| **Golden dataset** | A curated, labeled set of example inputs/expected outputs used as the ground truth for evals |
| **LLM-as-judge** | Using one LLM to score/grade another LLM's output, in place of (or alongside) human grading |
| **Judge calibration** | Measuring how well an LLM judge's scores agree with real human judgments |
| **Groundedness / faithfulness** | Whether a generated answer is actually supported by the retrieved/provided source material (vs. hallucinated) |
| **Precision@k, Recall@k, MRR, NDCG** | Standard information-retrieval metrics for how good a search/retrieval result set is |
| **Drift** | A model, prompt, or data distribution changing over time in a way that degrades quality without code changing |
| **Regression suite** | A set of tests re-run automatically (e.g. in CI) to catch quality drops before they ship |
| **OTel / OpenTelemetry** | An open standard for instrumentation/tracing; has emerging conventions specifically for logging LLM calls (cost, tokens, latency) |

## AI security

| Term | Meaning |
|---|---|
| **OWASP** | Open Worldwide Application Security Project — publishes widely-used security checklists, including a "Top 10" specifically for LLM applications |
| **Prompt injection** | An attack where malicious instructions are smuggled into a model's input (directly by a user, or indirectly via retrieved/external content) to hijack its behaviour |
| **Jailbreak** | A prompt-injection variant specifically aimed at bypassing a model's safety guardrails |
| **Guardrails** | Layered defenses (input/output filters, policy engines, sandboxing) that constrain what an AI system can do or say |
| **Red-teaming** | Deliberately attacking your own system to find its failure modes before an adversary does |

## Business / leadership shorthand

| Term | Meaning |
|---|---|
| **ROI / TCO** | Return on Investment / Total Cost of Ownership |
| **KPI** | Key Performance Indicator |
| **FinOps** | The discipline of managing and optimizing cloud (or AI) spend as a cross-functional practice |
| **DORA metrics** | Four widely-used software delivery metrics: deployment frequency, lead time for changes, change failure rate, and MTTR |
| **MTTR** | Mean Time To Recovery/Repair — how long it takes to fix a failure once it occurs |
| **CoE** | Center of Excellence — a centralized team of specialists supporting the wider org |
| **POV** | Point of View — a stated, defensible position on a topic |
| **CFP** | Call For Proposals — the submission process for speaking at a conference |
| **CBOM** | Cryptographic Bill of Materials — an inventory of every cryptographic algorithm/key/certificate in use across a system (the starting point for any PQC migration) |

## Cloud, infra & general tooling

| Term | Meaning |
|---|---|
| **API / SDK / CLI** | Application Programming Interface / Software Development Kit / Command-Line Interface |
| **uv** | A fast, modern Python package and virtual-environment manager (used throughout [13](13_Lab_Setup_Guide.md)) |
| **venv** | A Python virtual environment — an isolated set of installed packages per project |
| **K8s** | Kubernetes — a container-orchestration platform (already in your existing stack) |
| **ELK Stack** | Elasticsearch, Logstash, Kibana — a common observability/log-analysis stack (already in your existing stack) |
| **CI/CD** | Continuous Integration / Continuous Deployment — automated testing and release pipelines |
| **GPU** | Graphics Processing Unit — the hardware most AI training/inference runs on, due to its parallel-compute design |

## AI governance & regulation

| Term | Meaning |
|---|---|
| **EU AI Act** | The European Union's binding AI regulation, tiering AI systems by risk level with obligations attached to each tier |
| **GPAI** | General-Purpose AI — the EU AI Act's category for foundation models themselves (as distinct from specific AI *applications* built on them) |
| **High-risk AI system** | An EU AI Act classification triggering the heaviest compliance obligations (risk management, documentation, human oversight, etc.) |
| **Provider / Deployer** | EU AI Act roles: the *provider* builds/places an AI system on the market; the *deployer* uses it. Obligations differ, and a company can become a provider by substantially modifying a third-party model. |
| **Conformity assessment** | The formal process of checking a high-risk AI system meets legal requirements before it can be placed on the market |
| **ISO/IEC 42001** | An international, certifiable standard for an organization's AI Management System (governance processes around AI, not the models themselves) |
| **ISO/IEC 23894** | A companion ISO standard giving guidance specifically on AI risk management |
| **NIST AI RMF** | The US National Institute of Standards and Technology's (voluntary) AI Risk Management Framework |
| **GDPR** | The EU's General Data Protection Regulation, governing personal data (relevant to what goes into prompts/logs/training data) |
| **DPDP Act** | India's Digital Personal Data Protection Act (2023) — India's equivalent data-protection law |
| **AIGP** | AI Governance Professional — a certification offered by the IAPP (International Association of Privacy Professionals) |

## Automotive safety & security (your existing domain — included for cross-reference)

| Term | Meaning |
|---|---|
| **ISO 26262** | The core international standard for automotive **functional safety** (systematic/random hardware-software faults) |
| **ISO 21448 (SOTIF)** | Safety Of The Intended Functionality — handles hazards from a system's performance *limitations* even when nothing "broke" (the natural safety frame for ML/AI behaviour) |
| **ISO/PAS 8800** | A newer standard specifically addressing safety and AI in road vehicles — the direct bridge between your existing domain and AI assurance |
| **ISO/SAE 21434** | The core automotive **cybersecurity engineering** standard (you already hold this experience) |
| **UNECE R155** | A UN regulation requiring a **CSMS** (Cybersecurity Management System) for vehicle type approval |
| **UNECE R156** | The companion UN regulation on **Software Update Management Systems (SUMS)** — governs how OTA updates (including model updates) must be managed |
| **CSMS** | Cybersecurity Management System — the organizational process R155 requires |
| **SUMS** | Software Update Management System — the organizational process R156 requires |
| **AUTOSAR** | AUTomotive Open System ARchitecture — a standardized automotive software architecture/platform |
| **V2X** | Vehicle-to-Everything — communication between a vehicle and other vehicles, infrastructure, etc. |
| **V2C** | Vehicle-to-Cloud — your specific platform domain at Stellantis |
| **OTA** | Over-The-Air (software/firmware updates delivered remotely, without a physical connection) |
| **PKI** | Public Key Infrastructure — the certificate/key system underlying secure vehicle communication and OTA signing |
| **HSM** | Hardware Security Module — a physical device that securely stores/manages cryptographic keys, often used for vehicle secure boot/signing |
| **SDV** | Software-Defined Vehicle — the industry term for vehicles whose functionality is primarily defined/updated by software rather than fixed hardware |
| **Digital twin** | A virtual/simulated replica of a physical system used for testing without the physical asset (your vehicle simulator, reframed) |

## Post-quantum cryptography (PQC)

| Term | Meaning |
|---|---|
| **PQC** | Post-Quantum Cryptography — cryptographic algorithms designed to remain secure even against attacks from a future quantum computer. ⚠️ **Not to be confused with PQ / Product Quantization above — unrelated, just similar-looking initials.** |
| **Shor's algorithm** | A quantum algorithm that (on a sufficiently powerful quantum computer) can break RSA/ECC-style asymmetric cryptography — the core PQC threat |
| **Grover's algorithm** | A quantum algorithm that speeds up brute-force search, roughly halving the effective strength of symmetric keys (e.g. AES-256 remains fine; it's not the same category of threat as Shor's) |
| **"Harvest now, decrypt later"** | The threat model where encrypted data is captured/stored today so it can be decrypted once a quantum computer capable of breaking it exists |
| **ML-KEM (FIPS 203)** | The NIST-standardized post-quantum Key Encapsulation Mechanism (lattice-based, derived from Kyber) |
| **ML-DSA (FIPS 204)** | The NIST-standardized post-quantum digital-signature algorithm (lattice-based, derived from Dilithium) — the general-purpose PQC signature choice |
| **SLH-DSA (FIPS 205)** | A NIST-standardized post-quantum signature algorithm based on hash functions (derived from SPHINCS+) — more conservative, larger signatures |
| **HQC** | A NIST-selected backup post-quantum KEM, based on a different mathematical problem than ML-KEM, for cryptographic diversity |
| **NIST IR 8547** | NIST guidance on the transition timeline away from classical (pre-quantum) cryptographic algorithms |
| **CNSA 2.0** | The US National Security Agency's Commercial National Security Algorithm suite specifying PQC transition requirements/timelines |
| **Crypto-agility** | An architecture property: the ability to swap cryptographic algorithms without a system redesign — the central engineering goal of a PQC migration |
| **Hybrid mode** | Running a classical and a post-quantum algorithm together during the transition period, so security holds even if one is later broken |
| **KEM** | Key Encapsulation Mechanism — a cryptographic method for two parties to securely agree on a shared secret key |

## Quantum computing (background, low-priority track)

| Term | Meaning |
|---|---|
| **Qubit** | Quantum bit — the basic unit of quantum information, able to exist in superposition (unlike a classical 0/1 bit) |
| **Superposition / entanglement** | Core quantum-mechanical properties that quantum computers exploit for certain kinds of speedup |
| **Gate model vs. annealing** | Two different quantum computing paradigms — general-purpose programmable circuits vs. specialized optimization-focused hardware |
| **NISQ** | Noisy Intermediate-Scale Quantum — the current era of quantum hardware: real but error-prone and limited in scale |
| **Logical vs. physical qubits** | A "logical" (reliable) qubit is built from many noisy "physical" qubits via error correction — why raw qubit counts are a misleading headline metric |
| **QML** | Quantum Machine Learning — using quantum computers for ML tasks; still a research-stage field with no demonstrated production advantage for your domain |

---

## Two disambiguation pairs worth remembering

- **PQ vs. PQC** — Product Quantization (a vector-search compression technique) vs. Post-Quantum
  Cryptography (cryptography resistant to quantum attacks). Same initials in casual use, completely
  unrelated fields. Context always makes it obvious, but it trips people up in conversation.
- **SOTIF vs. functional safety (ISO 26262)** — functional safety is about things *breaking*
  (faults); SOTIF is about a system doing exactly what it was designed to do, and that turning out
  to be unsafe anyway due to a performance limitation or unforeseen scenario. This is the correct
  mental model for most AI/ML safety problems — see [07](07_Governance_Safety_Regulation.md) §4.
