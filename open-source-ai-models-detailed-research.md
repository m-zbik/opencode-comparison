# Open-Source AI Models: Detailed Research Report

---

## Table of Contents

1. [General-Purpose Model Comparison](#1-general-purpose-model-comparison)
2. [Coding-Focused Model Comparison](#2-coding-focused-model-comparison)
3. [Security & Privacy Analysis](#3-security--privacy-analysis)
4. [Cost Analysis](#4-cost-analysis)
5. [Installation & Setup Guide](#5-installation--setup-guide)
6. [Scientific Papers & Benchmarks](#6-scientific-papers--benchmarks)
7. [GitHub Ecosystem & Tools](#7-github-ecosystem--tools)
8. [Sources](#8-sources)

---

## 1. General-Purpose Model Comparison

### Overview Table

| Model | Sizes Available | Architecture | License | Context Window |
|-------|----------------|-------------|---------|---------------|
| **Llama 3.1/3.2/3.3** (Meta) | 1B, 3B, 8B, 70B, 405B (+ 11B/90B Vision) | Dense Transformer | Llama 3 Community License (commercial OK with restrictions) | 128K |
| **Mixtral / Mistral Small 3** (Mistral AI) | 7B, 8x7B (46.7B), 8x22B (141B), Small 3 (24B) | MoE (Mixtral); Dense (Small 3) | Apache 2.0 | 32K-128K |
| **Qwen 2.5** (Alibaba) | 0.5B, 1.5B, 3B, 7B, 14B, 32B, 72B | Dense Transformer | Apache 2.0 (most sizes) | 128K |
| **DeepSeek V3** (DeepSeek) | 671B total / 37B active | MoE (Multi-head Latent Attention) | MIT License | 128K |
| **DeepSeek R1** (DeepSeek) | 685B total / 37B active (+ distilled: 1.5B-70B) | MoE | MIT License | 128K |
| **Gemma 2** (Google) | 2B, 9B, 27B | Dense Transformer | Gemma Terms of Use (permissive) | 8K (base); up to 128K (Gemma 3) |
| **Phi-4** (Microsoft) | 14B | Dense Transformer | MIT License | 16K |
| **Command R+** (Cohere) | 104B | Dense Transformer | CC-BY-NC | 128K |
| **Yi 1.5** (01.AI) | 6B, 9B, 34B | Dense Transformer | Apache 2.0 | 4K-200K |
| **DBRX** (Databricks) | 132B total / 36B active | Fine-grained MoE | Databricks Open Model License | 32K |

### Benchmark Scores

| Model (flagship variant) | MMLU | HumanEval | MBPP | GPQA Diamond | MATH |
|--------------------------|------|-----------|------|-------------|------|
| **DeepSeek R1** | 90.8% | ~97% (Codeforces top 3.7%) | -- | 71.5% | 97.3% (MATH-500) |
| **DeepSeek V3** | 88.5% | 82.6% | 75.4% | 59.1% | -- |
| **Llama 3.1 405B** | ~87.3% | 89.0% | 78.8% | -- | -- |
| **Llama 3.3 70B** | ~86% | 80.5% | 75.4% | -- | -- |
| **Qwen 2.5 72B** | 86.1% | ~85%+ | -- | -- | ~83% |
| **Phi-4 (14B)** | 84.8% | 82.6% | -- | 56.1% | 80.4% |
| **Mistral Small 3 (24B)** | ~81% | ~88% | -- | -- | -- |
| **Mixtral 8x22B** | ~77% | ~70%+ | -- | -- | -- |
| **Yi 1.5 34B** | 77.1% | -- | -- | -- | -- |
| **Gemma 2 27B** | 75.2% | ~54% | -- | -- | -- |
| **Command R+ (104B)** | 75.7% | -- | -- | -- | -- |
| **Llama 3.1 8B** | 73.4% | 72.6% | 60.8% | -- | -- |
| **DBRX (132B MoE)** | 73.7% | 70.1% | -- | -- | -- |
| **Gemma 2 9B** | 71.3% | 40.2% | -- | -- | -- |

### RAM / VRAM Requirements

| Model Size | FP16 (full precision) | Q4 Quantized | Notes |
|------------|----------------------|-------------|-------|
| ~1-3B | 2-6 GB | 1-2 GB | Runs on phones, Raspberry Pi |
| ~7-9B | 14-18 GB | 5-7 GB | Single consumer GPU (RTX 3060/4060) |
| ~14B | ~28 GB | 8-9 GB | RTX 4090 (FP16) or RTX 3080 (Q4) |
| ~24-27B | 48-54 GB | 14-18 GB | RTX 4090 (Q4) or dual GPU |
| ~34B | ~68 GB | 20-24 GB | RTX 4090 or A6000 (Q4) |
| ~70-72B | ~140 GB | 35-42 GB | 2x RTX 4090 (Q4) or A100 80GB |
| ~132-141B MoE | ~270 GB | 70-80 GB | Multi-GPU setup required |
| ~405B | ~810 GB | ~200 GB | 8x A100 80GB or similar |
| ~671B MoE | ~1.3 TB (FP16) | ~350 GB (FP8) | 8x H200 (141GB each) |

### Key Takeaways

1. **DeepSeek R1** dominates reasoning and knowledge benchmarks (MMLU 90.8%, MATH-500 97.3%) but requires massive hardware for full-weight inference. Its MIT license and distilled variants (1.5B-70B) make it accessible at smaller scales.
2. **Qwen 2.5 72B** offers the best balance: strong MMLU (86.1%), excellent coding, 128K context, and sizes from 0.5B to 72B.
3. **Llama 3.3 70B** matches Llama 3.1 405B performance in a 70B package. The broadest community ecosystem.
4. **Phi-4 (14B)** punches far above its weight: 84.8% MMLU and 82.6% HumanEval in only 14B parameters under MIT license.
5. **Mistral Small 3 (24B)** is the sleeper pick at 81% MMLU with Apache 2.0 licensing.
6. **Command R+, Yi, DBRX** are increasingly eclipsed by the 2025 generation.

---

## 2. Coding-Focused Model Comparison

### Overview Table

| Model | Sizes | License | Context Window | Languages |
|-------|-------|---------|---------------|-----------|
| **Qwen2.5-Coder** (Alibaba) | 0.5B-32B | Apache 2.0 (most) | 128K | 92+ languages |
| **DeepSeek-Coder V2** | 16B, 236B (21B active) | DeepSeek License | 128K | 338 languages |
| **Codestral** (Mistral) | 22B | Non-Production License | 256K | 80+ languages |
| **CodeLlama** (Meta) | 7B, 13B, 34B, 70B | Llama 2 License | 100K | Multi-language |
| **StarCoder 2** (BigCode) | 3B, 7B, 15B | BigCode OpenRAIL-M | 16K | 600+ languages |
| **WizardCoder** | 15B, 33B | Varies | 16K | Multi-language |
| **Phind-CodeLlama** | 34B | Llama 2 License | 16K | Multi-language |

### Coding Benchmark Scores

| Model | HumanEval | MBPP | SWE-bench | Notes |
|-------|-----------|------|-----------|-------|
| **Qwen2.5-Coder 32B** | **92.7%** | ~80%+ | -- | Matches GPT-4o on HumanEval |
| **DeepSeek-Coder V2 236B** | 90.2% | 76.2% | 12.7% | First open model >10% SWE-bench |
| **Codestral 22B** | 86.6% | 80.2% | -- | FIM pass@1: 95.3% |
| **Qwen2.5-Coder 7B** | 84.1% | -- | -- | Beats Codestral at smaller size |
| **WizardCoder 33B** | 79.9% | 78.9% | -- | Fine-tuned on DeepSeek-Coder |
| **Phind-CodeLlama 34B v2** | 73.8% | -- | -- | Fine-tuned on 1.5B code tokens |
| **StarCoder 2 15B** | 72.6% | 75.2% | -- | Best for rare language support |
| **CodeLlama 70B** | 67.8% | 65.6% | -- | Legacy, superseded |

### Practical Quality Summary

| Model | Strengths | Weaknesses | Best Use Case |
|-------|-----------|------------|--------------|
| **Qwen2.5-Coder 32B** | Highest HumanEval; 128K context; excellent repair | ~22 GB Q4 VRAM | Best all-around coding model |
| **DeepSeek-Coder V2 236B** | 338 languages; 128K context; SWE-bench leader | Significant hardware needed | Complex multi-file tasks |
| **Codestral 22B** | 256K context (largest); very fast; 95.3% FIM | Non-production license | Code completion, scaffolding |
| **Qwen2.5-Coder 7B** | 84.1% HumanEval at only 7B; ~5 GB Q4 | Less robust on complex tasks | Local IDE assistant on modest hardware |
| **StarCoder 2 15B** | 600+ languages; transparent training data | Only 16K context; lower scores | Rare language support |

### Key Takeaways

1. **Qwen2.5-Coder 32B is the open-source coding champion** at 92.7% HumanEval. The 7B variant is exceptional for its size class.
2. **DeepSeek-Coder V2** remains best for repo-level, multi-file engineering tasks.
3. **Codestral 22B** has the longest context (256K) but its non-production license limits commercial use.
4. **CodeLlama, WizardCoder, Phind-CodeLlama** are now legacy models -- superseded by 2025-era models.
5. **The gap between open-source and closed-source coding models has nearly closed.**

---

## 3. Security & Privacy Analysis

### Data Privacy & Network Isolation

#### Can Open-Source Models Run Fully Air-Gapped?

**Yes, definitively.** All major tools support fully air-gapped operation once model files are pre-loaded:

| Tool | Air-Gapped Support | Telemetry Status | Open Source | License |
|------|-------------------|------------------|-------------|---------|
| **llama.cpp** | Full -- zero dependencies, no cloud calls | None known | Yes | MIT |
| **Ollama** | Full (after model download) | See details below | Yes | MIT |
| **vLLM** | Full (Docker/Podman-based) | None known | Yes | Apache 2.0 |
| **LocalAI** | Full (orchestration layer) | None known | Yes | MIT |
| **LM Studio** | Full (after model download) | Minimal (update checks) | **No** (closed source) | Proprietary |

#### Telemetry Analysis

**llama.cpp** is the gold standard for privacy. Pure C/C++ inference engine with no networking code beyond its optional HTTP server. No model registry, no update mechanism, no telemetry.

**Ollama** has faced community scrutiny ([GitHub issue #2567](https://github.com/ollama/ollama/issues/2567)):
- Contacts `registry.ollama.ai` when pulling or checking for model updates
- Users reported difficulty finding a clear privacy policy ([issue #11442](https://github.com/ollama/ollama/issues/11442))
- **No prompt or response data is sent** -- only model registry queries
- Recommendation: block outgoing connections to `registry.ollama.ai` via firewall
- In air-gapped mode with pre-downloaded models, no outbound connections needed

**vLLM** is fully open-source (Apache 2.0). Supports air-gapped deployment via Docker/Podman with Nginx reverse proxy. No known telemetry.

**LM Studio** has a published [privacy policy](https://lmstudio.ai/app-privacy). Prompts and conversations remain on-device. However, **it is closed-source** so claims cannot be independently verified.

### Enterprise Security Benefits

**Data Sovereignty**: All processing occurs within organizational infrastructure. Prompts, responses, documents never traverse external networks.

**Regulatory Compliance**:
- **GDPR**: No cross-border data transfer issues; simplified DPIAs
- **HIPAA**: No BAA needed with third-party AI provider; ePHI never leaves security perimeter
- **SOC 2**: Full control over security controls and audit trails
- **EU AI Act**: Full control over conformity assessments for high-risk systems

**No Third-Party Dependencies**:
- No Data Processing Addendums to negotiate
- No reliance on vendor promises about data handling
- No risk of vendor policy changes affecting compliance
- Complete control over data retention, deletion, and access

### What Changed in 2025: Cloud AI Privacy Got Worse

In August-September 2025, all three major providers reversed user privacy protections:

**OpenAI**:
- Introduced conversation scanning -- all ChatGPT chats monitored for "harmful content", escalated to human reviewers
- API data not used for training by default
- Consumer data may be used unless opted out
- Data retained for up to 30 days via API

**Anthropic** (September 2025):
- Updated terms prompting users to allow training data use
- If accepted: conversations used for training, retained up to **5 years**
- If declined: retention at approximately 30 days

**Google** (August 2025):
- Auto-enabled "Personal Content" for Gemini
- All chats and uploaded data contribute to training unless manually disabled
- Children's data: Google will train on data from teenagers who opt in

### Security Risks of Local Models

**Supply Chain Attacks (Most Critical)**:
- Python pickle-based serialization (.bin files) can execute arbitrary code during deserialization
- In 2025, malicious models using "nullifAI" evasion techniques bypassed detection
- Three critical zero-day vulnerabilities (CVSS 9.3) found in PickleScan
- **Mitigation**: Use SafeTensors or GGUF format only; verify SHA256 checksums; download from trusted sources only

**Known CVEs in Local AI Infrastructure (2025-2026)**:

vLLM:
- CVE-2026-22778 -- RCE via malicious video link (heap overflow)
- CVE-2025-62164 -- RCE via malicious prompt embeddings (CVSS 8.8)
- CVE-2025-66448 -- RCE via model config auto-mapping

Ollama:
- CVE-2025-63389 -- Missing authentication enabling unauthorized model management
- Four CVEs patched in v0.1.47: path traversal, denial-of-service, segfault

**Prompt Injection**: Running locally does NOT protect against prompt injection. OWASP LLM01:2025 ranks it as the #1 risk. Research shows just 5 crafted documents can manipulate AI responses 90% of the time.

### Network Architecture for Internal-Only Deployment

**Docker-Based Isolation**:
```yaml
# docker-compose.yml for air-gapped Ollama
version: '3.8'
services:
  ollama:
    image: ollama/ollama
    ports:
      - "127.0.0.1:11434:11434"  # Bind to localhost only
    volumes:
      - ./models:/root/.ollama/models
    networks:
      - airgapped
networks:
  airgapped:
    internal: true  # No external internet access
```

**Firewall Rules**:
```bash
# Block all incoming to Ollama port by default
sudo iptables -A INPUT -p tcp --dport 11434 -j DROP
# Allow only from localhost
sudo iptables -I INPUT -p tcp -s 127.0.0.1 --dport 11434 -j ACCEPT
# Allow from specific internal subnet
sudo iptables -I INPUT -p tcp -s 192.168.1.0/24 --dport 11434 -j ACCEPT
# Block ALL outbound from the Ollama process
sudo iptables -A OUTPUT -m owner --uid-owner ollama -j DROP
```

**Authentication Layer** (Critical -- Ollama has no built-in auth):
- Nginx/Caddy reverse proxy with basic auth or mTLS
- OAuth2 proxy for SSO integration
- VPN requirement for remote access
- API gateway (Kong, Traefik) for rate limiting

**Zero Egress Architecture**:
```
[Users/Applications]
        |
   [Firewall/VPN]
        |
   [Reverse Proxy + Auth]    <-- Nginx/Caddy with mTLS
        |
   [Load Balancer]           <-- For multiple inference nodes
        |
   [Inference Engine]        <-- Ollama/vLLM/llama.cpp
        |
   [Local Model Storage]     <-- Pre-loaded GGUF/SafeTensors files
        |
   [GPU Nodes]               <-- No outbound network access
```

---

## 4. Cost Analysis

### Hardware Tiers

| Tier | Hardware | Cost | Models You Can Run | Monthly Electricity |
|------|----------|------|--------------------|-------------------|
| **Entry** | MacBook M2/M3 16GB or RTX 3060 12GB | $1,000-1,500 | 7B Q4, basic 13B | $5-10 |
| **Mainstream** | Mac Mini M4 Pro 48GB or RTX 4090 system | $2,000-3,500 | 7B-33B comfortably | $15-65 |
| **Prosumer** | Mac Studio M4 Max 128GB or 2x RTX 4090 | $3,500-5,500 | 70B Q4-Q5 comfortably | $15-130 |
| **Enthusiast** | Mac Studio M3 Ultra 256-512GB | $5,500-14,000 | 70B Q8, 405B Q4 (slow) | $15-20 |
| **Enterprise** | 8x A100 or 4x H100 | $100K-200K+ | 405B FP8, DeepSeek 671B | $500-2,000+ |

### GPU Options and Prices (Early 2026)

| GPU | VRAM | Street Price | TDP | Best For |
|-----|------|-------------|-----|----------|
| RTX 3060 12GB | 12 GB | $200-250 (used) | 170W | 7B-13B on a budget |
| RTX 3090 | 24 GB | $800-1,300 (used) | 350W | 7B-33B, best price/VRAM |
| RTX 4090 | 24 GB | $2,200-2,755 | 450W | 7B-33B, best performance |
| RTX 5090 | 32 GB | $3,000-5,800 | 575W | Up to 70B Q4 |
| AMD RX 7900 XTX | 24 GB | $800-950 | 355W | Budget 24GB (Linux/ROCm) |
| NVIDIA A100 80GB | 80 GB | $12,000-15,000 | 300W | 70B FP16, enterprise |
| NVIDIA H100 80GB | 80 GB | ~$40,000 | 700W | 405B multi-GPU |
| Apple M4 Max (Mac Studio) | 128 GB unified | $3,499-4,199 (system) | ~75W | Silent, 70B Q4 |
| Apple M3 Ultra (Mac Studio) | Up to 512 GB | $3,999-14,099 (system) | ~115W | 405B, DeepSeek 671B |

**Note**: Apple Silicon's unified memory is uniquely suited for LLMs -- GPU and CPU share the same pool. A Mac Studio M3 Ultra 512GB can load models that would need multiple datacenter GPUs on NVIDIA, though at slower speeds.

### Electricity Costs (24/7 Full Load)

| GPU | Monthly kWh | Monthly Cost ($0.20/kWh) | Annual Cost |
|-----|-------------|--------------------------|------------|
| RTX 4090 | 324 kWh | $64.80 | $788 |
| 2x RTX 4090 | 648 kWh | $129.60 | $1,555 |
| Mac Studio M4 Max | 54 kWh | $10.80 | $130 |
| Mac Studio M3 Ultra | 82.8 kWh | $16.56 | $199 |

Realistic costs for on-demand inference are 30-60% lower (GPUs idle most of the time).

### Cloud vs Local Break-Even

| Usage Level | Cloud API Cost/Month | Local Cost/Month | Verdict |
|-------------|---------------------|------------------|---------|
| Light (<50K tokens/day) | $5-50 | $10-15 + amortized HW | Cloud is cheaper |
| Moderate (50K-500K/day) | $50-750 | $10-30 + amortized HW | **Local breaks even in 3-12 months** |
| Heavy (500K-2M+/day) | $500-5,000+ | $15-130 + amortized HW | **Local wins decisively (1-3 months)** |

**Privacy/compliance bonus**: Local hosting avoids compliance overhead and risk of cloud APIs -- significant indirect cost savings for HIPAA, PCI, GDPR.

### Quantization Impact

| Quantization | Size vs FP16 | Quality Retention | Best Use Case |
|-------------|-------------|-------------------|--------------|
| **Q8_0** | ~50% | ~99% | Abundant memory (128GB+ Mac) |
| **Q6_K** | ~40% | ~98% | High quality on 24GB GPUs |
| **Q5_K_M** | ~34% | ~97% | Quality-conscious, 16GB VRAM |
| **Q4_K_M** | ~28% | ~95% | **Best overall balance** (recommended) |
| **Q3_K_S** | ~22% | ~90% | Constrained devices |
| **Q2_K** | ~16% | ~80-85% | Last resort |

**Key insight**: Q4_K_M of a 70B model consistently outperforms FP16 of a 7B model. Bigger model at lower quantization beats smaller model at full precision.

### Software Stack: All Free

| Tool | License | Best For |
|------|---------|---------|
| **Ollama** | MIT | Beginners, quick setup |
| **llama.cpp** | MIT | CPU inference, edge devices, max control |
| **vLLM** | Apache 2.0 | Production serving, high concurrency |
| **SGLang** | Apache 2.0 | Structured generation, production |
| **LM Studio** | Free personal | Desktop GUI for beginners |

---

## 5. Installation & Setup Guide

### Ollama (Recommended Starting Point)

**Installation**:
```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.ai/install.sh | sh

# Windows: download from ollama.com
```

**Usage**:
```bash
ollama serve                          # Start server
ollama pull qwen2.5-coder:7b         # Download model
ollama run qwen2.5-coder:7b          # Run interactively
ollama list                           # List downloaded models
```

**API** at `http://localhost:11434`:
```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen2.5-coder:7b",
  "messages": [{"role": "user", "content": "Explain Docker"}],
  "stream": false
}'
```

**Recommended Models**:
| Model | Size | Best For |
|-------|------|----------|
| `qwen2.5-coder:7b` | ~4.5 GB | Best coding at 7B |
| `qwen2.5-coder:32b` | ~20 GB | Near-GPT-4o coding |
| `deepseek-r1:32b` | ~20 GB | Reasoning + coding |
| `llama3.1:8b` | ~4.7 GB | General purpose |

### llama.cpp (Maximum Control)

```bash
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
cmake -B build                        # Add -DGGML_CUDA=ON for NVIDIA
cmake --build build --config Release

# Run as API server
./build/bin/llama-server -m model.gguf --port 8080

# Or directly from HuggingFace
./build/bin/llama-server -hf ggml-org/gemma-3-1b-it-GGUF
```

### LM Studio (GUI for Beginners)

1. Download from [lmstudio.ai](https://lmstudio.ai/)
2. Use Discover tab to search and download models
3. Chat interface built-in
4. Local API server at `http://localhost:1234/v1`

### vLLM (Production Serving)

```bash
pip install vllm

# Serve with OpenAI-compatible API
vllm serve meta-llama/Llama-3.1-8B-Instruct

# Multi-GPU
vllm serve meta-llama/Llama-3.1-70B-Instruct --tensor-parallel-size 4

# Docker
docker run --runtime nvidia --gpus all \
  -v ~/.cache/huggingface:/root/.cache/huggingface \
  -p 8000:8000 --ipc=host \
  vllm/vllm-openai:latest \
  --model meta-llama/Llama-3.1-8B-Instruct
```

Requirements: Linux only, NVIDIA GPU (compute capability >= 7.0), CUDA 12.1+.

### LocalAI (Multi-Modal Local API)

```bash
# Docker (CPU)
docker run -d --name localai -p 8080:8080 \
  -v localai_models:/build/models localai/localai:latest-cpu

# Docker (NVIDIA GPU)
docker run -d --name localai -p 8080:8080 \
  -v localai_models:/build/models --gpus all \
  localai/localai:latest-gpu-nvidia-cuda-12
```

Supports: text, images (Stable Diffusion), audio (Whisper), TTS, embeddings.

### Open WebUI (ChatGPT-Like Interface)

```bash
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

Access at `http://localhost:3000`. Auto-detects Ollama models.

### Connecting to Coding Tools

**Continue.dev** (VS Code / JetBrains):
```json
{
  "models": [
    {
      "title": "Qwen 2.5 Coder 7B",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen 2.5 Coder 7B",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b"
  }
}
```

**OpenCode** (Terminal Agent):
```json
{
  "provider": "ollama",
  "model": "qwen2.5-coder:32b",
  "baseURL": "http://localhost:11434/v1"
}
```

**Any OpenAI-compatible tool**: Set `OPENAI_API_BASE` to:
| Tool | API URL |
|------|---------|
| Ollama | `http://localhost:11434/v1` |
| llama.cpp | `http://localhost:8080/v1` |
| LM Studio | `http://localhost:1234/v1` |
| vLLM | `http://localhost:8000/v1` |
| LocalAI | `http://localhost:8080/v1` |

---

## 6. Scientific Papers & Benchmarks

### Major Technical Reports

| Paper | Authors | Key Finding | Link |
|-------|---------|-------------|------|
| DeepSeek-V3 Technical Report | DeepSeek-AI | 671B MoE matches GPT-4o (MMLU 88.5%) | [arXiv:2412.19437](https://arxiv.org/abs/2412.19437) |
| DeepSeek-R1 | DeepSeek-AI | Matches OpenAI o1 via pure RL (MATH-500 97.3%) | [arXiv:2501.12948](https://arxiv.org/abs/2501.12948) |
| The Llama 3 Herd of Models | Meta AI | First open model rivaling top closed-source | [arXiv:2407.21783](https://arxiv.org/abs/2407.21783) |
| Qwen2.5 Technical Report | Alibaba | 72B competes with 405B at 5x smaller size | [arXiv:2412.15115](https://arxiv.org/abs/2412.15115) |
| Qwen2.5-Coder Technical Report | Alibaba | SOTA on EvalPlus, outperforming all open-source | [arXiv:2409.12186](https://arxiv.org/abs/2409.12186) |
| DeepSeek-Coder-V2 | DeepSeek-AI | "Breaking the Barrier of Closed-Source Models" | [arXiv:2406.11931](https://arxiv.org/abs/2406.11931) |

### Benchmark Papers

| Paper | Venue | Key Finding | Link |
|-------|-------|-------------|------|
| SWE-bench | ICLR 2024 Oral | Real-world GitHub issue resolution benchmark | [arXiv:2310.06770](https://arxiv.org/abs/2310.06770) |
| BigCodeBench | ICLR 2025 | 1,140 engineering tasks; best models reach ~60% vs 97% human | [arXiv:2406.15877](https://arxiv.org/abs/2406.15877) |
| LiveCodeBench | 2024 | Contamination-free eval; closed models generally lead | [arXiv:2403.07974](https://arxiv.org/abs/2403.07974) |
| HumanEval Pro / MBPP Pro | ACL 2025 | Self-invoking tasks show significant performance drops | [arXiv:2412.21199](https://arxiv.org/abs/2412.21199) |

### Meta-Analyses

| Paper | Key Finding | Link |
|-------|-------------|------|
| The Open Source Advantage in LLMs | Open-source is most robust path for advancing LLM research | [arXiv:2412.12004](https://arxiv.org/abs/2412.12004) |
| The Leaderboard Illusion | Systematic biases in Chatbot Arena favoring closed-source | [arXiv:2504.20879](https://arxiv.org/abs/2504.20879) |

### Where Open-Source Matches or Exceeds Closed-Source

| Domain | Evidence |
|--------|----------|
| Mathematics | DeepSeek-R1 matches o1 on AIME (79.8% vs 79.2%) and MATH-500 (97.3% vs 96.4%) |
| General Knowledge | DeepSeek-V3 (88.5%), Llama 3.1 405B (88.6%) match GPT-4o (88.7%) |
| Code Generation | Qwen2.5-Coder (92.7%) matches GPT-4o (~90%) |
| Cost Efficiency | DeepSeek-R1 is 90-95% cheaper than o1 with comparable performance |

### Where Open-Source Falls Short

| Domain | Evidence |
|--------|----------|
| Complex Software Engineering | SWE-bench: closed-source leads (~80% vs ~68%) |
| Advanced Reasoning (GPQA) | OpenAI o3 (87.7%) vs DeepSeek-R1 (71.5%) |
| Complex Instruction Following | BigCodeBench: closed-source leads overall |

---

## 7. GitHub Ecosystem & Tools

### Core Inference Engines

| Repository | Stars | License | Air-Gapped | Enterprise Ready |
|-----------|-------|---------|------------|-----------------|
| [ollama/ollama](https://github.com/ollama/ollama) | ~163K | MIT | Yes (full) | High |
| [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) | ~95.8K | MIT | Yes (native) | High |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | ~71.2K | Apache-2.0 | Yes (Docker) | Very High |
| [mudler/LocalAI](https://github.com/mudler/LocalAI) | ~42.9K | MIT | Yes (Docker) | Moderate-High |

### Coding-Focused Tools

| Repository | Stars | License | Air-Gapped | Enterprise Ready |
|-----------|-------|---------|------------|-----------------|
| [opencode-ai/opencode](https://github.com/opencode-ai/opencode) | ~100K | MIT | Yes (local endpoint) | High |
| [cline/cline](https://github.com/cline/cline) | ~58.4K | Apache-2.0 | Yes (local backend) | High |
| [Aider-AI/aider](https://github.com/Aider-AI/aider) | ~40.2K | Apache-2.0 | Yes (local endpoint) | Moderate-High |
| [TabbyML/tabby](https://github.com/TabbyML/tabby) | ~32.6K | Custom | Yes (official guide) | High |
| [continuedev/continue](https://github.com/continuedev/continue) | ~31.5K | Apache-2.0 | Yes (VSIX offline) | High |

### Enterprise Adoption Patterns

- **Government/Defense**: llama.cpp and Ollama in fully air-gapped environments. GGUF models transferred via secure media.
- **Financial Institutions**: vLLM for high-throughput serving with Kubernetes. LiteLLM as API gateway.
- **Healthcare (HIPAA)**: Ollama + Open WebUI with network isolation.
- **General Enterprise**: Continue + Ollama is the most common "local Copilot replacement" stack.

---

## 8. Sources

### Technical Reports
- [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437)
- [DeepSeek-R1](https://arxiv.org/abs/2501.12948) | [Nature](https://www.nature.com/articles/s41586-025-09422-z)
- [DeepSeek-Coder-V2](https://arxiv.org/abs/2406.11931)
- [The Llama 3 Herd of Models](https://arxiv.org/abs/2407.21783)
- [Qwen2.5 Technical Report](https://arxiv.org/abs/2412.15115)
- [Qwen2.5-Coder Technical Report](https://arxiv.org/abs/2409.12186)
- [Gemma 2 Paper](https://arxiv.org/html/2408.00118v1)
- [Phi-4 Technical Report](https://techcommunity.microsoft.com/blog/educatordeveloperblog/phi-4-small-language-models-that-pack-a-punch/4464167)

### Benchmark Papers
- [SWE-bench](https://arxiv.org/abs/2310.06770) | [SWE-bench Goes Live](https://arxiv.org/abs/2505.23419)
- [BigCodeBench](https://arxiv.org/abs/2406.15877)
- [LiveCodeBench](https://arxiv.org/abs/2403.07974)
- [HumanEval Pro / MBPP Pro](https://arxiv.org/abs/2412.21199)
- [The Open Source Advantage in LLMs](https://arxiv.org/abs/2412.12004)
- [The Leaderboard Illusion](https://arxiv.org/abs/2504.20879)

### Security & Privacy
- [Ollama Telemetry Discussion (GitHub #2567)](https://github.com/ollama/ollama/issues/2567)
- [Ollama Privacy Concerns (GitHub #11442)](https://github.com/ollama/ollama/issues/11442)
- [LM Studio Privacy Policy](https://lmstudio.ai/app-privacy)
- [OpenAI, Google & Anthropic Privacy Reversals (2025)](https://tvnewscheck.com/business/article/openai-google-anthropic-user-privacy-settings/)
- [Anthropic Privacy Shift](https://www.bitdefender.com/en-us/blog/hotforsecurity/anthropic-shifts-privacy-stance-lets-users-share-data-for-ai-training)
- [Malicious AI Models -- Supply Chain (ACM)](https://cacm.acm.org/research/malicious-ai-models-undermine-software-supply-chain-security/)
- [PickleScan Zero-Days (JFrog)](https://jfrog.com/blog/unveiling-3-zero-day-vulnerabilities-in-picklescan/)
- [CVE-2026-22778 vLLM RCE](https://www.ox.security/blog/cve-2026-22778-vllm-rce-vulnerability/)
- [CVE-2025-63389 Ollama Auth Bypass](https://github.com/advisories/GHSA-f6mr-38g8-39rg)
- [OWASP LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)

### Cost & Hardware
- [Complete Guide to Running AI Models Locally (Medium)](https://medium.com/@orami98/the-complete-guide-to-running-ai-models-locally-in-2025-hardware-requirements-setup-and-real-3619ad10e28c)
- [LLM Total Cost of Ownership (Ptolemay)](https://www.ptolemay.com/post/llm-total-cost-of-ownership)
- [Cost-Benefit Analysis of On-Premise LLM Deployment (arXiv)](https://arxiv.org/html/2509.18101v3)
- [AI API Pricing Comparison 2026 (IntuitionLabs)](https://intuitionlabs.ai/articles/ai-api-pricing-comparison-grok-gemini-openai-claude)
- [GGUF Quantization Guide (Local AI Zone)](https://local-ai-zone.github.io/guides/what-is-ai-quantization-q4-k-m-q8-gguf-guide-2025.html)

### Tools & Installation
- [Ollama Docs](https://docs.ollama.com/quickstart)
- [llama.cpp GitHub](https://github.com/ggml-org/llama.cpp)
- [vLLM Quickstart](https://docs.vllm.ai/en/latest/getting_started/quickstart/)
- [LocalAI](https://localai.io/)
- [Open WebUI](https://docs.openwebui.com/getting-started/quick-start/)
- [Continue.dev Ollama Guide](https://docs.continue.dev/guides/ollama-guide)
- [OpenCode Ollama Integration](https://docs.ollama.com/integrations/opencode)
- [Tabby Air-Gapped Deployment](https://www.tabbyml.com/blog/deploy-tabby-in-air-gapped-environment-with-docker)

### Leaderboards
- [HuggingFace Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard)
- [Chatbot Arena / LMSYS](https://lmsys.org/)
- [SWE-bench Leaderboard](https://www.swebench.com/)
- [BigCodeBench Leaderboard](https://bigcode-bench.github.io/)

---

*Research compiled March 2026. Based on arXiv papers, GitHub repositories, benchmark leaderboards, vendor documentation, and web sources.*
