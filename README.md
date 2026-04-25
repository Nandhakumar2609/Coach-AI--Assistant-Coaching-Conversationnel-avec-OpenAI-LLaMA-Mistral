# 🤖 Coaching Assistant — Full PoC

> An AI-powered coaching assistant for managers, built with OpenAI GPT-4, LangChain, FAISS, and Flask.  
> Built by **Incenteev** — used by teams at P&G, Orange, Société Générale, LG, and Groupama.

---

## 📸 Preview

A full-stack conversational coaching app running inside a Jupyter notebook:

- **Sidebar** with 6 manager profiles (directive / non-directive / balanced styles)
- **RAG pipeline** — FAISS vector search over 25 coaching documents
- **Quality scoring** — GPT-4 judges each response on 5 dimensions
- **Flask web server** — beautiful dark UI served at `http://127.0.0.1:PORT`

---

## 🗂 Project Structure

```
coaching_assistant.ipynb   ← Main notebook (run top to bottom)
coaching_assistant.html    ← Standalone HTML demo (browser-only, uses OpenAI key)
README.md                  ← This file
```

---

## ⚙️ Stack

| Layer | Technology |
|---|---|
| LLM | OpenAI GPT-4o-mini |
| Embeddings | text-embedding-3-small |
| Vector search | FAISS (faiss-cpu) |
| Web server | Flask |
| API layer | FastAPI + Uvicorn |
| Notebook | Jupyter / JupyterLab |
| Scoring | GPT-4 as judge |
| Profiles | Pydantic dataclasses |

---

## 🚀 Quickstart

### 1. Clone the repo

```bash
git clone https://github.com/your-username/coaching-assistant.git
cd coaching-assistant
```

### 2. Install dependencies

```bash
pip install openai langchain langchain-openai langchain-community faiss-cpu \
            fastapi uvicorn pydantic python-dotenv flask ipywidgets tiktoken \
            pypdf httpx nest-asyncio
```

### 3. Set your OpenAI API key

Open `coaching_assistant.ipynb` and in **Phase 1**, replace:

```python
OPENAI_API_KEY = "sk-proj-..."   # ← paste your key here
```

Or use a `.env` file:

```
OPENAI_API_KEY=sk-proj-...
```

### 4. Run the notebook top to bottom

Run all cells in order: **Phase 0 → Phase 8**.

Phase 8 starts a Flask server and prints a clickable link:

```
✅ Coaching Assistant live at http://127.0.0.1:7860
```

Open that URL in your browser.

---

## 📋 Phases

| Phase | What it does |
|---|---|
| **0** | Install all dependencies |
| **1** | Config + OpenAI baseline chat |
| **2** | Knowledge base (25 docs) + FAISS index |
| **3** | Multi-LLM router (OpenAI / Ollama / Mistral) |
| **4** | Intelligent response scorer (GPT-4 as judge) |
| **5** | User profiles + personalization (25 synthetic managers) |
| **6** | Core coaching engine (RAG + Profile + Scorer) |
| **7** | FastAPI REST API (port 8000) |
| **8** | Flask web UI (port 7860) |

---

## 👤 Profiles

The app ships with 6 demo profiles selectable in the sidebar:

| Name | Company | Style |
|---|---|---|
| Alice Martin | P&G | Non-directive |
| Mohammed Al-Rashid | Orange | Directive |
| Marie Fontaine | Société Générale | Directive |
| Ji-ho Kim | LG | Directive |
| Isabelle Moreau | Groupama | Balanced |
| Priya Nair | Capgemini | Non-directive |

---

## 🧠 How the RAG pipeline works

```
User message
     │
     ▼
embed_texts()          ← OpenAI text-embedding-3-small
     │
     ▼
FAISS.search(k=3)      ← cosine similarity over 25 coaching docs
     │
     ▼
System prompt          ← profile + style + retrieved context
     │
     ▼
chat_openai()          ← GPT-4o-mini response
     │
     ▼
score_response()       ← optional GPT-4 quality score (1-5 per dimension)
```

---

## 🌐 Standalone HTML demo

`coaching_assistant.html` is a fully self-contained browser app.  
Open it directly — no server needed. It calls OpenAI from your browser using your API key (entered via a modal on first load).

---

## 🔧 Customization

| What | Where |
|---|---|
| Change LLM provider | `ACTIVE_MODEL` in Phase 1 |
| Add coaching documents | `COACHING_DOCUMENTS` list in Phase 2, then re-run `build_faiss_index()` |
| Add user profiles | `USER_PROFILES` list in Phase 5 |
| Change port | `find_free_port(start=7860)` in Phase 8 |
| API docs (FastAPI) | `http://127.0.0.1:8000/docs` while Phase 7 is running |

---

## 🛣 Next steps

- Swap `COACHING_DOCS` for real PDFs using `PyPDFLoader`
- Persist FAISS index to disk: `faiss.write_index(index, "coaching.index")`
- Persist profiles with SQLite instead of in-memory dict
- Add LangGraph for multi-step coaching workflows
- Deploy Flask app to Render / Railway / HuggingFace Spaces

---

## 📄 License

MIT — free to use and adapt.
