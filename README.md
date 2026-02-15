# 🌾 FarmChatBot: A Multilingual RAG-based Agricultural Chatbot with Evaluation Pipeline

## 📋 Overview

FarmChatBot is a Retrieval-Augmented Generation (RAG) chatbot designed to provide accurate, trustworthy, and specific agricultural advice. It leverages large language models (LLMs) served via the Groq API and is evaluated using standard NLP metrics such as BLEU, ROUGE, Precision, Recall, F1-score, and Cosine Similarity.

FarmBot supports **multilingual input and output**, enabling farmers and users to interact using **Hindi or English audio/text**, while maintaining high-quality retrieval and generation using English language models.

---

## 🏗️ Architecture

### 1. 🗂️ Knowledge Base Construction (QA + Web)

- **QA Vector Store:** We embed and store ~174,000 question-answer pairs using `all-MiniLM-L6-v2` in ChromaDB (`qa_context`).
- **Live Web Context via n8n:** Real-time data is fetched from trusted agricultural sources using **n8n workflow** → programmable Google Search → scraping content → embedding and storing in `web_context`.
- **Trafilatura** is used for clean content extraction from noisy HTML.

### 2. 🔁 RAG Retrieval Process

- User's query is embedded and used to:
  - Retrieve top-k similar passages from the static **QA knowledge base**
  - Dynamically fetch real-time context from **live web scraping**
- The top results from both are **combined** and truncated to fit model limits.

### 3. 🤖 Groq LLM Generation

- The LLM is instructed to respond strictly using retrieved context only.
- Model used: `meta-llama/llama-4-scout-17b-16e-instruct` (via Groq API).

---

## 🌐 Multilingual Support

### Input:
- 🎙️ Hindi audio → 📝 Hindi text (via speech-to-text)
- 📝 Hindi text → 📝 English text (via translation)

### Core:
- English query used for embedding, RAG retrieval, and Groq LLM answering.

### Output:
- 📝 English text → 📝 Hindi translation
- 📝 Hindi text → 🔈 Hindi audio (via text-to-speech)

This ensures a seamless experience for non-English speakers while preserving model quality through English-based reasoning.

---

## 📏 Evaluation Pipeline

### Metrics Used:

- **BLEU Score**
- **ROUGE-1 & ROUGE-L**
- **Precision, Recall, F1** (based on keyword extraction)
- **Cosine Similarity** (between reference and generated answer embeddings)

### Improvements Implemented:

- ✅ Automatic keyword extraction (coming soon) from reference answers for dynamic precision/recall scoring.
- ✅ Embedding similarity checks to validate closeness beyond lexical overlap.
- ✅ GPT-style evaluation (LLM-generated scores) used optionally for subjective quality checks.

---

## 🔬 Vector Store Details

ChromaDB is used to store two primary collections:

- `qa_context`: Question–Answer data (preloaded)
- `web_context`: Real-time context fetched using n8n & Google CSE

Each document includes:
- Cleaned passage content
- Embeddings (`all-MiniLM-L6-v2`)
- Metadata: URL, original query, etc.

---

## 📁 Notebooks

- `farmBot_Final.ipynb`: Main chatbot pipeline using QA context.
- `RAG_test(web+qa).ipynb`: Evaluation + dynamic web retrieval + full scoring.
- `evaluated_out_ref2.csv`: Contains final scored outputs from evaluation pipeline.

---

## 🧪 How to Run

1. Store QA dataset as embeddings in ChromaDB.
2. Launch n8n webhook for real-time web scraping.
3. Run chatbot pipeline with combined context retrieval (QA + Web).
4. Evaluate responses using BLEU, ROUGE, F1, and cosine similarity.

---

## 💡 Future Work

- 🧠 Memory-based chat history
- 📲 Telegram/WhatsApp integration
- 🧾 Doctor-consultation fallback if context confidence is low
- 🌍 Support for more Indian languages beyond Hindi

---

## 🤝 Credits

This project is inspired by the implementation of RAG in maternal care systems and leverages the following:
- [ChromaDB](https://www.trychroma.com/)
- [n8n](https://n8n.io/)
- [Trafilatura](https://trafilatura.readthedocs.io/)
- [Groq LLM](https://console.groq.com/)
- [NLTK, Sklearn, HuggingFace](https://huggingface.co/)

---
