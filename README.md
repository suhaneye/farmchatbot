# 🌾 FarmChatBot — Multilingual Agricultural Advisory System (RAG + Evaluation Framework)

A production-style Retrieval Augmented Generation (RAG) system designed to deliver reliable agricultural recommendations in low-resource language settings.  
The project focuses on grounded AI responses, evaluation, and system design rather than only chatbot generation.

---

## Why this project exists

In rural environments, farmers often rely on fragmented or unverified information and may not have direct access to experts.  
Generic conversational AI systems often hallucinate and produce unsafe recommendations.

This project explores a practical goal:

> Build an assistant that answers only when it has verified context.

FarmChatBot combines knowledge retrieval, real-time web context, multilingual access, and automated evaluation.  
The objective is decision support, not casual conversation.

---

## What the system does

The system accepts a farmer query (Hindi or English, audio or text) and:

1. Understands the question
2. Retrieves relevant agricultural knowledge
3. Optionally pulls real-time web context
4. Generates an answer grounded in retrieved evidence
5. Translates the response back to the user's language
6. Evaluates the answer quality

---

## System Architecture

### 1) Knowledge Layer

Two independent knowledge sources are used.

**Static Knowledge Base**
- ~174,000 agricultural Q/A pairs
- Embeddings generated using `all-MiniLM-L6-v2`
- Stored in ChromaDB vector index (`qa_context`)

**Dynamic Knowledge (Live Context)**
- Automated retrieval via n8n workflow
- Web extraction using Trafilatura
- Embedded and stored as `web_context`

This allows the system to answer both general and current agricultural queries.

---

### 2) Retrieval (RAG Core)

For each query:
- The query is embedded
- Top-k relevant documents are retrieved from the vector store
- Optional live web documents are fetched
- Context is merged and truncated to model limits

The assistant is instructed not to answer without supporting context.

---

### 3) Grounded Generation

Model: `Llama-4-Scout-17B-Instruct` (via Groq API)

Prompt constraints:
- answer only using retrieved context
- if context is insufficient, respond with uncertainty

The language model is used as a reasoning layer rather than a knowledge source.

---

### 4) Multilingual Pipeline

Input:
- Hindi speech → speech-to-text
- Hindi text → translated to English

Processing:
- retrieval and reasoning occur in English

Output:
- English answer → Hindi translation
- optional Hindi text-to-speech

---

## Evaluation Framework

Responses are evaluated against reference answers using:

| Metric | Purpose |
|-------|------|
| BLEU | lexical similarity |
| ROUGE-L | sequence similarity |
| Precision / Recall / F1 | keyword coverage |
| Cosine Similarity | semantic similarity |

The system is treated as a measurable pipeline rather than only a demo chatbot.

---
farmchatbot/
├── farmBot_Final.ipynb # main pipeline
├── RAG_test(web+qa).ipynb # retrieval and evaluation testing
├── My_workflow_3.json # n8n workflow automation
├── questionsv4.csv # dataset
├── evaluated_out_ref2.csv # evaluation outputs
└── README.md

---

## How to Run (high-level)

1. Load or prepare embeddings in ChromaDB
2. (Optional) start n8n workflow for live context retrieval
3. Run `farmBot_Final.ipynb`
4. (Optional) run `RAG_test(web+qa).ipynb` for evaluation

---

## Key Ideas Demonstrated

- Retrieval Augmented Generation (RAG)
- Vector database search
- Grounded generation to reduce hallucinations
- Multilingual system interface
- Automated evaluation of model outputs

---

## Future Improvements

- Confidence scoring before answering
- Conversation memory
- Messaging integration (WhatsApp/Telegram)
- Support for additional Indian languages

---

## Takeaway

This project demonstrates a retrieval-first advisory pipeline where evidence precedes generation and responses are evaluated for reliability.

## Repository Contents

```text
farmchatbot/
├── farmBot_Final.ipynb        # main pipeline
├── RAG_test(web+qa).ipynb     # evaluation + retrieval
├── My_workflow_3.json         # n8n automation workflow
├── questionsv4.csv            # dataset
├── evaluated_out_ref2.csv     # scored outputs
└── README.md
```


