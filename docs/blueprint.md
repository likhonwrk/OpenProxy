# 🏗 Blueprint: OpenProxy Architecture

OpenProxy is an **OpenAI-compatible proxy server** built on FastAPI.  
It integrates multiple model providers behind a single unified API.

---

## 🔑 Components

### 1. API Layer (`app/api/`)
- Endpoints compatible with OpenAI (chat, completions, embeddings, images)
- Request validation with Pydantic

### 2. Router (`app/core/router.py`)
- Detects if `model="sheikh.md"`
- Calls `pick_best_backend()` to auto-route to provider
- Normalizes response into OpenAI format

### 3. Providers (`app/providers/`)
- Each provider has a client implementing `BaseProvider`
- Examples: `GroqProvider`, `TogetherProvider`, `CohereProvider`, `ReplicateProvider`
- Health check via `.is_available()`

### 4. Strategy
- Provider selection based on:
  - Priority
  - Quota availability
  - Health checks
  - Randomization fallback

---

## ⚡ Flow

1. User sends request → `model="sheikh.md"`
2. Router checks available providers
3. Best provider selected (`pick_best_backend`)
4. Request executed
5. Response normalized → returned as if from `sheikh.md`

---

## 🛡 Benefits

- OpenAI-compatible → drop-in replacement
- Multi-provider failover
- Cost optimization
- Extensible (add new providers easily)