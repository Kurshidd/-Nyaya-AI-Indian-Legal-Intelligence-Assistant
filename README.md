# ⚖️ Nyaya AI — Indian Legal Intelligence Assistant

> A fully local, free, and private Retrieval-Augmented Generation (RAG) system for querying Indian legal documents — built on open-source LLMs with zero API costs.

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-Local%20LLM-black)
![ChromaDB](https://img.shields.io/badge/ChromaDB-Vector%20Store-purple)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📖 Overview

**Nyaya AI** ("Nyaya" meaning *justice* in Sanskrit/Hindi) is an AI-powered legal assistant that lets you ask natural-language questions about Indian law and get accurate, citation-backed answers — instantly, offline, and for free.

Unlike cloud-based legal AI tools, Nyaya AI runs **100% on your local machine**. No API keys, no subscription, no data leaving your computer.

### Why I built this

Legal research in India is often locked behind dense PDFs, paywalled databases, or expensive software. Nyaya AI demonstrates how a complete, production-style RAG pipeline can be built using only free and open-source tools — making legal information more accessible to students, researchers, and citizens.

---

## ✨ Features

- 🔍 **Semantic Search** — Finds the most relevant legal sections using vector embeddings, not just keyword matching
- ⚖️ **Smart Routing** — Automatically detects legal vs. general queries and routes them appropriately
- 📄 **Source Citations** — Every database-backed answer cites the exact section and document it came from
- 🧠 **Visible Reasoning** — Shows the model's thinking process before the final answer (Qwen3 reasoning traces)
- 🎙️ **Voice Input** — Ask questions by speaking instead of typing
- 🔒 **Fully Local & Private** — No data ever leaves your machine; no internet required after setup
- 💰 **Zero Cost** — Built entirely on free, open-source models and tools
- 🎨 **Clean, Custom UI** — Polished dark-themed interface built with Streamlit

---

## 🏗️ Architecture

This project follows a classic three-stage RAG pipeline:

```
┌─────────────────────────────────────────────────────────────┐
│  STEP 1 — DATA INGESTION                                    │
│  PDF Documents → Parse → Chunk → Embed → Store in ChromaDB  │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  STEP 2 — RETRIEVAL                                          │
│  User Query → Embed → Similarity Search → Top-K Results     │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  STEP 3 — GENERATION                                         │
│  Retrieved Context + Query → LLM → Cited, Structured Answer │
└─────────────────────────────────────────────────────────────┘
```

### Tech Stack

| Component         | Technology                          |
|--------------------|--------------------------------------|
| PDF Parsing        | PyMuPDF (fitz)                      |
| Text Chunking      | LangChain RecursiveCharacterTextSplitter |
| Embedding Model    | `nomic-embed-text` (via Ollama)     |
| Vector Database    | ChromaDB (persistent, local)        |
| LLM (Generation)   | `qwen3:1.7b` (via Ollama)           |
| Web Framework      | Streamlit                           |
| Voice Input        | SpeechRecognition                   |

---

## 📚 Indexed Legal Documents

- Bhartiya Nyaya Sanhita (BNS) 2023
- Constitution of India
- *(extensible — add any legal PDF to expand the knowledge base)*

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com) installed and running
- ~6 GB free disk space (for models)

*********** important *********

*** first option
First do this work, make these three zip files into one zip file, then unzip it, then you will have a proper file.

*** second option
Extract these three zip files and then add the entire file and you will get a proper full file.

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/nyaya-ai.git
cd nyaya-ai
```

### 2. Create a virtual environment

```bash
cd legal_rag

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Pull required Ollama models

```bash
ollama pull nomic-embed-text
ollama pull qwen3:1.7b
```

### 5. Add your legal PDFs

Place PDF files inside `data/raw/`:

```
data/raw/
├── bns_2023.pdf
└── constitution_of_india.pdf
```

### 6. Run the ingestion pipeline

```bash
python main_step1.py
```

This parses, chunks, embeds, and stores your documents in ChromaDB (`vectorstore/`).

### 7. Launch the app

```bash
streamlit run app.py
```

Open your browser at `http://localhost:8501` 🎉

---

## 📁 Project Structure

```
nyaya-ai/
├── data/
│   └── raw/                  # Source legal PDFs
├── vectorstore/               # ChromaDB persistent storage
├── step1_ingestion/
│   ├── parser.py              # PDF text extraction
│   ├── chunker.py              # Text splitting
│   └── embedder.py             # Embedding + storage
├── step2_retrieval/
│   └── retriever.py            # Vector similarity search
├── step3_generation/
│   └── generator.py             # LLM answer generation
├── step4_ui/
│   └── voice.py                  # Voice input handling
├── main_step1.py               # Ingestion pipeline entrypoint
├── main_step2.py               # Retrieval test script
├── main_step3.py               # CLI chat interface
├── app.py                      # Streamlit web application
├── requirements.txt
└── README.md
```

---

## 🧠 How It Works

1. **Ingestion** — Legal PDFs are parsed page-by-page, split into ~800-character overlapping chunks, and embedded using `nomic-embed-text`. Vectors are persisted in ChromaDB.
2. **Query Routing** — Incoming questions are classified as *legal* or *general* using keyword heuristics.
3. **Retrieval** — For legal queries, the question is embedded and compared against stored vectors using cosine similarity. Top-K most relevant chunks are retrieved.
4. **Relevance Gating** — If the best match score is below a threshold (0.65), the system falls back to the LLM's own general legal knowledge instead of forcing a weak database answer.
5. **Generation** — Qwen3 1.7B receives the retrieved context and generates a structured answer with section citations. Its `<think>` reasoning trace is parsed and displayed separately from the final answer.

---

## 🛣️ Roadmap

- [ ] Add BNSS 2023, BSA 2023, CPC, and other key statutes
- [ ] Integrate Indian Kanoon for real case law citations
- [ ] Legal document drafting (bail applications, notices)
- [ ] PDF export of generated answers
- [ ] Multi-document comparison view

---

## ⚠️ Disclaimer

Nyaya AI is an educational and research tool. It is **not a substitute for professional legal advice**. Always consult a qualified advocate for actual legal matters. Answers are generated by an AI model and may contain errors or omissions.

---

## 🤝 Contributing

Contributions are welcome! Feel free to open an issue or submit a PR for:
- Additional legal document support
- UI/UX improvements
- Bug fixes
- New features from the roadmap

---


---

## 👤 Author

**Khurshid Shaikh**
AI/ML Engineer · Mumbai, India

Built as a demonstration of a complete, production-style local RAG system — from raw PDFs to a polished, citation-backed chat interface — using entirely free and open-source tools.

---

<p align="center">Made with ⚖️ in India</p>
