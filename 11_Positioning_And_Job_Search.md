# 11 — Positioning & Job Search

> Do the résumé surgery in **Phase 4 (Month 13+)**, not now. Rewriting your positioning before the
> evidence exists produces claims you can't defend, which is worse than an outdated résumé. Read
> this early to know what you're building toward; execute it late.

---

## 1. The positioning statement

**One sentence, and everything else supports it:**

> *"I lead AI platforms where the output moves physical things — and can't be wrong."*

**The three-part proof:**
1. **Scale & systems** — 22 years, V2C platform at ~15M vehicles, 0→40+ engineer org
2. **AI platform depth** — inference economics, evals, agents, edge/cloud hybrid inference
3. **Assurance** — safety-critical and regulated delivery; AI governance that isn't theatre

**What you are explicitly *not* claiming:** that you train foundation models, that you're a
researcher, or that you've run a large ML research org. Own the boundary confidently. A leader who
knows exactly where their expertise ends reads as more senior, not less.

---

## 2. Résumé surgery (Month 13)

Your `Gajendra_Mali_AIEra.docx` is already a strong structural rewrite — the "decide what to build /
own outcomes / make AI produce them" frame is good, and the AI-Native Delivery Leadership section is
the right idea. The problem is that several claims are currently thin. After Phase 1–3, they won't be.

### Fix these specific lines

| Current | Problem | Replace with (after the work exists) |
|---|---|---|
| `AI/ML & Data Science: TensorFlow, scikit-learn, Pandas, NumPy \| CNN, RNN, LSTM, GRU architectures` | **Reads as "stopped learning in 2019."** The single most damaging line on the résumé for an AI role. | `AI/ML Platform: LLM inference optimization (vLLM, quantization, KV-cache economics) · RAG & context engineering · Agentic systems & MCP · Evaluation frameworks & LLM-as-judge · Model routing & cost engineering · Edge inference & distillation · Classical ML: time-series forecasting, anomaly detection` |
| `Apply AI/agentic workflows ... to multiply team throughput` | Unsupported claim from a non-AI background — invites a question you currently can't answer for 20 minutes. | Same claim, plus one measured result and a named artifact. Claims become safe once evidence exists. |
| `Architected the Hybrid Inference Engine (HIE)` (one line) | **Your most valuable line, buried.** | Expand to 2–3 lines and connect it explicitly to modern hybrid edge/cloud inference. See [05](05_Edge_And_Physical_AI.md) §5. |
| `blockchain exploration for PKI compliance` | Dated; reads as a technology-fashion decision. | `Led post-quantum cryptography readiness assessment for vehicle PKI and OTA signing — crypto-agility architecture under a 15-year asset lifetime.` (After Project 6.) |
| `Leading an AI cost-optimization initiative ... backed at SVP and CTO level` | Good, but no outcome and no method. | Add the method and a defensible number. This is a headline achievement — treat it like one. |

### Structural changes
- **Lead with an AI Platform Leadership section**, not "Executive Summary." The first 15 lines
  decide whether you're an AI candidate or an automotive candidate.
- **Add a "Selected Work" block** with 3 links: signature project, eval framework, one published
  article. Links on a Director résumé are unusual and they get clicked.
- **Compress the pre-2012 history** to two lines — the AI-era résumé already does this well, keep it.
- **Keep the metrics table.** It's genuinely strong and unusual. Add two AI-specific rows.
- **Keep both education credentials prominent.** They pass the formal-grounding filter instantly
  and they matter for visa/immigration processes in Europe and the Middle East.

### Keep a résumé per archetype
Maintain three versions (you already have a `tailored/` habit — extend it):
- **AI Platform / Infrastructure Director** — lead with inference economics and the gateway
- **AI-native VP Engineering** — lead with org scale, delivery outcomes, and adoption measurement
- **SDV / Edge AI leadership** — lead with automotive depth and the hybrid inference work

---

## 3. LinkedIn

Your `LinkedIn_Update_Sheet.md` already has the right structure. Two updates for the AI-platform framing:

**Headline (evolve Option D):**
```
Engineering Leader — AI Platforms for Physical Systems | SDV & Vehicle-to-Cloud at 15M-vehicle scale
| Edge/Hybrid Inference · AI Cost Engineering · Evals & AI Governance (ISO 21434 · EU AI Act) | 22 yrs
```

**Highest-priority fixes from your existing sheet, still outstanding:**
- [ ] The Stellantis experience section is **empty**. Fill it. This is the biggest single gap and
      it costs you recruiter searches every week it stays blank.
- [ ] Update "~19 years" → "22+ years"
- [ ] Remove the hashtag clutter

**Then, from Month 6:** post every article you publish. Not reposts, not commentary — your own work.
Ten to twelve original technical posts over 18 months will do more for inbound opportunity than any
amount of profile optimization.

---

## 4. Target company archetypes

| Archetype | Why you fit | Example shapes |
|---|---|---|
| **OEM / Tier-1 SDV organizations** | Native domain. Every OEM is standing up SDV + AI platform functions and is short of leaders who understand both. | European and US OEMs, major Tier-1 suppliers, EV-native manufacturers |
| **Automotive software & connectivity platform companies** | Your Harman/GSDP history is directly relevant | Connected-services platforms, telematics providers, OTA/software-defined-vehicle vendors |
| **Robotics & industrial autonomy scale-ups** | Same problem shape: real-time, safety-relevant, fleet-scale, edge inference. Actively hiring from automotive. | Warehouse/logistics robotics, industrial autonomy, drone platforms, AV companies |
| **Enterprise AI platform roles at large non-tech companies** | Your platform + governance + cost combination is exactly what they need, and they are less likely to filter on ML research pedigree | Industrials, energy, logistics, manufacturing, large financial institutions |
| **AI infrastructure vendors** | Inference economics and enterprise credibility; you'd be a field/solutions-side or platform leader | Inference platforms, MLOps/eval tooling, edge-AI hardware and software |
| **Middle East giga-projects & national programs** | Explicitly on your relocation list; they hire for scale and breadth, pay well, and value the multi-region delivery record | Saudi/UAE mobility, smart-city and national AI programs |

**Geography note:** your existing materials say open to Europe, US, and the Middle East. Be aware
that US Director roles at this level increasingly filter on prior US work authorization, while
Europe (especially Germany, France, Netherlands, Sweden — all automotive hubs) and the Middle East
are structurally more accessible from India with your profile. Prioritize accordingly.

---

## 5. The AI Director interview loop

Typical stages and how to prepare:

| Stage | What they're testing | Your preparation |
|---|---|---|
| **Recruiter screen** | Is this an AI person or an infra person with AI vocabulary? | A 90-second narrative ending in a specific AI outcome you own |
| **Hiring manager** | Judgment and scope | The build/buy framework, the cost story, one deep technical artifact |
| **AI system design** | Can you architect an AI system end to end? | Practice: design a RAG system, an agent platform, an inference serving tier, an eval pipeline. Always cover cost, latency, quality, and failure. |
| **Technical depth** | Are the claims real? | Projects 1–3. Be able to go deep on the thing you actually built. |
| **Org/leadership** | Can you build and run the team? | Team topology, hiring, adoption measurement, the AI career path you built |
| **Executive / VP** | Can you own a budget and speak to the board? | The SVP/CTO backing story, the counterfactual method, the governance posture |

### The questions to have polished answers for
1. *"How do you know your AI system is working?"* → evals, judge calibration, online metrics. **Most
   candidates fail this. It should be your strongest answer.**
2. *"What does your AI cost, and how did you reduce it?"* → the lever stack + cost per successful task
3. *"Tell me about an AI project that failed."* → have a real one with an honest root cause
4. *"Build or buy?"* → the framework, then apply it live to their actual situation
5. *"How do you structure an AI team?"* → platform + embedded, with the charter
6. *"How do you handle AI risk?"* → risk tiering, architectural containment, human-accountability
   boundary, regulatory posture. **Your differentiator — make sure this question gets asked.**
7. *"Why you, given you haven't run an ML research org?"* → the boundary statement from §1. Own it.
8. *"What are you reading / what changed your mind recently?"* → have something specific and recent

### Questions to ask them (these signal seniority)
- "How do you currently measure whether an AI feature is working?"
- "What's your AI spend, and who owns it?"
- "Where does AI capability live — central team or embedded?"
- "What's your risk classification process, and how long does it take a team to get through it?"
- "What's the most expensive AI mistake you've made so far?"

---

## 6. Networking — the channel that actually converts

At Director+ most roles are filled through referral and search, not applications. Applications
convert at low single-digit percentages; warm referrals convert an order of magnitude better.

**The cadence (from Month 6):**
- **4 conversations/month.** Ex-Harman colleagues now at OEMs and Tier-1s, IIITB and LJMU alumni in
  AI roles, people who publish work you found useful, SDV community contacts.
- **Lead with the work, not the ask.** "I wrote this about hybrid inference and thought of your
  team" outperforms "I'm exploring opportunities" by a wide margin.
- **Executive search firms:** build relationships with 3–4 recruiters who specialize in automotive
  technology leadership or AI leadership. They place these roles. Do this from Month 12.
- **Keep a simple CRM** — a spreadsheet with name, org, last contact, and next step. 100+ warm
  contacts by Month 18 is a realistic and sufficient target.

---

## 7. The parallel internal track

Don't let the external roadmap crowd out the Stellantis Director case — they feed each other.

From your conversation with Stephan, the internal case needs three things:
1. **Perimeter growth** — he said explicitly: each time there's an opportunity, consolidate and grow.
   → *Claim the AI platform capability for SDP. It's unowned and it's strategically legible.*
2. **Evidence of whole-ecosystem impact** — expected to be clear after the migration
   → *Your AI enablement operating model spans the whole org. That's ecosystem impact you control
   the timeline on, unlike the migration.*
3. **The technical + functional dual-impact argument** — he named this as the specific justification
   that distinguishes your group from the rest of PDT
   → *Nothing demonstrates it better than an AI platform capability you personally architected
   and then led an org to adopt.*

**And be honest about the constraint:** there's a director quota, people waiting two-plus years, and
job-family harmonization that hasn't landed. Stephan is genuine but he told you it will take time
and that he "failed this year." **Build the external option properly.** The strongest position is
having both — and the same evidence builds both, which is the whole design of this roadmap.

**Decision point: Month 18.** If the internal Director role is real and dated by then, take it with
a strengthened market alternative in hand. If it isn't, you'll have spent 18 months becoming
someone the external market wants — which was worth doing either way.
