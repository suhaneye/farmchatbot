# 🌾 FarmChatBot — Multilingual Agricultural Advisory System (RAG + Evaluation Framework)

A production-style Retrieval Augmented Generation (RAG) system designed to deliver reliable agricultural recommendations in low-resource language settings.  
The project focuses on **grounded AI responses**, evaluation, and deployment thinking rather than only chatbot generation.

---

## Why this project exists

In rural environments, farmers often rely on:
- fragmented information sources
- unverified advice
- language barriers
- delayed access to experts

Generic chatbots can hallucinate and produce unsafe recommendations.

This project explores a practical goal:

> *Build an assistant that answers only when it has verified context.*

FarmChatBot combines:
- knowledge retrieval
- real-time web context
- multilingual interface
- automated evaluation

The goal is **decision support**, not conversation.

---

## What the system does

The system accepts a farmer query (Hindi/English, audio/text) and:

1. Understands the question  
2. Retrieves relevant agricultural knowledge  
3. Pulls real-time web context (optional)  
4. Generates an answer **grounded in retrieved evidence**  
5. Translates back to the user’s language  
6. Evaluates answer quality automatically

---

## System Architecture

### 1) Knowledge Layer

Two sources are used:

**Static Knowledge Base**
- ~174,000 agricultural Q/A pairs  
- Embeddings: `all-MiniLM-L6-v2`  
- Vector store: ChromaDB (`qa_context`)

**Dynamic Knowledge (Live Context)**
- Automated retrieval via n8n workflow  
- Web extraction via Trafilatura  
- Embedded + stored as `web_context`

---

### 2) Retrieval (RAG Core)

For each query:
- embed the query
- retrieve Top-K passages from QA vector store
- optionally fetch fresh web context
- merge + truncate context for model limits

The assistant is instructed to **not answer without context**.

---

### 3) Grounded Generation

Model: `Llama-4-Scout-17B-Instruct` (via Groq API)

Prompt constraint:
- answer using provided context only  
- if insufficient context → respond with uncertainty

---

### 4) Multilingual Pipeline

Input:
- Hindi speech → Speech-to-Text  
- Hindi text → translated to English

Core:
- retrieval + reasoning in English

Output:
- English answer → Hindi translation  
- optional Hindi Text-to-Speech

---

## Evaluation Framework

Responses are evaluated against references using:

| Metric | What it checks |
|---|---|
| BLEU | lexical overlap |
| ROUGE-L | sequence similarity |
| Precision / Recall / F1 | keyword coverage |
| Cosine Similarity | semantic similarity |

This treats the chatbot as a **system to be measured**, not just demoed.

---

## Repository Contents

```text
farmchatbot/
├── farmBot_Final.ipynb        # main pipeline
├── RAG_test(web+qa).ipynb     # evaluation & web+QA retrieval
├── My_workflow_3.json         # n8n automation workflow
├── questionsv4.csv            # dataset
├── evaluated_out_ref2.csv     # scored outputs
└── README.md
How to Run (high-level)

Load/prepare embeddings in ChromaDB

Start n8n workflow (optional) for live web context

Run farmBot_Final.ipynb

(Optional) run RAG_test(web+qa).ipynb for evaluation

Key Ideas Demonstrated

Retrieval Augmented Generation (RAG)

Vector search with ChromaDB

Grounded generation to reduce hallucinations

Multilingual interface design

Automated evaluation of LLM outputs

Future Improvements

Confidence scoring before answering

Memory / conversation history

WhatsApp / Telegram integration

More Indian language support

Takeaway

This is not just a chatbot demo — it is a retrieval-first advisory pipeline where evidence comes before generation, and quality is evaluated systematically.
