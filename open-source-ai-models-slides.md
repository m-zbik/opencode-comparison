---
marp: true
theme: default
paginate: true
---

# Local Open-Source AI Models
## Why It's Worth the Experiment

*Security, Cost, and Quality Analysis*

---

# The Big Question

**Can we run AI models entirely on our own network -- with zero data leakage?**

**YES.**

- All major tools support fully air-gapped operation
- No prompts, code, or documents ever leave your infrastructure
- Source code is auditable (MIT / Apache 2.0 licensed)
- Quality now matches GPT-4o on coding benchmarks

---

# Slide Agenda

1. Security & Privacy (Why local matters)
2. Cloud AI Privacy Got Worse in 2025
3. Do These Tools Phone Home?
4. Model Comparison -- General Purpose
5. Model Comparison -- Coding
6. The Gap Has Nearly Closed
7. Cost Analysis
8. Quick Start Guide
9. Architecture for Air-Gapped Deployment
10. Risks to Manage
11. Scientific Evidence
12. Recommendation

---

# 1. Security: Local vs Cloud

| | Local Open-Source | Cloud AI |
|---|---|---|
| **Data leaves your org?** | Never | Always |
| **Fully air-gapped?** | Yes | Impossible |
| **Code auditable?** | Yes | No |
| **Trains on your data?** | Impossible | Risk varies |
| **GDPR/HIPAA/SOC2** | Simplified | Requires DPAs, BAAs |
| **Vendor lock-in** | None | High |

---

# 2. Cloud AI Privacy Got Worse in 2025

### OpenAI (August 2025)
All ChatGPT conversations now **scanned and monitored**
Escalated to human reviewers

### Anthropic (September 2025)
Users prompted to allow training on data
If accepted: retained up to **5 years**

### Google (August 2025)
Auto-enabled "Personal Content" for Gemini
**All chats train models** unless manually disabled

> With local models, **none of this applies**.

---

# 3. Do These Tools Phone Home?

| Tool | Sends Data? | Air-Gapped? | Open Source? |
|------|-------------|-------------|-------------|
| **llama.cpp** | **NO** -- zero networking code | Full | Yes (MIT) |
| **Ollama** | Registry only (no prompts) | Full* | Yes (MIT) |
| **vLLM** | **NO** | Full | Yes (Apache) |
| **LocalAI** | **NO** | Full | Yes (MIT) |
| **LM Studio** | Update checks only | Full* | **No** |

*After initial model download. Block registry via firewall for full isolation.

**llama.cpp is the gold standard** -- pure C/C++, no networking code whatsoever.

---

# 4. General-Purpose Models: Top Picks

| Model | Parameters | MMLU | RAM (Q4) | License |
|-------|-----------|------|----------|---------|
| DeepSeek R1 | 671B MoE | **90.8%** | ~350 GB | MIT |
| Llama 3.3 | 70B | ~86% | ~40 GB | Llama 3 |
| Qwen 2.5 | 72B | 86.1% | ~40 GB | Apache 2.0 |
| Phi-4 | **14B** | 84.8% | **~9 GB** | MIT |
| Mistral Small 3 | 24B | ~81% | ~14 GB | Apache 2.0 |

**Phi-4 (14B)** is remarkable -- 84.8% MMLU in only 14B params, runs on a laptop.

---

# 5. Coding Models: Top Picks

| Model | Parameters | HumanEval | RAM (Q4) |
|-------|-----------|-----------|----------|
| **Qwen2.5-Coder 32B** | 32B | **92.7%** | ~20 GB |
| DeepSeek-Coder V2 | 236B MoE | 90.2% | ~70 GB |
| Codestral | 22B | 86.6% | ~13 GB |
| **Qwen2.5-Coder 7B** | **7B** | **84.1%** | **~5 GB** |
| StarCoder 2 | 15B | 72.6% | ~9 GB |

**Qwen2.5-Coder 7B** -- 84.1% HumanEval at only 5 GB. Runs on any modern laptop.
**Qwen2.5-Coder 32B** -- 92.7% HumanEval. Matches GPT-4o.

---

# 6. The Gap Has Nearly Closed

| Benchmark | Best Open-Source | Best Closed-Source | Winner |
|-----------|-----------------|-------------------|--------|
| MMLU | DeepSeek R1: **90.8%** | GPT-4o: 88.7% | Open |
| MATH-500 | DeepSeek R1: **97.3%** | o1: 96.4% | Open |
| HumanEval | Qwen2.5-Coder: **92.7%** | GPT-4o: ~90% | Open |
| AIME 2024 | DeepSeek R1: **79.8%** | o1: 79.2% | Parity |
| SWE-bench | ~68% | ~80%+ | Closed |
| GPQA Diamond | 71.5% | 87.7% | Closed |

**Open-source leads on standard benchmarks. Closed-source leads on complex multi-step tasks.**

---

# 7. Cost: Hardware Tiers

| Tier | Hardware | One-Time Cost | Models | Monthly Power |
|------|----------|--------------|--------|--------------|
| Entry | MacBook M2 16GB | ~$1,500 | 7B | $5-10 |
| **Sweet Spot** | **Mac Mini M4 Pro 48GB** | **~$2,000** | **Up to 32B** | **$10-15** |
| Prosumer | Mac Studio M4 Max 128GB | ~$3,500-4,200 | Up to 70B | $10-15 |
| Enterprise | 2x RTX 4090 server | ~$5,500 | 70B | $65-130 |

**All software is 100% free** (Ollama, llama.cpp, vLLM, Continue.dev, Open WebUI)

---

# 7b. When Does Local Beat Cloud APIs?

| Usage | Cloud Cost/Month | Local Cost/Month | Break-Even |
|-------|-----------------|------------------|------------|
| Light | $5-50 | $10-15 + HW | Cloud wins |
| **Moderate** | **$50-750** | **$10-30 + HW** | **3-12 months** |
| **Heavy** | **$500-5,000+** | **$15-130 + HW** | **1-3 months** |

### Hidden savings with local:
- No per-token charges -- unlimited usage
- No compliance overhead (no DPAs, vendor audits)
- No API key management risk
- No vendor lock-in

---

# 7c. Quantization: Run Bigger Models on Less Hardware

| Quantization | Size Reduction | Quality Kept |
|-------------|---------------|-------------|
| Q8_0 | 50% smaller | ~99% |
| Q5_K_M | 66% smaller | ~97% |
| **Q4_K_M** | **72% smaller** | **~95%** |
| Q2_K | 84% smaller | ~80-85% |

A 70B model: 140 GB (full) --> **40 GB (Q4)** --> fits on a Mac Studio

> **Key insight**: 70B model at Q4 consistently beats 7B model at full precision.
> Always choose the biggest model your hardware can fit.

---

# 8. Quick Start: 10 Minutes

```bash
# Step 1: Install Ollama
brew install ollama                     # macOS
curl -fsSL https://ollama.ai/install.sh | sh   # Linux

# Step 2: Download a coding model
ollama pull qwen2.5-coder:7b           # 5 GB download

# Step 3: Use it
ollama run qwen2.5-coder:7b

# Step 4: Install Continue.dev in VS Code
#   (Extensions marketplace --> search "Continue")

# Step 5: (Optional) ChatGPT-like web UI
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

---

# 9. Air-Gapped Architecture

```
 [Developers / Applications]
          |
     [VPN / Firewall]           <-- No egress allowed
          |
     [Reverse Proxy + Auth]     <-- Nginx with mTLS
          |                         (Ollama has no built-in auth)
     [Inference Engine]         <-- Ollama / vLLM / llama.cpp
          |
     [Local Model Storage]     <-- Pre-loaded GGUF files
          |
     [GPU / Apple Silicon]     <-- Zero internet access
```

### Setup:
1. Download models on a connected machine
2. Transfer via secure media to air-gapped network
3. Run with firewall blocking all egress

---

# 10. Risks to Manage

| Risk | Severity | Mitigation |
|------|----------|-----------|
| Supply chain attacks (malicious model weights) | High | Use GGUF/SafeTensors only; verify SHA256 checksums |
| No built-in auth in Ollama | High | Always front with Nginx + auth proxy |
| CVEs in inference tools (3 RCE vulns in vLLM in 2025) | Medium | Active patching, container sandboxing |
| Prompt injection | Medium | Applies to ALL AI (local and cloud equally) |
| Lower quality on complex tasks | Low | Use local for routine; cloud for complex architecture |

**Local deployment shifts security burden to your team** -- but gives you full control.

---

# 11. Scientific Evidence

| Paper | Key Finding |
|-------|-------------|
| **DeepSeek-R1** (Nature, 2025) | Matches OpenAI o1 on reasoning; 90-95% cheaper |
| **Qwen2.5-Coder** (arXiv, 2024) | SOTA on code benchmarks; matches GPT-4o |
| **Llama 3** (Meta, 2024) | First open model rivaling top closed-source |
| **"Open Source Advantage in LLMs"** (arXiv, 2024) | Open-source is most robust path for LLM advancement |
| **DeepSeek-Coder-V2** (arXiv, 2024) | "Breaking the Barrier of Closed-Source Models" |
| **BigCodeBench** (ICLR, 2025) | 1,140 real engineering tasks benchmark |

---

# 12. Recommendation

### Start Small, Prove Value

**Week 1-2**: Install Ollama + Qwen2.5-Coder 7B on existing hardware
- Zero cost, zero risk, 10-minute setup
- Test with real coding tasks from your team

**Week 3-4**: Add IDE integration (Continue.dev) and web UI (Open WebUI)
- Measure developer satisfaction and productivity

**Month 2**: Evaluate dedicated hardware if results are positive
- Mac Mini M4 Pro 48GB (~$2,000) for team of 3-5
- Mac Studio M4 Max 128GB (~$4,000) for team of 5-10

**Month 3**: Production deployment with air-gapped architecture
- Nginx + auth, Docker isolation, no egress

---

# The Bottom Line

### Open-source AI models can run 100% on your internal network.

- **Zero data leakage** -- prompts never leave your infrastructure
- **Fully auditable** -- MIT/Apache licensed source code
- **Quality parity** -- matches GPT-4o on coding benchmarks
- **Free software** -- one-time hardware cost only
- **10-minute setup** -- brew install ollama && ollama pull qwen2.5-coder:7b

### The question is not "should we try this?"
### The question is "why haven't we started yet?"

---

# Appendix: Tool Comparison

| Tool | Type | GPU Required? | Best For |
|------|------|--------------|----------|
| **Ollama** | CLI + API | No | Daily dev, easiest setup |
| **llama.cpp** | C++ engine | No | Max control, air-gapped |
| **vLLM** | Python server | Yes (NVIDIA) | Production, high throughput |
| **LM Studio** | Desktop GUI | No | Beginners |
| **LocalAI** | Docker service | No | Multi-modal |
| **Open WebUI** | Web UI | No | ChatGPT-like interface |
| **Continue.dev** | VS Code ext | No | IDE integration |
| **OpenCode** | Terminal agent | No | CLI coding assistant |
| **Tabby** | Code completion | No | Self-hosted Copilot |

---

# Appendix: GitHub Stars & Maturity

| Repository | Stars | License | Actively Maintained |
|-----------|-------|---------|-------------------|
| ollama/ollama | ~163K | MIT | Yes (daily) |
| opencode-ai/opencode | ~100K | MIT | Yes |
| ggml-org/llama.cpp | ~95.8K | MIT | Yes (hourly releases) |
| vllm-project/vllm | ~71.2K | Apache 2.0 | Yes |
| cline/cline | ~58.4K | Apache 2.0 | Yes |
| mudler/LocalAI | ~42.9K | MIT | Yes |
| Aider-AI/aider | ~40.2K | Apache 2.0 | Yes |
| TabbyML/tabby | ~32.6K | Custom | Yes |
| continuedev/continue | ~31.5K | Apache 2.0 | Yes |

---

# Questions?

### Resources
- One-pager: `open-source-ai-models-one-pager.md`
- Full research: `open-source-ai-models-detailed-research.md`
- These slides: `open-source-ai-models-slides.md`

### Quick Start
```bash
brew install ollama
ollama pull qwen2.5-coder:7b
ollama run qwen2.5-coder:7b
```

*Research compiled March 2026*
