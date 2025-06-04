# **DeepSeek-RAG Chatbot: ViVO**

The original goal behind this project was to create a self-hosted AI assistant designed for internal use at Vineyard Offshore. It integrates with Microsoft Teams, SharePoint, and Microsoft 365 to streamline knowledge retrieval, support, and process navigation.

🔧 Core Features:
Secure Document Retrieval: Uses RAG (Retrieval-Augmented Generation) or optionally Azure Cognitive Search for answering questions from company documents.

Local LLM: Employs models like DeepSeek or Llama 3.2, hosted locally (no GPT-4 fallback).

Authentication & Access Control: Uses Azure AD to ensure responses are scoped to what the user is authorized to view.

Deployment Targets: Microsoft Teams app and a floating widget on SharePoint.

Backend Stack: FastAPI, local vector database (FAISS/ChromaDB), sentence-transformers for embeddings.


This refactored chatbot enables **fast, accurate, and explainable retrieval of information** from PDFs, DOCX, and TXT files using:

- **DeepSeek-7B**
- **BM25**
- **FAISS**
- **Neural Reranking (Cross-Encoder)**
- **GraphRAG**
- **Chat History Integration**

---

## 🔹 **New Features in This Version**

- **GraphRAG Integration:** Builds a **Knowledge Graph** from documents to enhance **relational** and **contextual** understanding.
- **Chat Memory Awareness:** Maintains ongoing conversation state for **coherent, context-aware** responses.
- **Enhanced Error Handling:** Fixes chat history reset issues and improves overall **user stability**.

---

## ⚙️ **Installation & Setup**

You can run the chatbot in one of two ways:

- **Traditional Python (venv) Installation**
- **Docker-Based Deployment**

---

## 1️⃣ **Traditional Python Installation**

### A. Clone & Set Up Environment

```bash
git clone https://github.com/SaiAkhil066/DeepSeek-RAG-Chatbot.git
cd DeepSeek-RAG-Chatbot

python -m venv venv

# Activate the environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

pip install --upgrade pip  # optional
pip install -r requirements.txt
```

### B. Install & Configure Ollama

1. Download Ollama: [https://ollama.com](https://ollama.com)  
2. Pull the required models:

```bash
ollama pull deepseek-r1:7b
ollama pull nomic-embed-text
```

> Optional: Update the `.env` file to use alternate models via `MODEL` and `EMBEDDINGS_MODEL`.

### C. Run the Application

Ensure Ollama is active:

```bash
ollama serve
```

Start the Streamlit app:

```bash
streamlit run app.py
```

Then visit [http://localhost:8501](http://localhost:8501) to use the chatbot.

---

## 2️⃣ **Docker Installation**

### A. Single-Container (Ollama on Host)

If Ollama is already installed and listening at `localhost:11434`:

```bash
docker-compose build
docker-compose up
```

Access the chatbot via [http://localhost:8501](http://localhost:8501).

### B. Two-Container (Fully Dockerized)

Use this `docker-compose.yml` configuration to containerize both Ollama and the chatbot:

```yaml
version: "3.8"

services:
  ollama:
    image: ghcr.io/jmorganca/ollama:latest
    container_name: ollama
    ports:
      - "11434:11434"

  deepgraph-rag-service:
    container_name: deepgraph-rag-service
    build: .
    ports:
      - "8501:8501"
    environment:
      - OLLAMA_API_URL=http://ollama:11434
      - MODEL=deepseek-r1:7b
      - EMBEDDINGS_MODEL=nomic-embed-text:latest
      - CROSS_ENCODER_MODEL=cross-encoder/ms-marco-MiniLM-L-6-v2
    depends_on:
      - ollama
```

Then run:

```bash
docker-compose build
docker-compose up
```

Navigate to [http://localhost:8501](http://localhost:8501) to begin.

> 💡 For easier debugging and upgrades, start with the single-container setup.

---

## 🧠 **How the Chatbot Works**

1. **Upload Documents:** Supports PDF, DOCX, and TXT files.
2. **Hybrid Retrieval:** Combines BM25 (keyword search) and FAISS (vector similarity) for chunk selection.
3. **GraphRAG:** Builds a knowledge graph to model relationships and structure.
4. **Neural Reranking:** Uses a cross-encoder to reorder retrieved chunks for relevance.
5. **HyDE Query Expansion:** Hypothetically expands queries to improve retrieval scope.
6. **Chat Memory Integration:** Maintains dialogue continuity across multiple turns.
7. **LLM Generation:** DeepSeek-7B produces the final, contextually informed answer.

---

## 🧩 **RAG Settings Explained**

These in-app settings provide fine-tuned control over retrieval and response generation:

### ✅ Enable RAG  
Turns on retrieval-augmented generation. Disabling this bypasses document-based retrieval entirely.

### ✅ Enable HyDE  
Applies **Hypothetical Document Embeddings**—the model generates a hypothetical answer from the query, then embeds it to improve recall.

- Use for abstract or exploratory queries.
- Disable for exact-match or high-precision use cases.

### ✅ Enable Neural Reranking  
Activates a **cross-encoder** model to refine and re-rank the initially retrieved results.

- Improves result quality by analyzing query–chunk relevance jointly.
- May slightly increase latency.

### ✅ Enable GraphRAG  
Constructs and uses a **Knowledge Graph** for contextual retrieval.

- Boosts performance on questions involving entity relationships or multi-hop reasoning.

---

### 🔥 Temperature (`0.00 – 1.00`)

Controls creativity of the LLM:

- **0.1–0.3**: Deterministic, precise answers (recommended for QA and technical use).
- **0.6–1.0**: More diverse or speculative responses (for brainstorming or exploratory tasks).

---

### 📚 Max Contexts (`1 – 5`)

Sets the number of retrieved text chunks used for response generation:

- **1–2**: Fast and focused, better for short queries.
- **3–5**: Comprehensive, suitable for multi-topic or deep-dive queries.

---

### 🧹 Clear Chat History

Resets chat memory, useful when switching topics or troubleshooting context drift.

---

## ✅ **Key Highlights**

- **Runs Offline:** No internet required; all inference and retrieval are local.
- **Multi-Format Support:** Accepts PDF, Word, and text files.
- **Hybrid Search:** Integrates keyword and vector search for best results.
- **Graph-Powered Retrieval:** GraphRAG deepens understanding and linkage.
- **Local LLM + RAG:** Brings full-stack private retrieval to your machine.

---

**Enjoy local, secure, and explainable AI-powered document Q&A with advanced retrieval techniques—all from your own desktop.**  
_The future of retrieval-augmented generation is here—private, powerful, and offline._
