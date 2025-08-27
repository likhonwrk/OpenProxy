# 🧞 Meta-Model Spec: `sheikh.md`

`sheikh.md` is the **universal alias model** in OpenProxy.  
It abstracts away providers and models, giving users a single consistent entrypoint.

---

## 🎯 Purpose

- Users only specify:
  ```python
  client.chat.completions.create(model="sheikh.md", messages=[...])
  ```

- OpenProxy automatically chooses the **best backend model**.
- Response is always normalized as if it came from `sheikh.md`.

---

## 🔑 Features

1. **Provider Agnostic**
   - Can map to Groq (LLaMA-3), Together (Mixtral), Cohere (Command R), etc.
   - User doesn't know or care which.

2. **Task-Aware**
   - Chat → LLMs
   - Embeddings → vector models
   - Images → text-to-image models

3. **Stable Output**
   - Always returns OpenAI-style JSON:
     ```json
     {
       "id": "chatcmpl-xyz",
       "object": "chat.completion",
       "model": "sheikh.md",
       "choices": [...]
     }
     ```

4. **Fallback**
   - If one provider fails, automatically retries another.

---

## ✅ Example Usage

```python
resp = client.chat.completions.create(
    model="sheikh.md",
    messages=[{"role": "user", "content": "Tell me a bedtime story"}]
)
print(resp.choices[0].message["content"])
```

---

## 🔄 Routing Logic

When `model="sheikh.md"` is specified:

1. **Task Detection**: Analyze request type (chat, embeddings, images)
2. **Provider Selection**: Choose best available provider based on:
   - Health status
   - Current load
   - Cost efficiency
   - Response time
3. **Model Mapping**: Map to provider-specific model
4. **Response Normalization**: Convert response to OpenAI format with `model="sheikh.md"`

---

## 🎛️ Advanced Features

- **Load Balancing**: Distribute requests across multiple providers
- **Circuit Breaker**: Temporarily disable failing providers
- **Caching**: Cache responses for identical requests
- **Rate Limiting**: Respect provider-specific quotas
- **Monitoring**: Track performance metrics per provider