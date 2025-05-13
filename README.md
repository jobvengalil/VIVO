ViVO: On‑Premise RAG Streamlit App

This repository contains a Streamlit-based Retrieval-Augmented Generation (RAG) application, designed to run entirely on-premise. Users can upload documents (PDFs, text files, etc.), ingest them into a vector store, and chat with a local LLM to get context-aware responses based solely on the uploaded content.

Features

Document Ingestion: Upload and ingest multiple documents (PDFs) via Streamlit UI.

Vector Store: Uses ChromaDB to store embeddings and perform similarity search.

Embedding & LLM Backend: Generates embeddings and responses using Ollama model endpoints.

Interactive Chat: Streamlit chat interface with session state management.

File Upload & Temp Storage: Handles temporary files securely and deletes them after ingestion.

Prerequisites

Python 3.10 or higher

Streamlit

ChromaDB

Ollama endpoint accessible locally for both embeddings and chat

Installation

Clone the repository

git clone https://github.com/your-org/vivo-rag-app.git
cd vivo-rag-app

Create and activate a virtual environment

python -m venv .venv
source .venv/bin/activate   # On Windows: .\.venv\Scripts\activate

Install dependencies

pip install -r requirements.txt

Configuration

Certain settings are defined at the top of app.py and rag.py:

Embedding Model: In rag.py, the Ollama embedding model name (e.g., mxbai-embed-large).

Chat Model: In rag.py, the Ollama chat model name (e.g., deepseek-r1:latest).

Chroma Directory: The persistent directory for ChromaDB is set to "chroma_db".

Chunking Parameters:

chunk_size: 1000 tokens

chunk_overlap: 100 tokens

These values can be adjusted directly in the source files.

Running the App

Start the Streamlit server from the command line:

streamlit run app.py

Once launched, open the URL printed in your console (by default, http://localhost:8501) in a browser.

Usage

Upload Documents: Click the file uploader and select one or more PDF files.

Ingest: Press the "Ingest" button to process and embed the documents. Progress is displayed inline.

Chat: Enter your query in the text input and press Enter. The assistant will return answers based on the ingested documents.

Session State: Uploaded documents and chat history persist for the session. To start fresh, restart the Streamlit app.

File Structure

├── app.py             # Streamlit UI and session management
├── rag.py             # RAG logic: ingestion, vector store, and chat
├── requirements.txt   # Python dependencies
├── LICENSE            # Project license
└── README.md          # This documentation

License

This project is licensed under the MIT License.

