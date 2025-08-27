# 🔮 OpenProxy

An **OpenAI-compatible AI proxy server** that unifies multiple providers (Groq, Cohere, Together, Replicate, HF Inference, Fireworks, SambaNova, and more) under one universal API.

---

## ✨ Features

- OpenAI-compatible endpoints (`/v1/chat/completions`, `/v1/embeddings`, `/v1/images`)
- Multiple providers integrated behind one API
- Universal meta-model alias: **`sheikh.md`**
- Auto-routing: best provider chosen per request
- Extensible provider system

---

## 🚀 Quick Start

### 1. Clone repo
```bash
git clone https://github.com/sheikh-vegeta/OpenProxy
cd OpenProxy
```

### 2. Install deps
```bash
pip install -r requirements.txt
```

### 3. Run server
```bash
uvicorn app.main:app --reload
```

Server runs at: `http://localhost:8000/v1`

---

## 🔑 Usage

### Python Example

```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="fake-key")

resp = client.chat.completions.create(
    model="sheikh.md",
    messages=[{"role": "user", "content": "Explain AI in 3 bullet points"}]
)

print(resp.choices[0].message["content"])
```

### cURL Example

```bash
curl -X POST "http://localhost:8000/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer fake-key" \
  -d '{
    "model": "sheikh.md",
    "messages": [{"role": "user", "content": "Hello, world!"}]
  }'
```

---

## 🧞 Universal Meta-Model: `sheikh.md`

- One alias across all tasks.
- Auto-routes to Groq, Cohere, Together, Fireworks, etc.
- Always normalized to OpenAI API format.

Example:

```python
client.chat.completions.create(model="sheikh.md", messages=[...])
```

---

## 📂 Project Structure

```
app/
  api/          # OpenAI-style endpoints
  core/         # router + logic
  providers/    # provider integrations
  models/       # Pydantic schemas
docs/
  blueprint.md  # architecture
  task.md       # task definitions
  sheikh.md     # meta-model spec
```

---

## 🛡 Providers Supported

- **Groq** (LLaMA-3)
- **Together** (Mixtral, LLaMA-3)
- **Cohere** (Command R)
- **Fireworks**
- **HuggingFace Inference**
- **Replicate**
- **SambaNova**
- **Cerebras**
- **Fal AI**
- **Featherless**
- **Hyperbolic**
- **Nebius**
- **Novita**
- **Nscale**

---

## 🔧 Configuration

Set environment variables for provider API keys:

```bash
export GROQ_API_KEY="your-groq-key"
export TOGETHER_API_KEY="your-together-key"
export COHERE_API_KEY="your-cohere-key"
# ... etc
```

---

## 🛠 Roadmap

- [x] Chat completion proxy
- [x] Multi-provider routing
- [x] sheikh.md meta-model
- [ ] Embeddings proxy
- [ ] Text-to-image proxy
- [ ] Dynamic quota-aware routing
- [ ] WebUI dashboard
- [ ] Streaming support
- [ ] Function calling

---

## 📖 Documentation

- [Architecture Blueprint](docs/blueprint.md)
- [Task Definitions](docs/task.md)
- [Meta-Model Spec](docs/sheikh.md)

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Built with ❤️ for the AI community. Special thanks to all the provider APIs that make this possible.