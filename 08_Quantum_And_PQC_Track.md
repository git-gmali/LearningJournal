# 08 — Quantum & Post-Quantum Track

> You asked specifically whether the roadmap should include quantum. Here is the honest answer,
> split into the part that is urgent and career-relevant **right now**, and the part that isn't.

---

## The short version

| Topic | Business relevance to you | Time allocation | Verdict |
|---|---|---|---|
| **Post-quantum cryptography (PQC) migration** | **Urgent, and directly in your domain** | 80% of this track | ✅ **Do it. Start in Month 4.** |
| Quantum computing / QML | 5–15 year horizon, no near-term Director-role demand | 20% of this track | 🟡 Informed observer only |
| Quantum sensing / annealing / quantum-inspired optimization | Niche, occasionally real | ~0% | ⬜ Awareness only |

**Total time budget: ~3 hours/month across 18 months (~55 hours).** Deliberately small.

**Do not** build a quantum-computing specialization to win an AI Director role. There is no
meaningful hiring demand for it in that market, and every hour spent there is an hour not spent
on inference economics or evals, which are being hired for *today*. But **do** own post-quantum
cryptography, because it is real, deadline-driven, and sits precisely on top of the vehicle PKI
and OTA-signing infrastructure you already own.

---

## PART A — Post-Quantum Cryptography (the part that matters)

### Why this is genuinely yours

Three facts that combine into an unusual opportunity:

1. **"Harvest now, decrypt later" is a present-tense threat.** Encrypted traffic captured today can
   be decrypted once a cryptographically-relevant quantum computer exists. Any data with a long
   confidentiality lifetime is already at risk.
2. **Vehicles live 15–20 years.** A vehicle produced today, with keys provisioned today, will still
   be on the road well past most credible quantum timelines. Automotive has one of the worst
   crypto-agility problems of any industry — and the longest exposure window.
3. **NIST finalized the standards in August 2024** — ML-KEM (FIPS 203), ML-DSA (FIPS 204), and
   SLH-DSA (FIPS 205), with HQC selected later as a backup KEM. NIST has published draft guidance
   proposing deprecation of classical RSA/ECC around 2030 and disallowing them around 2035.
   **Migration is a live program, not a research topic.**

And a fourth, personal one: your résumé says you explored *blockchain for PKI compliance* at
Stellantis. That was the right instinct — you correctly identified vehicle PKI as strategically
important — pointed at the wrong technology. **PQC is the actual answer to that question.**
Redirecting that exploration is a natural, defensible move and an excellent story: *"I looked at
this problem, concluded blockchain wasn't the answer, and led the crypto-agility work instead."*

### What to learn (~25 hours total)

**Fundamentals (6h)**
- What Shor's and Grover's algorithms actually break: Shor breaks RSA/ECC/DH (asymmetric
  cryptography — catastrophic); Grover halves effective symmetric key strength (AES-256 remains
  fine — manageable). **Know the difference cold** — most people conflate them.
- The lattice-based approach behind ML-KEM and ML-DSA, at a conceptual level
- Hash-based signatures (SLH-DSA) — conservative, large signatures, stateless
- Why signature and key sizes are the real engineering problem: PQC artifacts are substantially
  larger than ECC equivalents, which matters enormously over constrained vehicle links and
  in-vehicle networks

**The standards (5h)**
- FIPS 203 (ML-KEM / Kyber-derived) — key encapsulation
- FIPS 204 (ML-DSA / Dilithium-derived) — signatures, the general-purpose choice
- FIPS 205 (SLH-DSA / SPHINCS+-derived) — hash-based signatures for long-lived roots
- HQC as the backup KEM on a different mathematical assumption (diversification)
- NIST IR 8547 (transition guidance) and CNSA 2.0 timelines
- **Hybrid modes** (classical + PQC combined) — the transition strategy virtually everyone adopts,
  and the thing you'll actually deploy

**Migration engineering — the Director-level layer (10h)**
- **Cryptographic inventory / CBOM (cryptographic bill of materials)** — you cannot migrate what
  you cannot find. This is where every real program starts, and where most stall.
- **Crypto-agility as an architecture property**: algorithm negotiation, versioned key formats,
  abstracted crypto interfaces, the ability to rotate primitives without a redesign
- Certificate lifecycle and PKI hierarchy migration; root-of-trust transition
- Constrained-device reality: HSM/secure-element support for PQC, memory footprints, handshake
  sizes, latency on in-vehicle networks
- **OTA/secure-boot signature migration** — the hardest automotive-specific case, because the
  verifier is often in immutable hardware
- Hybrid deployment sequencing and rollback
- Cost and program planning for a multi-year migration across a shipped fleet

**Automotive specifics (4h)**
- Vehicle PKI architecture, V2X security, and PQC's impact on message sizes and certificate chains
- How a PQC migration interacts with **UNECE R155 (CSMS)** and **R156 (software update
  management)** — a crypto migration *is* a software-update-management problem under R156
- AUTOSAR crypto stack implications and secure-element/HSM constraints
- Industry positions from automotive cybersecurity bodies — worth tracking for where OEMs are heading

### The deliverable (Portfolio Project 6)

**A PQC crypto-agility assessment and migration framework for connected-vehicle infrastructure.**

Structure:
1. The threat model — harvest-now-decrypt-later against a 15-year asset lifetime
2. Cryptographic inventory methodology for a V2C platform (what to look for, where it hides)
3. Prioritization framework — which assets migrate first, and the criteria
4. Crypto-agility architecture patterns for constrained and immutable-verifier systems
5. The hybrid transition sequence
6. The R155/R156 regulatory interaction
7. Program plan shape: phases, dependencies, and the honest cost drivers

This is publishable, entirely non-confidential (it's a framework, not Stellantis data), and sits in
a gap where the intersection of qualified people is tiny: cryptography migration expertise ∩
automotive systems ∩ engineering leadership.

**Secondary benefit:** it is also a strong internal play. From your transcript, Stephan told you
the Director case needs *perimeter growth and visible impact on the whole connectivity ecosystem*.
A PQC readiness assessment for vehicle PKI is exactly that kind of perimeter — strategically
important, unowned, board-legible, and squarely inside your existing remit.

---

## PART B — Quantum computing (the watching brief)

### The honest assessment

Quantum computing is genuinely progressing — error-correction milestones have been real and the
logical-qubit trajectory is improving. But for the purpose of *"what makes me competitive for an
AI Director role in the next 3 years,"* it is not on the critical path:

- There is no established hiring demand for quantum skills in AI leadership roles
- Quantum machine learning remains largely a research programme; no production-relevant advantage
  has been demonstrated for the problems you would own
- The hardware timeline for cryptographically- or commercially-relevant machines remains uncertain,
  and estimates vary by an order of magnitude
- Anyone telling you quantum is an urgent career investment for an *AI platform* leader is selling
  something

**But** — you should not be ignorant of it, because:
- Executives ask about it, and "I've looked at it, here's my read, here's why we're not investing
  yet, and here's the trigger that would change that" is a *better* answer than enthusiasm
- The PQC work above requires you to understand the threat honestly
- If the timeline compresses, you want to have been paying attention, not starting from zero

### Minimum viable quantum literacy (~15 hours total, spread over 18 months)

- **Concepts (5h):** qubits, superposition, entanglement, interference, measurement. Gate model vs
  annealing. Why quantum speedups are *narrow* — Shor, Grover, and quantum simulation — not general.
- **The engineering reality (4h):** decoherence, error rates, physical vs logical qubits, error
  correction overhead, the NISQ era and its limits. **Understand why "number of qubits" is a
  misleading headline metric.** That single insight makes you a more useful executive than most.
- **Candidate applications (4h):** quantum chemistry and materials (the most credible near-term
  value — and relevant to automotive via **battery chemistry**, which is a genuine strategic hook
  for you), optimization (over-claimed; classical and quantum-inspired methods usually win today),
  QML (research stage).
- **Hands-on, optional (2h):** run a Bell state and Grover's algorithm on a free simulator (Qiskit
  or Cirq), once. Purely so your understanding is concrete rather than verbal.

**Do not:** take a multi-month quantum programming course, pursue a quantum certification, or
build a quantum portfolio project. The opportunity cost is too high.

### Adjacent ideas worth 2 hours of awareness

- **Quantum-inspired optimization** — tensor networks and annealing-inspired classical solvers.
  Occasionally genuinely useful for routing/scheduling/fleet optimization, and available today on
  classical hardware.
- **Quantum sensing** — arguably closer to commercial reality than computing; relevant to
  navigation and inertial sensing in GNSS-denied environments. Interesting for automotive; not
  your career path.

### Review triggers

Re-assess quantum priority if any of these happen:
- A credible demonstration of quantum advantage on a commercially relevant problem
- Fault-tolerant logical qubits reaching the hundreds with stable error rates
- Your employer or a target employer stands up a funded quantum program
- PQC migration deadlines move into regulated, mandatory territory for automotive (watch this one
  closely — it's the most likely trigger and it lands in *your* domain)

---

## How this track fits the calendar

| When | What | Hours |
|---|---|---|
| Month 4 | PQC fundamentals — Shor vs Grover, the standards landscape | 6 |
| Month 6 | FIPS 203/204/205, hybrid modes, NIST transition guidance | 5 |
| Months 8–10 | Migration engineering: inventory, crypto-agility, constrained devices | 10 |
| Month 11 | Automotive specifics: vehicle PKI, R155/R156 interaction | 4 |
| Months 12–14 | **Write and publish the PQC migration framework (Project 6)** | 12 |
| Spread across 18 months | Quantum literacy — one 45-minute session per month | 15 |
| **Total** | | **~52h** |

That's roughly 8% of your total roadmap hours, weighted almost entirely toward the part that has a
deadline attached to it.
