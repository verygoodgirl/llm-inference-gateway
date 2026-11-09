# Self-Hosted Multi-Model AI Gateway

Production configuration and routing layer for a self-hosted AI workspace running **Open WebUI** over **LiteLLM Proxy**.

## Architecture
- **Frontend / Orchestration:** Open WebUI (custom filter & action pipelines, hybrid RAG, TTS chunking)
- **Inference Router:** LiteLLM Proxy with automated fallback chains and prompt-caching optimization
- **Upstream Providers:** Fireworks AI Serverless (AMD Instinct MI300X/MI350X), Moonshot AI (Kimi K3 / K3.1 Pro), DeepSeek V4.1, Qwen 3.8

## Routing Strategy
1. **Primary Reasoning & Vision:** `fireworks_ai/accounts/fireworks/models/kimi-k3-spot` with automatic 503/429 failover to standard serverless `kimi-k3` and `kimi-k3p1-pro`.
2. **Low-Latency Auxiliary Tasks (Title/Tag/Query Generation):** `fireworks_ai/accounts/fireworks/models/deepseek-v4p1-flash-1017-spot`.
3. **Embeddings & Reranking:** `qwen3-embedding-4b` and `qwen3-reranker-4b` via OpenAI-compatible serverless endpoints.
