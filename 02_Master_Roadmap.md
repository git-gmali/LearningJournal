# 02 — Master Roadmap: 18 Months

> Designed for **8–10 hours per week** alongside a demanding Principal Development Manager role.
> That is ~600 hours total. It is enough, but only if it's consistent. Two 4-hour blocks a week
> plus daily 20-minute reading beats one heroic Saturday.

---

## The shape of the plan

```
 Phase 0     Phase 1            Phase 2              Phase 3                 Phase 4
 Calibrate   LLM Fluency        Production AI        Platform + Moat         Market Entry
 Wk 1-2      Month 1-3          Month 4-6            Month 7-12              Month 13-18
 ─────────── ────────────────── ──────────────────── ─────────────────────── ──────────────────
 Baseline    Close the          Evals, RAG,          Inference economics,    Publish, speak,
 Lab setup   2019→2026 gap.     agents, guardrails.  edge/physical AI,       network, interview.
 Thesis      Hands-on daily.    Ship artifact #1.    governance. Ship        Convert to offer
                                                     signature artifact.     or internal Director.
 ───────────────────────────────────────────────────────────────────────────────────────────────
 Continuous: writing (2h/wk from Month 2) · PQC track (3h/month) · network (2 conversations/month)
```

**Rule: every phase ends with something that exists outside your head.** A repo, a document,
a talk, a measured result. Phases do not end because time passed.

---

## Weekly Operating System

This is the part that actually determines whether the plan works.

| Slot | When | Duration | What |
|---|---|---|---|
| **Deep Block A** | Sat or Sun morning | 3h | Hands-on build. Code, not reading. Phase project work. |
| **Deep Block B** | One weekday evening (protect it) | 2–2.5h | Structured study — course module, paper, syllabus item. |
| **Writing block** | Sunday evening | 1.5h | Write up what you learned. Ship publicly every 2 weeks. |
| **Daily drip** | Morning, before work | 20 min | Reading: one paper abstract, one newsletter, one doc page. |
| **Network slot** | Any weekday lunch, 2× per month | 45 min | One conversation with someone doing AI at the level you want. |

**Monthly (first Sunday, 1h):** review `12_Progress_Tracker.md`. Ask three questions:
1. What shipped? (If nothing shipped, the month failed regardless of hours logged.)
2. What did I learn that changes the plan?
3. Who now knows something about my work that didn't last month?

**Quarterly (half-day):** re-read `01_Gap_Assessment.md`, re-score yourself honestly, adjust.

### Guardrails on yourself

- **Reading is not progress.** Cap consumption at 40% of your hours. 60% must be building or writing.
- **No tutorial loops.** After the first month, every hands-on session should produce something
  that didn't exist, not reproduce something from a tutorial.
- **Spend your own money.** ~₹5,000–9,000/month ($60–100) on API credits and occasional GPU
  rental. From the transcript: waiting for corporate AI credits already stalls you for days at
  a time. Do not let your learning be rate-limited by a budget line you don't control.
- **When work gets brutal, drop to the floor, don't drop out.** Minimum viable week =
  the daily 20 minutes + one 2-hour block. Never a zero week.

---

## PHASE 0 — Calibrate (Weeks 1–2, ~10h)

**Goal: set up so that Phase 1 has no friction.**

- [ ] **Set up the lab** — step-by-step instructions, commands, and a day-by-day schedule are in
      [13_Lab_Setup_Guide](13_Lab_Setup_Guide.md). In short:
  - API keys with billing: Anthropic (Claude), plus one of OpenAI/Google for comparison work
  - Local: `uv` for Python env, Ollama or LM Studio for local small models
  - A GPU path: Google Colab Pro, Modal, RunPod, or Lambda — pick one, get a hello-world job running
  - A code agent in your daily loop (Claude Code / Cursor) — use it for real work from week 1
- [ ] Create a **public GitHub account presence** (you mentioned GitHub access as an open item
      with Stephan — this is your *personal* one, separate from work).
- [ ] Create a learning journal repo. Every session gets 5 lines. This becomes your writing source material.
- [ ] Write your **one-page thesis**: who you want to be in 18 months, in your own words, 300 words.
      Re-read it monthly. Draft in `12_Progress_Tracker.md`.
- [ ] Baseline self-score against the table in [01](01_Gap_Assessment.md). Date it.

---

## PHASE 1 — LLM & Agent Fluency (Months 1–3, ~100h)

**Goal: erase the 2019→2026 vocabulary gap, with hands on keyboard. By the end you can
whiteboard a transformer, reason about inference cost, and have built a working agent.**

### Month 1 — Transformers and the modern stack
- [ ] Work through the transformer from the inside out (see [03](03_Technical_Syllabus.md) T1.1–T1.3).
      Target: you can draw attention on a whiteboard and explain why the KV cache exists.
- [ ] Build: implement a minimal attention block + a tiny character-level transformer from
      scratch, once. Not to be good at it — to never be bluffing about it again.
- [ ] Run a local small model (Llama/Qwen/Gemma class) via Ollama. Quantize it. Measure
      tokens/sec at different quantization levels. **Write down the numbers.** This is your
      first real data.
- [ ] Read: "Attention Is All You Need", then a current architecture report (e.g. a recent
      open-weights model's technical report). Note what changed in 8 years.

### Month 2 — Building with LLMs properly
- [ ] Context engineering: structured prompting, few-shot vs zero-shot, output schemas /
      structured outputs, system-prompt design, prompt caching.
- [ ] Build a **RAG system end-to-end** over a real corpus you care about — use your own
      Stellantis-adjacent public domain (e.g. UNECE R155/R156 + ISO 21434 public summaries,
      or AUTOSAR docs). Chunking, embeddings, hybrid search, reranking.
- [ ] Break it deliberately. Document the failure modes. **The failure catalogue is worth more
      than the working demo** — that's the Director-level artifact.
- [ ] Start the writing habit: publish post #1. Suggested: *"What a 22-year systems engineer
      got wrong about RAG in the first month."* Honest, specific, technical.

### Month 3 — Agents and tools
- [ ] Agent fundamentals: tool/function calling, the ReAct-style loop, planning, memory,
      multi-turn state, when agents beat pipelines and when they don't.
- [ ] **MCP (Model Context Protocol)** — build your own MCP server exposing a tool. This is
      now the de facto integration standard; knowing it hands-on is current-stack signal.
- [ ] Build: an agent that does something genuinely useful in your actual job. Strong candidate —
      a **log/trace triage agent** over ELK data that proposes a root cause for an incident.
      (This becomes Portfolio Project 1.)
- [ ] Study agent failure modes: loops, tool misuse, context rot, cascading errors, cost blowups.
- [ ] Publish post #2: what you built, what it cost, where it failed.

**Phase 1 exit criteria:**
- ✅ A working agent you built, running against real data
- ✅ Two public write-ups
- ✅ You can explain KV cache, quantization tradeoffs, and RAG-vs-fine-tune without notes
- ✅ Measured numbers of your own (tokens/sec, cost/query, retrieval precision)

---

## PHASE 2 — Production AI Systems (Months 4–6, ~110h)

**Goal: move from "can build a demo" to "can run it in production and prove it works."
This is the phase that separates you from the 10,000 other leaders who did a prompt course.**

### Month 4 — Evals: the highest-leverage skill in the plan
- [ ] Learn eval design properly: offline vs online, golden datasets, LLM-as-judge (and its
      well-documented biases), pairwise comparison, rubric design, inter-rater agreement.
- [ ] Build an **eval harness** for your Phase 1 agent. Regression suite. CI-integrated.
- [ ] Learn the production quality loop: tracing (OpenTelemetry GenAI conventions), online
      feedback capture, drift detection, error taxonomy, triage workflow.
- [ ] Write the piece that will get shared: *"Evals are the unit tests of AI, and most teams
      don't have any."* Use your own harness as the worked example.

### Month 5 — Guardrails, security, and reliability
- [ ] OWASP Top 10 for LLM Applications — work through each, reproduce at least three attacks
      against your own agent (prompt injection, indirect injection via retrieved content, tool
      misuse/excessive agency).
- [ ] Build defence in depth: input/output validation, sandboxed tool execution, least-privilege
      tool scopes, human-in-the-loop gates, allow-lists for destructive actions.
- [ ] **The automotive-specific version:** design an agent that can propose but never
      autonomously execute a safety-relevant vehicle command. Document the control architecture.
      *This is a genuinely rare artifact and it's directly your domain.* (Portfolio Project 4.)
- [ ] Study real AI incidents and postmortems. Build an AI incident-response runbook.

### Month 6 — Inference economics (start the deep dive)
- [ ] Work through [04](04_AI_Platform_And_Inference_Economics.md) sections 1–4 fully.
- [ ] Do the GPU memory math by hand until it's intuitive: model weights + KV cache + activations;
      how batch size, context length and quantization move the numbers.
- [ ] Stand up vLLM (or SGLang) yourself. Benchmark: throughput vs latency vs batch size.
      Produce a curve. **Own that curve.**
- [ ] Model routing: build a small router that sends easy queries to a cheap/small model and
      hard ones to a frontier model. Measure the cost/quality frontier.
- [ ] Take your Stellantis AI cost-optimization program and write the **sanitized, portable
      version**: the methodology, the decision framework, the measured result, the counterfactual.
      No confidential numbers — the *framework* is the asset. (Portfolio Project 5.)

**Phase 2 exit criteria:**
- ✅ An eval harness in CI with real pass/fail gates
- ✅ A documented red-team exercise against your own system
- ✅ Your own inference benchmark curve with numbers you measured
- ✅ Four public write-ups total; at least one has been shared beyond your network
- ✅ A portable articulation of the AI cost program

---

## PHASE 3 — Platform Depth + The Moat (Months 7–12, ~200h)

**Goal: build the thing only you can build, and add the governance layer that makes you
memorable. This is where the roadmap stops being generic.**

### Months 7–9 — Edge & Physical AI (your differentiator)
- [ ] Work through [05](05_Edge_And_Physical_AI.md) completely.
- [ ] Small language models, distillation, quantization for edge (INT8/INT4, AWQ/GPTQ),
      pruning, hardware-aware deployment (NPU/DSP/automotive SoC classes).
- [ ] **Build Portfolio Project 3 — the Hybrid Edge-Cloud Inference Router.** On-device small
      model with confidence-based escalation to cloud. Produce the three-axis tradeoff surface:
      latency × cost × accuracy. This is the direct intellectual descendant of the Harman
      Hybrid Inference Engine, ten years on, and it is your signature piece.
- [ ] Multimodal and world models: VLMs, video understanding, simulation for training.
      Connect explicitly to your vehicle simulator work — synthetic data and sim-to-real are
      the same conversation you've been having for years, under a new name.
- [ ] Read the physical-AI/robotics-foundation-model literature enough to hold a view.
      The SDV → robotics talent flow is real and you're on the right side of it.

### Months 10–12 — Governance, assurance, and the leadership layer
- [ ] Work through [07](07_Governance_Safety_Regulation.md).
- [ ] EU AI Act: risk tiers, GPAI obligations, high-risk conformity assessment, and what it
      means operationally for an engineering org. **Verify the current timeline state before
      you quote it** — the schedule has been under active amendment.
- [ ] ISO/IEC 42001 (AI management systems) — study properly; decide on the Lead Implementer
      certification (see [10](10_Credentials_And_Resources.md)).
- [ ] NIST AI RMF; ISO/PAS 8800 (safety and AI in road vehicles); ISO 21448 SOTIF. Map these
      against the ISO 21434 / R155 world you already know. **Write the mapping.** Almost nobody
      has written a good one and it would be read widely.
- [ ] Work through [06](06_AI_Leadership_And_Org_Design.md): team topology, build/buy/fine-tune
      frameworks, AI adoption measurement, hiring AI engineers when you aren't one.
- [ ] Formalise the AI career-path/enablement work you already started for the SDP team into a
      publishable **AI-native engineering org operating model**.

**Phase 3 exit criteria:**
- ✅ Signature project public with a real write-up and measured results
- ✅ The safety-standards-to-AI-governance mapping published
- ✅ One conference talk submitted (accepted or not — submission is the milestone)
- ✅ Eight to ten public artifacts total
- ✅ You have a POV you can state in one sentence and defend for an hour

---

## PHASE 4 — Market Entry & Conversion (Months 13–18, ~160h)

**Goal: convert capability into an offer — external Director+, or the internal Director role,
on your terms.**

- [ ] **Résumé surgery** using [11](11_Positioning_And_Job_Search.md). This is now safe to do,
      because the claims are backed.
- [ ] LinkedIn rebuild (Option D headline from your existing sheet, updated for AI-platform framing).
- [ ] Speak: target 2 talks or podcasts. Automotive/SDV conferences, AI infra meetups, internal
      Stellantis forums that have external visibility.
- [ ] Network deliberately: 4 conversations/month. Target list in [11](11_Positioning_And_Job_Search.md).
      Warm referrals convert ~10× better than applications at Director+.
- [ ] Interview preparation specific to AI Director loops: system design for AI platforms,
      "tell me about an AI initiative you owned end to end," cost/ROI defence, org design,
      and the AI-governance question almost nobody prepares for.
- [ ] Run the internal track in parallel: package the migration evidence + your AI platform
      capability into the Director dossier Stephan needs. Give him ammunition, not a request.
- [ ] Decide by Month 18. Having two live options is the goal; the choice is the easy part.

---

## Time budget summary

| Phase | Duration | Hours | Cumulative |
|---|---|---|---|
| 0 — Calibrate | 2 weeks | 10 | 10 |
| 1 — LLM fluency | 3 months | 100 | 110 |
| 2 — Production AI | 3 months | 110 | 220 |
| 3 — Platform + moat | 6 months | 200 | 420 |
| 4 — Market entry | 6 months | 160 | 580 |
| Continuous (PQC, writing, network) | 18 months | ~90 | ~670 |

~670 hours over 78 weeks ≈ **8.6 hours/week**. Sustainable. Not easy.

---

## What to do if you fall behind

You will fall behind. Some months work will eat everything. The recovery rule:

1. **Never restart.** Resume from where you stopped, even if it's been six weeks.
2. **Cut scope, not phases.** Each phase has a "minimum shippable" core — the build artifact.
   Drop the reading, keep the build.
3. **Reset the ship date publicly.** Tell one person your new date. External commitment is the
   cheapest accountability mechanism that exists.
