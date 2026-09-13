# 13 — Lab Setup Guide (Phase 0, detailed)

> Expands the "Set up the lab" checklist item in [02_Master_Roadmap](02_Master_Roadmap.md) Phase 0
> into concrete, do-this-now steps. Written for macOS + zsh (your environment). Budget: this is the
> ~10-hour Phase 0 allocation — a day-by-day schedule is at the bottom so it doesn't sprawl.
>
> **Security ground rule, before anything else:** every key you create in this guide goes into a
> local `.env` file or the macOS Keychain — **never into a file that gets committed to git.**
> Step 0 below sets up the guardrail before you have any keys to leak.

---

## Step 0 — Guardrail: `.gitignore` before any keys exist (5 min)

Your `LearningJournal` is already a git repo. Before creating a single API key, make sure secrets
can't accidentally get committed to it.

```bash
cd ~/Documents/AILearnings/LearningJournal
cat >> .gitignore <<'GITIGNORE'
# secrets
.env
.env.*
*.pem
*.key

# python
.venv/
__pycache__/
*.pyc

# jupyter
.ipynb_checkpoints/
GITIGNORE
git add .gitignore
git commit -m "Add .gitignore before adding any API keys"
```

From here on, every key lives in a `.env` file at the repo root, loaded at runtime — never typed
into code, never pasted into a notebook cell you might commit.

---

## Step 1 — Anthropic API key with billing (20 min)

**What this is, and why it's separate from what you're using right now:** your Claude.ai / Claude
Code subscription is a flat-fee product for chatting and coding. The **Anthropic Console** is a
separate, pay-per-token API product — this is what you need to *build* things (Projects 1–6:
agents, eval harnesses, the inference router). They use different billing.

1. Go to **console.anthropic.com** and sign in (same account as Claude.ai works, or create one).
2. **Settings → Billing** → add a payment method and load a small initial credit balance
   (start with $20–25 — this covers most of Month 1's experimentation).
3. **Settings → Limits** → set a **monthly spend cap**. This is the single most important click in
   this whole guide — it's what stops a runaway agent loop (Tier 1 T1.5 failure mode) from
   becoming a surprise bill. Set it to your monthly learning budget (₹5–9k / ~$60–100).
4. **Settings → API Keys → Create Key.** Name it something you'll recognise later
   (e.g. `learning-lab-2026`). Copy it immediately — it's shown once.
5. Store it locally:
   ```bash
   cd ~/Documents/AILearnings/LearningJournal
   echo 'ANTHROPIC_API_KEY=sk-ant-...' >> .env
   ```
6. **Verify it works** (needs Step 2's Python env, or just curl for a fast check):
   ```bash
   curl https://api.anthropic.com/v1/messages \
     -H "x-api-key: $ANTHROPIC_API_KEY" \
     -H "anthropic-version: 2023-06-01" \
     -H "content-type: application/json" \
     -d '{"model":"claude-sonnet-4-5","max_tokens":100,"messages":[{"role":"user","content":"Say hello in one sentence."}]}'
   ```
   A JSON response with a `content` field = you're live.

### Pick one comparison provider

You need a second provider so you can compare cost/quality (T1.2 decision framework, evals in
T1.6) and because interviewers will assume multi-provider fluency.

| Choice | Why pick this one | Setup |
|---|---|---|
| **Google Gemini (recommended to start)** | Generous free tier in **Google AI Studio** — you can experiment for weeks before spending anything. Lowest-friction second provider. | Go to **aistudio.google.com** → "Get API key" → create key → `echo 'GOOGLE_API_KEY=...' >> .env` |
| **OpenAI** | Most commonly referenced in job interviews and in the wider ecosystem's docs/tutorials. Worth adding by Month 2 even if you start with Gemini. | Go to **platform.openai.com** → Billing → add payment + spend cap → API keys → create → `echo 'OPENAI_API_KEY=...' >> .env` |

**Recommendation:** start with Gemini (free tier removes budget anxiety in Week 1), add OpenAI in
Month 2 once you're doing comparison work for real (Project 2's eval harness benefits from judging
across providers).

---

## Step 2 — Local Python environment with `uv` (15 min)

`uv` is a fast, modern Python package/env manager (single static binary, replaces
pip+venv+pyenv for most purposes). It matches your instinct for simple, low-overhead tooling.

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh
# Restart the shell or: source $HOME/.local/bin/env
uv --version
```

Set up the project environment inside your learning journal repo:

```bash
cd ~/Documents/AILearnings/LearningJournal
uv init --no-workspace lab
cd lab
uv add anthropic openai google-genai python-dotenv jupyterlab ipykernel
```

This creates a `.venv` (already gitignored from Step 0), a `pyproject.toml`, and a lockfile — pin
your dependencies from day one, the same discipline you'd expect in a production repo.

**Smoke test** (`lab/hello.py`):
```python
import os
from dotenv import load_dotenv
from anthropic import Anthropic

load_dotenv("../.env")
client = Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])
msg = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=100,
    messages=[{"role": "user", "content": "Say hello in one sentence."}],
)
print(msg.content[0].text)
```
```bash
uv run python hello.py
```

### Ollama — local small models

This is what you'll use for the edge/quantization work in [05](05_Edge_And_Physical_AI.md) and
for free, offline experimentation generally.

```bash
brew install ollama
brew services start ollama        # runs as a background service on localhost:11434
ollama pull llama3.2               # ~2GB, good general small model
ollama pull qwen2.5:7b             # strong for its size, good comparison point
ollama run llama3.2 "Explain KV cache in two sentences."
```

Ollama exposes an **OpenAI-compatible endpoint** at `http://localhost:11434/v1`, so you can hit it
with the same `openai` Python client you already installed — useful for the model-routing work in
[04](04_AI_Platform_And_Inference_Economics.md) §3.

### LM Studio — optional, GUI alternative

Use this when you want to *browse and compare* models visually (quantization levels, model cards,
a chat UI) rather than script against them. Download from **lmstudio.ai**, install, use its model
browser to pull a couple of quantized models, and try its built-in chat.

**Recommendation:** Ollama is your daily driver (scriptable, matches how you'll actually build
Projects 1–6). Install LM Studio too, but treat it as a browsing/comparison tool, not the primary
path — don't let exploring two tools eat the Phase 0 budget.

---

## Step 3 — A GPU path (30–45 min, pick ONE for now)

You need this for: quantization experiments, running vLLM/SGLang benchmarks
([04](04_AI_Platform_And_Inference_Economics.md) §7), and distillation work
([05](05_Edge_And_Physical_AI.md)). You do **not** need it for Month 1 — Ollama on CPU covers early
work. Set it up now so it's not a blocker when Month 6 arrives.

| Option | Model | Best for | Setup effort |
|---|---|---|---|
| **Google Colab Pro** | ~$10–12/mo flat | Notebook-first learning, quick experiments, the from-scratch transformer build in Month 1 | Lowest — just a browser |
| **Modal (recommended)** | Pay-per-second, generous free credits to start | Scripted, config-driven GPU jobs — matches how you actually work (Kafka/config-driven instincts). Best fit for Projects 3 & the vLLM benchmarking work. | Low — CLI + Python decorators |
| **RunPod** | Pay-per-hour, marketplace pricing | Renting a specific GPU (e.g. for a sustained vLLM serving benchmark) | Medium — more manual instance management |
| **Lambda (Cloud)** | Pay-per-hour, on-demand instances | Similar to RunPod; sometimes better GPU availability | Medium |

**Recommendation: Modal for the primary path**, because it's serverless (no idle-instance cost
anxiety), Python-native, and the "define a function, decorate it, it runs on a GPU in the cloud"
model is a natural fit for someone who thinks in config-driven architectures. Add Colab Pro too —
it's cheap and best for the Month 1 notebook-based transformer build.

### Modal quick start
```bash
uv add modal
uv run modal setup          # opens browser, links your account, creates token
```
Create `hello_gpu.py`:
```python
import modal

app = modal.App("hello-gpu")
image = modal.Image.debian_slim().pip_install("torch")

@app.function(gpu="T4", image=image)
def hello():
    import torch
    return f"CUDA available: {torch.cuda.is_available()}, device: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'none'}"

@app.local_entrypoint()
def main():
    print(hello.remote())
```
```bash
uv run modal run hello_gpu.py
```
Seeing `CUDA available: True` and a GPU name printed back = your GPU path works.

### Colab Pro quick start (if you pick this instead, or in addition)
1. Go to **colab.research.google.com** → Settings (gear icon) → sign up for Colab Pro.
2. New notebook → Runtime → Change runtime type → **GPU (T4/A100 depending on plan)**.
3. Hello-world cell:
   ```python
   import torch
   print(torch.cuda.is_available(), torch.cuda.get_device_name(0))
   ```
4. Store your `ANTHROPIC_API_KEY` etc. as Colab **Secrets** (key icon in left sidebar), not
   hardcoded in the notebook.

**Defer RunPod/Lambda** until Month 6, when you're specifically renting a GPU for a sustained
vLLM/SGLang serving benchmark ([04](04_AI_Platform_And_Inference_Economics.md) §7) — Modal covers
everything before that.

---

## Step 4 — A code agent in your daily loop (10 min — you're already most of the way there)

You're reading this from inside Claude Code, so the core piece is already running. Two things to
do deliberately, not just passively use it:

1. **Point it at your actual work from Week 1**, not just this roadmap. Use it for real Stellantis
   engineering tasks, for drafting the articles in [09](09_Portfolio_Projects.md), and for the
   Portfolio Project code itself. The goal is to build the habit *and* start generating the
   before/after productivity data you'll need for the honest measurement work in
   [06](06_AI_Leadership_And_Org_Design.md) §2.1 — track your own cycle time informally from now,
   even before you build formal instrumentation.
2. **Add Cursor as a complementary IDE-native option**, useful when you want inline-diff editing
   inside a full editor rather than a terminal-first flow. Download from **cursor.com**, sign in,
   connect your Anthropic/OpenAI keys under Settings → Models (or use its built-in models).
   Use it for the more exploratory, click-around parts of Projects 1–6; keep Claude Code for
   anything terminal/repo/multi-file-workflow shaped.

**Log it:** from Week 1, add one line per day to your learning journal (Step 6 below) noting one
thing the agent did that saved real time, and one thing it got wrong. This becomes primary source
material for your own "AI productivity, measured honestly" story later.

---

## Step 5 — Public GitHub presence (10 min)

Separate from any work GitHub account.

1. Create (or repurpose) a **personal** GitHub account — `github.com`, use your personal email.
2. Set a profile README with a one-line bio pointing at what you're building
   (update this again properly in Phase 4 — see [11](11_Positioning_And_Job_Search.md)).
3. Make sure `LearningJournal` — or a public subset of it, once articles/projects exist — has a
   home here. You don't need to make the whole roadmap public; you do need your **portfolio
   projects** ([09](09_Portfolio_Projects.md)) to live somewhere a recruiter can click through to.

---

## Step 6 — Learning journal discipline (5 min to set up, then ongoing)

You already have the repo (`LearningJournal`, currently holding this roadmap). Add one file that
makes the daily habit frictionless:

```bash
mkdir -p journal
cat > journal/$(date +%Y-%m).md <<'JOURNAL'
# Journal — YYYY-MM

## YYYY-MM-DD
- 

JOURNAL
```

**Rule: 5 lines minimum per session, same day, no exceptions.** What you did, one number you
measured, one thing that surprised you. This is the raw material [09](09_Portfolio_Projects.md)'s
articles get written from — writing them cold at the end of a month is much harder than compiling
five months of five-line entries.

---

## Step 7 — Thesis and baseline (15 min)

- [ ] Write the one-page thesis directly into `12_Progress_Tracker.md` (template already there).
- [ ] Score yourself honestly against the table in [01_Gap_Assessment](01_Gap_Assessment.md) §2,
      date it, and paste the scores into the Quarterly Re-scoring table in
      [12_Progress_Tracker](12_Progress_Tracker.md).

---

## Budget summary

| Item | Monthly cost | Notes |
|---|---|---|
| Anthropic API | ~$30–50 | Spend-capped in Step 1 |
| Gemini (free tier) | $0 to start | Add paid tier only if you outgrow free quota |
| OpenAI API | ~$10–20 (from Month 2) | Spend-capped |
| Modal | Pay-per-second; free credits cover Month 1–2 | Budget ~$10–15/mo from Month 3 |
| Colab Pro | ~$12 | Optional if Modal alone covers your needs |
| Ollama, LM Studio, uv, GitHub, Cursor free tier | $0 | Local/free |
| **Total** | **~₹5,000–9,000 ($60–100)/month** | Matches the budget already set in [02](02_Master_Roadmap.md) and [10](10_Credentials_And_Resources.md) |

---

## Day-by-day schedule (fits the ~10h Phase 0 budget)

| Day | Task | Time |
|---|---|---|
| 1 | Step 0 (.gitignore) + Step 1 (Anthropic key + spend cap + verify) | 45 min |
| 2 | Step 1 continued (Gemini key) + Step 2 (uv + Python env + smoke test) | 1 h |
| 3 | Step 2 continued (Ollama install + pull 2 models + first local prompt) | 1 h |
| 4–5 | Step 3 (Modal setup + hello-GPU job running; or Colab Pro if you prefer notebooks first) | 1.5 h |
| 6 | Step 4 (Cursor setup, first real task run through Claude Code deliberately) | 45 min |
| 7 | Step 5 (GitHub) + Step 6 (journal skeleton) + Step 7 (thesis + baseline scoring) | 1.5 h |
| — | Buffer for anything that didn't install cleanly the first time (it never does) | 2–3 h |

**Exit check for Phase 0:** you can run one command that calls Anthropic, one that calls a local
Ollama model, and one that runs on a rented GPU via Modal — all three from the same repo, all keys
loaded from `.env`, nothing secret in git history. That's the lab. Phase 1 starts the moment this
is true.
