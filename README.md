<p align="center">
<b>Production-style AI system demonstrating grounded LLM responses using retrieval instead of hallucination.</b>
</p>

<h1 align="center">🌾 FarmChatBot</h1>
<h3 align="center">Multilingual Agricultural Advisory System (RAG + Evaluation Framework)</h3>

---

## Overview

FarmChatBot is a **Retrieval Augmented Generation (RAG) system** designed to deliver reliable agricultural recommendations in low-resource language settings.

The project emphasizes **grounded AI responses, evaluation, and system design** rather than only chatbot generation.

Instead of letting a model guess answers, the system retrieves verified information and forces the model to respond only using evidence.

---

## Why this project exists

In rural environments, farmers often rely on:

- fragmented information sources  
- unverified advice  
- language barriers  
- delayed access to experts  

Generic conversational AI systems can hallucinate and produce unsafe recommendations.

**Goal**

> Build an assistant that answers only when verified context exists.

FarmChatBot combines knowledge retrieval, real-time web context, multilingual access, and automated evaluation.

---

## What the system does

Given a user query (Hindi/English, audio/text), the system:

1. Understands the question  
2. Retrieves relevant agricultural knowledge  
3. Optionally pulls real-time web context  
4. Generates a grounded answer  
5. Translates to the user’s language  
6. Automatically evaluates response quality

---

## System Architecture

### Knowledge Layer

**Static Knowledge Base**
- ~174,000 agricultural Q/A pairs
- Embeddings: `all-MiniLM-L6-v2`
- Vector database: ChromaDB

**Dynamic Knowledge**
- Live context retrieved using n8n workflow
- Web extraction using Trafilatura
- Stored as searchable embeddings

---

### Retrieval (RAG Core)

For each query:

- embed the query
- retrieve Top-K passages
- optionally fetch fresh web data
- merge context and pass to model

The assistant is instructed **not to answer without evidence**.

---

### Grounded Generation

Model: `Llama-4-Scout-17B-Instruct` (Groq API)

Constraint:
- Answer only using retrieved context
- If context is insufficient → return uncertainty

---

### Multilingual Pipeline

Input:
- Hindi speech → speech-to-text
- Hindi text → translated to English

Processing:
- retrieval and reasoning in English

Output:
- English → Hindi translation
- optional Hindi text-to-speech

---

## Evaluation Framework

The system evaluates responses automatically.

| Metric | Purpose |
|------|------|
| BLEU | lexical overlap |
| ROUGE-L | sequence similarity |
| Precision / Recall / F1 | keyword coverage |
| Cosine Similarity | semantic similarity |

This treats the chatbot as an **engineered system**, not a demo.

---

## Repository Structure

```text
farmchatbot/
├── farmBot_Final.ipynb        # main pipeline
├── RAG_test(web+qa).ipynb     # evaluation + retrieval
├── My_workflow_3.json         # n8n automation workflow
├── questionsv4.csv            # dataset
├── evaluated_out_ref2.csv     # scored outputs
└── README.md
```

---

## Key Concepts Demonstrated

- Retrieval Augmented Generation (RAG)
- Vector database search
- Grounded generation to reduce hallucinations
- Multilingual interface design
- Automatic evaluation of LLM outputs

---

## Future Improvements

- Confidence scoring before answering
- Conversation memory
- WhatsApp / Telegram integration
- Support for additional Indian languages

---

## Takeaway
This project demonstrates a **retrieval-first advisory pipeline** where evidence precedes generation and model outputs are systematically evaluated for reliability.
This project demonstrates a **retrieval-first advisory pipeline** where evidence precedes generation and model outputs are systematically evaluated for reliability.
