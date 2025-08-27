# 📝 Task Definitions for OpenProxy

Every request maps to a **task type**.  
Tasks determine which providers are eligible, and how routing is applied.

---

## 🔑 Core Tasks

### 1. Chat / Completion
- Input: `messages[]`
- Output: text response
- Providers: Groq, Together, Cohere, Fireworks, HF Inference, Replicate, SambaNova, Cerebras, Novita, Nscale, Fal AI, Featherless, Hyperbolic, Nebius

### 2. Embeddings
- Input: `input[]`
- Output: vector embedding
- Providers: Cohere, OpenAI-compatible, Together, HF Inference

### 3. Text-to-Image
- Input: prompt
- Output: generated image (base64 / URL)
- Providers: Replicate, Stability AI (via HF/Together), Fal AI, Fireworks

### 4. Reranking / Classification
- Input: query + documents
- Output: ranked docs
- Providers: Cohere, Nomic, HF Inference

---

## 🛠 Routing Strategy

- Each task is mapped to a **routing tactic**.
- Tasks use the **meta-model alias `sheikh.md`** if no explicit provider is given.
- Router picks the best provider according to:
  1. **Availability / Health**
  2. **Latency**
  3. **Cost efficiency**
  4. **Quota / limits**

Example:
```
task=chat → sheikh.md → Groq(llama-3-70b-instruct) if available
```

---

## 📊 Decision Matrix

| Task Type | Primary Providers | Default Model | Fallback Strategy |
|-----------|------------------|---------------|-------------------|
| Chat/Completion | Groq, Together, Cohere | llama-3-70b-instruct | Round-robin |
| Embeddings | Cohere, Together | embed-english-v3.0 | Health-based |
| Text-to-Image | Replicate, Fal AI | stable-diffusion-xl | Cost-optimized |
| Reranking | Cohere, Nomic | rerank-english-v3.0 | Latency-based |

---

## ⚙️ Configuration

Routing rules can be customized via:
- Environment variables
- Provider priority weights
- Health check intervals
- Quota thresholds