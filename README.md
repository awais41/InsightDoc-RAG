# InsightDoc RAG

RAG-based document question-answering for PDF and text files with source-aware retrieval.

InsightDoc lets users upload documents, index them into a vector store, ask natural-language questions, and receive answers grounded in retrieved document context with source metadata.

## Architecture

```text
Documents
→ parsing
→ chunking
→ embeddings
→ ChromaDB

Question
→ query embedding
→ similarity search
→ relevant chunks
→ LLM generation
→ grounded answer with sources
```

## Tech Stack

| Layer | Technology |
| --- | --- |
| Embeddings | Sentence Transformers (`all-MiniLM-L6-v2`) |
| Vector store | ChromaDB |
| LLM access | OpenRouter-compatible API |
| Interface | Streamlit |
| Document parsing | PyPDF |

## Project Structure

```text
.
├── app.py
├── modules/
│   ├── document_loader.py
│   ├── vector_store.py
│   └── rag_brain.py
├── requirements.txt
├── .env.example
└── README.md
```

## How It Works

1. PDF and text files are parsed into document content.
2. Content is divided into overlapping chunks to preserve local context.
3. Each chunk is converted into an embedding with `all-MiniLM-L6-v2`.
4. Embeddings and source metadata are stored in ChromaDB.
5. A user question is embedded with the same model.
6. Similarity search retrieves the most relevant chunks.
7. Retrieved context and the question are sent to the configured LLM.
8. The answer is returned together with document source information.

## Run Locally

```bash
python -m venv .venv
```

Activate the environment:

```powershell
.venv\Scripts\activate
```

```bash
# macOS / Linux
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Copy the environment template and set your OpenRouter key:

```powershell
Copy-Item .env.example .env
```

```bash
# macOS / Linux
cp .env.example .env
```

Start the app:

```bash
streamlit run app.py
```

## Engineering Focus

- Retrieval-Augmented Generation
- Semantic search
- Embedding pipelines
- Vector databases
- Source metadata and grounded generation
- Modular Python application design

## Current Scope

The current implementation supports PDF and text documents and retrieves the most relevant chunks for each question. Future improvements can include additional file formats, multi-turn retrieval, reranking, evaluation pipelines, and production deployment.

## Note on Reliability

RAG can reduce unsupported answers by grounding generation in retrieved context, but answer quality still depends on parsing, chunking, retrieval quality, model behavior, and the available source material.
